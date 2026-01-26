# Domain Knowledge Patterns for Skills

Patterns for designing skills that primarily convey specialized knowledge, schemas, conventions, and domain expertise.

## When to Use This Reference

Load this when creating skills that:
- Teach domain-specific knowledge (finance, legal, healthcare, etc.)
- Document company-specific conventions
- Provide database schemas and relationships
- Explain business logic and rules
- Share organizational policies and procedures
- Convey industry best practices

## Core Domain Knowledge Patterns

### Pattern 1: Schema Documentation

**Use when:** Agent needs to understand data structures, databases, or API schemas.

**Structure:**
```markdown
## [Domain] Schema

### Overview

[Brief description of domain and data model]

### Tables/Collections

**Table/Collection 1: [name]**
- Purpose: [What this stores]
- Key fields:
  - `field1` (type): [Description]
  - `field2` (type): [Description]
- Indexes: [Important indexes]
- Relationships: [Foreign keys, references]

**Table/Collection 2: [name]**
[Same structure...]

### Relationships

```
table1.id → table2.foreign_key_id
table1.user_id → users.id
```

### Common Queries

**Query pattern 1: [Name]**
```sql
[Example query]
```

**Query pattern 2: [Name]**
```sql
[Example query]
```

### Data Conventions

- [Convention 1]
- [Convention 2]
- [Convention 3]
```

**Example - E-commerce Schema:**
```markdown
## E-commerce Database Schema

### Overview

Multi-tenant e-commerce platform. Each customer has isolated data identified by `tenant_id`.

### Core Tables

**users**
- Purpose: User accounts and authentication
- Key fields:
  - `id` (uuid): Primary key
  - `email` (string): Unique email address
  - `tenant_id` (uuid): Foreign key to tenants
  - `role` (enum): 'admin' | 'user' | 'guest'
  - `created_at` (timestamp): Account creation date
- Indexes: email, tenant_id
- Relationships: `tenant_id` → `tenants.id`

**orders**
- Purpose: Customer orders
- Key fields:
  - `id` (uuid): Primary key
  - `user_id` (uuid): Foreign key to users
  - `tenant_id` (uuid): Foreign key to tenants
  - `status` (enum): 'pending' | 'paid' | 'shipped' | 'delivered' | 'cancelled'
  - `total_amount` (decimal): Order total in cents
  - `currency` (string): ISO 4217 currency code (USD, EUR, etc.)
  - `created_at` (timestamp): Order creation
  - `updated_at` (timestamp): Last update
- Indexes: user_id, tenant_id, status, created_at
- Relationships:
  - `user_id` → `users.id`
  - `tenant_id` → `tenants.id`

**order_items**
- Purpose: Line items within orders
- Key fields:
  - `id` (uuid): Primary key
  - `order_id` (uuid): Foreign key to orders
  - `product_id` (uuid): Foreign key to products
  - `quantity` (integer): Item count
  - `unit_price` (decimal): Price per unit in cents
  - `subtotal` (decimal): quantity × unit_price
- Indexes: order_id, product_id
- Relationships:
  - `order_id` → `orders.id`
  - `product_id` → `products.id`

**products**
- Purpose: Product catalog
- Key fields:
  - `id` (uuid): Primary key
  - `tenant_id` (uuid): Foreign key to tenants
  - `sku` (string): Stock keeping unit (unique per tenant)
  - `name` (string): Product name
  - `price` (decimal): Current price in cents
  - `inventory_count` (integer): Available quantity
  - `active` (boolean): Product is active and purchasable
- Indexes: tenant_id, sku, active
- Relationships: `tenant_id` → `tenants.id`

### Relationships Diagram

```
tenants
  ├── users
  │     └── orders
  │           └── order_items
  │                 └── products (via product_id)
  └── products
```

### Common Query Patterns

**Get user's orders with items:**
```sql
SELECT o.id, o.status, o.total_amount, oi.product_id, oi.quantity
FROM orders o
JOIN order_items oi ON o.id = oi.order_id
WHERE o.user_id = ? AND o.tenant_id = ?
ORDER BY o.created_at DESC
```

**Calculate total revenue for tenant:**
```sql
SELECT SUM(total_amount) as revenue
FROM orders
WHERE tenant_id = ?
  AND status IN ('paid', 'shipped', 'delivered')
  AND created_at >= ?
```

**Check product inventory:**
```sql
SELECT id, name, inventory_count
FROM products
WHERE tenant_id = ?
  AND active = true
  AND inventory_count < 10
ORDER BY inventory_count ASC
```

### Data Conventions

- **Amounts:** Always stored in cents (integer) to avoid floating-point errors
- **Tenant isolation:** Every query MUST filter by `tenant_id`
- **Soft deletes:** Use `deleted_at` timestamp, never hard delete orders
- **Status transitions:** Orders flow: pending → paid → shipped → delivered
- **SKUs:** Must be unique within tenant, format: `{category}-{number}` (e.g., `TSHIRT-001`)
```

### Pattern 2: Business Rules and Logic

**Use when:** Agent needs to understand business rules, calculations, or decision logic.

**Structure:**
```markdown
## [Domain] Business Rules

### Rule 1: [Name]

**Definition:** [Clear statement of rule]

**Applies when:** [Conditions]

**Logic:**
1. [Step 1]
2. [Step 2]
3. [Result]

**Examples:**
- [Example scenario 1] → [Outcome]
- [Example scenario 2] → [Outcome]

**Exceptions:**
- [Exception 1]: [How to handle]
- [Exception 2]: [How to handle]

### Rule 2: [Name]
[Same structure...]

### Decision Trees

**Decision: [Name]**

```
IF [condition A]
  THEN [outcome 1]
ELSE IF [condition B]
  THEN [outcome 2]
ELSE
  THEN [default outcome]
```
```

**Example - Pricing Business Rules:**
```markdown
## Pricing Business Rules

### Rule 1: Volume Discount

**Definition:** Customers receive automatic discounts based on order quantity.

**Applies when:** Single product quantity ≥ 10 units

**Logic:**
1. Check quantity of each product in order
2. Apply discount tier:
   - 10-49 units: 5% off
   - 50-99 units: 10% off
   - 100+ units: 15% off
3. Apply discount to that product's subtotal only

**Examples:**
- Order 15 units @ $10 each → $10 × 15 × 0.95 = $142.50 (5% off)
- Order 60 units @ $10 each → $10 × 60 × 0.90 = $540.00 (10% off)
- Order 150 units @ $10 each → $10 × 150 × 0.85 = $1,275.00 (15% off)

**Exceptions:**
- Discounts do NOT stack (only highest applicable discount applies)
- Sale items (marked with `on_sale: true`) excluded from volume discounts
- Discounts calculated per product, not per order total

### Rule 2: Customer Tier Pricing

**Definition:** Premium customers receive tier-based pricing.

**Applies when:** Customer has `tier` attribute set

**Tiers:**
- `bronze`: 2% off all orders
- `silver`: 5% off all orders
- `gold`: 8% off all orders
- `platinum`: 12% off all orders

**Logic:**
1. Check customer's `tier` field
2. Apply tier discount to order subtotal
3. Tier discount applies AFTER volume discounts

**Stacking rules:**
```
Base Price: $1000
Volume Discount (10%): $1000 × 0.90 = $900
Tier Discount (Gold, 8%): $900 × 0.92 = $828
Final Price: $828
```

### Rule 3: Free Shipping Threshold

**Definition:** Orders over threshold amount get free shipping.

**Thresholds by region:**
- US: $50
- Canada: $75
- Europe: €60
- Rest of world: $100

**Logic:**
1. Calculate order subtotal (after all discounts)
2. Check shipping address region
3. If subtotal ≥ threshold for region, set shipping cost to $0
4. Otherwise, calculate shipping based on weight and distance

**Exceptions:**
- Oversized items (over 50 lbs) never qualify for free shipping
- Express shipping excluded from free shipping (customer must choose standard)
- Threshold checked BEFORE taxes

### Decision Tree: Final Price Calculation

```
Start with base price
  ↓
Apply volume discount (if applicable)
  ↓
Apply customer tier discount
  ↓
Calculate subtotal
  ↓
IF subtotal ≥ free shipping threshold AND standard shipping
  THEN shipping = $0
ELSE
  Calculate shipping cost
  ↓
subtotal + shipping = order total (before tax)
  ↓
Calculate tax based on shipping address
  ↓
order total + tax = FINAL PRICE
```

### Validation Rules

**Before accepting order:**
- ✓ All products exist and are active
- ✓ Sufficient inventory for quantities
- ✓ Prices match current product prices (prevent race conditions)
- ✓ Discounts correctly calculated
- ✓ Shipping address valid
- ✓ Payment method valid
```

### Pattern 3: Company Conventions and Standards

**Use when:** Agent needs to follow organization-specific practices.

**Structure:**
```markdown
## [Company/Team] Conventions

### Convention 1: [Name]

**Standard:** [What the convention is]

**Rationale:** [Why we do it this way]

**Implementation:**
- [How to apply it]
- [Tools or patterns to use]

**Examples:**
✅ **Correct:**
[Example]

❌ **Incorrect:**
[Example]

### Convention 2: [Name]
[Same structure...]

### Quick Reference

| Scenario | Convention | Example |
|----------|------------|---------|
| [Scenario 1] | [Convention] | [Example] |
| [Scenario 2] | [Convention] | [Example] |
```

**Example - API Design Conventions:**
```markdown
## API Design Conventions

### Convention 1: Endpoint Naming

**Standard:** Use plural nouns, lowercase, hyphen-separated for multi-word resources.

**Rationale:** Consistency improves API discoverability and reduces cognitive load.

**Implementation:**
- Collection: `/api/v2/resource-names`
- Single item: `/api/v2/resource-names/{id}`
- Nested: `/api/v2/parent-resources/{id}/child-resources`

**Examples:**

✅ **Correct:**
```
GET  /api/v2/users
GET  /api/v2/users/123
GET  /api/v2/users/123/orders
POST /api/v2/user-profiles
```

❌ **Incorrect:**
```
GET  /api/v2/getUsers          # Don't use verbs
GET  /api/v2/user              # Use plural
GET  /api/v2/UserProfiles      # Use lowercase
GET  /api/v2/user_profiles     # Use hyphens, not underscores
```

### Convention 2: Error Responses

**Standard:** All errors return consistent JSON structure with error code and message.

**Response format:**
```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message",
    "details": {
      "field": "Additional context"
    }
  }
}
```

**Error codes:**
- `INVALID_INPUT`: Client sent invalid data (400)
- `UNAUTHORIZED`: Authentication required or failed (401)
- `FORBIDDEN`: Authenticated but insufficient permissions (403)
- `NOT_FOUND`: Resource doesn't exist (404)
- `CONFLICT`: Resource already exists or state conflict (409)
- `RATE_LIMITED`: Too many requests (429)
- `INTERNAL_ERROR`: Server error (500)

**Examples:**

✅ **Correct:**
```json
{
  "error": {
    "code": "INVALID_INPUT",
    "message": "Email address is required",
    "details": {
      "field": "email",
      "value": null
    }
  }
}
```

❌ **Incorrect:**
```json
{
  "error": "Email is required"  # Not structured
}
```

### Convention 3: Authentication

**Standard:** JWT tokens in Authorization header with Bearer scheme.

**Token structure:**
```
Authorization: Bearer <jwt-token>
```

**JWT claims:**
- `sub`: User ID (UUID)
- `email`: User email
- `role`: User role (admin|user|guest)
- `tenant_id`: Tenant ID (UUID)
- `exp`: Expiration timestamp
- `iat`: Issued at timestamp

**Token lifetime:**
- Access token: 15 minutes
- Refresh token: 7 days

**Refresh flow:**
```
POST /api/v2/auth/refresh
Body: { "refresh_token": "..." }
Response: { "access_token": "...", "refresh_token": "..." }
```

### Convention 4: Pagination

**Standard:** Cursor-based pagination for lists.

**Request:**
```
GET /api/v2/users?limit=50&cursor=abc123
```

**Response:**
```json
{
  "data": [...],
  "pagination": {
    "next_cursor": "xyz789",
    "has_more": true
  }
}
```

**Parameters:**
- `limit`: Items per page (default: 25, max: 100)
- `cursor`: Pagination cursor from previous response

**Why cursor-based?**
- Consistent results even when data changes
- Better performance for large datasets
- No "page drift" issues

### Quick Reference

| Scenario | Convention | Example |
|----------|------------|---------|
| List users | GET /api/v2/users | `GET /api/v2/users?limit=50` |
| Get user | GET /api/v2/users/{id} | `GET /api/v2/users/123` |
| Create user | POST /api/v2/users | `POST /api/v2/users` + body |
| Update user | PATCH /api/v2/users/{id} | `PATCH /api/v2/users/123` + body |
| Delete user | DELETE /api/v2/users/{id} | `DELETE /api/v2/users/123` |
| Authenticate | POST /api/v2/auth/login | Returns access + refresh tokens |
| Refresh token | POST /api/v2/auth/refresh | New tokens using refresh token |
```

### Pattern 4: Industry Domain Knowledge

**Use when:** Agent needs specialized industry knowledge.

**Structure:**
```markdown
## [Industry/Domain] Knowledge

### Core Concepts

**Concept 1: [Name]**
- Definition: [What it is]
- Importance: [Why it matters]
- Used when: [Context]

**Concept 2: [Name]**
[Same structure...]

### Common Calculations

**Calculation 1: [Name]**
```
Formula: [mathematical formula]
Example: [concrete example with numbers]
```

**Calculation 2: [Name]**
```
Formula: [mathematical formula]
Example: [concrete example with numbers]
```

### Industry Standards

- [Standard 1]: [Description]
- [Standard 2]: [Description]

### Best Practices

1. [Practice 1]: [Why and how]
2. [Practice 2]: [Why and how]

### Common Pitfalls

- **Pitfall 1:** [What to avoid and why]
- **Pitfall 2:** [What to avoid and why]
```

**Example - Financial Metrics:**
```markdown
## Financial Metrics Knowledge

### Core Concepts

**MRR (Monthly Recurring Revenue)**
- Definition: Predictable revenue expected each month from subscriptions
- Importance: Key metric for SaaS businesses, shows business health
- Used when: Analyzing growth, forecasting, investor reporting

**ARR (Annual Recurring Revenue)**
- Definition: MRR × 12, annualized recurring revenue
- Importance: Industry standard for comparing SaaS companies
- Used when: Valuation, strategic planning, annual reporting

**Churn Rate**
- Definition: Percentage of customers who cancel subscriptions
- Importance: Indicates customer satisfaction and business sustainability
- Used when: Assessing retention, identifying problems, forecasting

**LTV (Lifetime Value)**
- Definition: Total revenue expected from a customer over their lifetime
- Importance: Determines maximum viable customer acquisition cost
- Used when: Marketing budget allocation, product strategy

**CAC (Customer Acquisition Cost)**
- Definition: Total marketing and sales costs divided by new customers acquired
- Importance: Must be significantly less than LTV for profitable growth
- Used when: Marketing ROI analysis, unit economics

### Common Calculations

**MRR Calculation:**
```
MRR = Sum of all monthly subscription values

Example:
- 100 customers @ $50/month = $5,000
- 50 customers @ $100/month = $5,000
- 20 customers @ $500/month = $10,000
Total MRR = $20,000
```

**Churn Rate:**
```
Churn Rate = (Customers Lost / Customers at Start) × 100

Example:
- Start of month: 1,000 customers
- End of month: 950 customers
- Customers lost: 50
Churn Rate = (50 / 1,000) × 100 = 5%
```

**LTV Calculation (Simple):**
```
LTV = ARPU / Churn Rate

Where ARPU = Average Revenue Per User

Example:
- ARPU = $50/month
- Monthly Churn = 5% (0.05)
LTV = $50 / 0.05 = $1,000
```

**LTV:CAC Ratio:**
```
LTV:CAC Ratio = LTV / CAC

Healthy SaaS: 3:1 or better

Example:
- LTV = $1,000
- CAC = $300
LTV:CAC = 1,000 / 300 = 3.33:1  ✓ Healthy
```

### Industry Standards

- **Good LTV:CAC Ratio:** 3:1 or higher
- **Acceptable Churn:** <5% monthly for B2C, <2% monthly for B2B
- **CAC Payback Period:** <12 months preferred
- **Magic Number:** >0.75 (indicates efficient growth)
- **Rule of 40:** Growth Rate + Profit Margin ≥ 40%

### Best Practices

1. **Cohort Analysis:** Track metrics by customer cohort (signup month) to identify trends
2. **Net Revenue Retention:** Include expansion revenue, not just churn
3. **Segment Analysis:** Calculate separately for customer segments (SMB, Mid-market, Enterprise)
4. **Leading Indicators:** Monitor engagement metrics that predict churn
5. **Gross vs Net:** Track both gross and net churn (net includes expansion)

### Common Pitfalls

- **Vanishing cohorts:** Early cohorts may be very different from current customers
- **Ignoring expansion:** Not accounting for upsells/cross-sells in calculations
- **Wrong time period:** Mixing monthly and annual metrics without conversion
- **Incomplete CAC:** Forgetting to include all sales & marketing costs
- **Premature optimization:** Over-optimizing metrics before product-market fit

### SaaS Metrics Dashboard

**Must track:**
- MRR (with breakdown: New, Expansion, Churned)
- Customer count
- ARPU
- Churn rate (customer and revenue)
- LTV:CAC ratio
- Gross margin
- Burn rate (for startups)

**Query patterns in schema:**
```sql
-- MRR calculation
SELECT DATE_TRUNC('month', created_at) as month,
       SUM(plan_price) as mrr
FROM subscriptions
WHERE status = 'active'
GROUP BY month;

-- Churn rate
SELECT
  COUNT(*) FILTER (WHERE status = 'cancelled') /
  COUNT(*) FILTER (WHERE created_at < DATE_TRUNC('month', CURRENT_DATE)) as churn_rate
FROM subscriptions
WHERE DATE_TRUNC('month', cancelled_at) = DATE_TRUNC('month', CURRENT_DATE);
```
```

## Best Practices for Domain Knowledge Skills

### 1. Focus on Non-Obvious Knowledge

**Agent already knows:**
- General programming concepts
- Common algorithms and data structures
- Standard industry practices
- Basic terminology

**Add to skill:**
- Company-specific conventions
- Proprietary schemas
- Custom business logic
- Internal tool usage
- Organizational policies

### 2. Use Clear Examples

**Always include concrete examples:**

❌ **Abstract:**
```markdown
Calculate the discount based on customer tier.
```

✅ **Concrete:**
```markdown
Calculate the discount based on customer tier:
- Bronze (tier_id: 1): 5% off
- Silver (tier_id: 2): 10% off
- Gold (tier_id: 3): 15% off

Example: $100 order, Silver tier → $100 × 0.90 = $90
```

### 3. Explain the "Why"

**Context helps agent make decisions:**

```markdown
## Naming Convention: Snake Case for Database Columns

**Standard:** All database columns use snake_case

**Why:** PostgreSQL folds unquoted identifiers to lowercase, so snake_case
avoids quoting requirements and reduces errors.

❌ camelCase → requires "camelCase" in quotes
❌ PascalCase → requires "PascalCase" in quotes
✅ snake_case → no quotes needed
```

### 4. Organize by Frequency of Use

**Put most common patterns first:**
```markdown
## Database Schema

### Most Common Queries
[90% of queries fall into these patterns]

### Specialized Queries
[10% edge cases]

### Legacy Tables
[Rarely used, kept for backwards compatibility]
```

### 5. Link to Detailed References

**Keep SKILL.md lean:**
```markdown
## Financial Calculations

### Core Metrics
[Brief overview of MRR, ARR, Churn]

For complete formulas and examples, see:
- references/financial-metrics.md - All calculations with examples
- references/saas-benchmarks.md - Industry standards and targets
- references/cohort-analysis.md - Advanced cohort tracking
```

### 6. Include Visual Aids (When Helpful)

**For complex relationships:**
```markdown
## User Permissions Model

```
Organization
  ├── Admin Role (full access)
  │     └── Can: Manage users, view all data, change settings
  ├── Manager Role (team access)
  │     └── Can: View team data, assign tasks, approve requests
  └── Member Role (self access)
        └── Can: View own data, submit requests
```
```

### 7. Update Regularly

**Domain knowledge changes:**
- Schedule quarterly reviews
- Update when business rules change
- Version control and changelog
- Archive outdated patterns

## Testing Domain Knowledge Skills

**Verify skill by asking agent to:**

1. **Explain concepts:** "What is MRR and how is it calculated?"
2. **Apply rules:** "Calculate the final price for this order"
3. **Make decisions:** "Which customer tier should get this discount?"
4. **Query data:** "Show me all orders from gold tier customers"
5. **Validate inputs:** "Is this API request formatted correctly?"

**Agent should:**
- Reference skill content accurately
- Apply rules correctly
- Explain reasoning
- Cite specific sections when answering

## When to Split Domain Knowledge

**Split into multiple skills or files when:**

- Skill exceeds 500 lines
- Covers multiple distinct sub-domains
- Some knowledge rarely used
- Team structure suggests separation

**Example: Split large skill:**

```
Before:
├── finance-skill/
    └── SKILL.md (2000 lines)

After:
├── financial-metrics/
│   └── SKILL.md (400 lines)
├── accounting-rules/
│   └── SKILL.md (350 lines)
└── tax-calculations/
    └── SKILL.md (300 lines)
```

**Or use references:**

```
finance-skill/
├── SKILL.md (300 lines overview)
└── references/
    ├── metrics.md (detailed calculations)
    ├── accounting.md (accounting rules)
    └── tax.md (tax rules)
```
