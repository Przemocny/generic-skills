# Integration Patterns for External Tools and APIs

Patterns for designing skills that integrate with external systems, APIs, databases, and tools.

## When to Use This Reference

Load this when creating skills with:
- REST API integrations
- CLI tool wrappers (gh, gcloud, aws, kubectl, etc.)
- Database connections
- Third-party service integrations (Jira, Salesforce, Slack, etc.)
- Cloud provider interactions
- Authentication/credential handling

## Core Integration Patterns

### Pattern 1: CLI Tool Wrapper

**Use when:** Wrapping existing CLI tools with additional context, error handling, or workflow.

**Structure:**
```markdown
## Tool: [CLI Name]

### Prerequisites
- Tool installed: `which [tool-name]`
- Authentication configured: `[auth-command]`
- Required permissions: [list]

### Common Operations

**Operation 1:**
```bash
[tool-name] [command] [flags]
```

**Operation 2:**
```bash
[tool-name] [command] [flags]
```

### Error Handling

**Error: [Common error]**
- Cause: [Why it happens]
- Solution: [How to fix]

**Error: [Common error]**
- Cause: [Why it happens]
- Solution: [How to fix]
```

**Example - GitHub CLI Wrapper:**
```markdown
## GitHub CLI Skill

### Prerequisites
- GitHub CLI installed: `which gh`
- Authenticated: `gh auth status`
- Repo context: Must be in git repository

### Common Operations

**View Pull Request:**
```bash
gh pr view [number] --json title,body,state,reviews
```

**Create Pull Request:**
```bash
gh pr create --title "Title" --body "Description" --base main
```

**List Issues:**
```bash
gh issue list --state open --limit 50
```

**Add Comment:**
```bash
gh pr comment [number] --body "Comment text"
```

### Error Handling

**Error: "Not in a git repository"**
- Cause: Command run outside git repo
- Solution: `cd` to repository root first

**Error: "Authentication required"**
- Cause: Not logged in to GitHub
- Solution: Run `gh auth login`

**Error: "Resource not found"**
- Cause: PR/issue number doesn't exist
- Solution: Verify number with `gh pr list` or `gh issue list`

### Rate Limiting

GitHub API has rate limits:
- Authenticated: 5,000 requests/hour
- Check remaining: `gh api rate_limit`
- If rate limited, wait or use different auth token
```

### Pattern 2: REST API Integration

**Use when:** Integrating with REST APIs, handling requests/responses.

**Structure:**
```markdown
## API: [Service Name]

### Authentication

**Method:** [API Key | OAuth | JWT | etc.]

**Setup:**
```bash
export API_KEY="your-key-here"
# Or use credential manager
```

**Headers:**
```bash
Authorization: Bearer ${API_KEY}
Content-Type: application/json
```

### Endpoints

**GET [endpoint]**
- Purpose: [What it does]
- Parameters:
  - `param1` (required): [description]
  - `param2` (optional): [description]
- Response: [Format description]
- Example:
  ```bash
  curl -H "Authorization: Bearer $API_KEY" \
    "https://api.example.com/v1/resource?param1=value"
  ```

**POST [endpoint]**
- Purpose: [What it does]
- Body:
  ```json
  {
    "field1": "value",
    "field2": "value"
  }
  ```
- Response: [Format description]
- Example:
  ```bash
  curl -X POST \
    -H "Authorization: Bearer $API_KEY" \
    -H "Content-Type: application/json" \
    -d '{"field1":"value"}' \
    "https://api.example.com/v1/resource"
  ```

### Response Handling

**Success (200):**
```json
{
  "status": "success",
  "data": { ... }
}
```

**Error (4xx):**
```json
{
  "error": "Error message",
  "code": "ERROR_CODE"
}
```

**Error (5xx):**
- Service temporarily unavailable
- Retry with exponential backoff

### Rate Limiting

- Limit: [requests per time period]
- Headers: `X-RateLimit-Remaining`, `X-RateLimit-Reset`
- Strategy: Check headers, pause if needed
```

**Example - Slack API Integration:**
```markdown
## Slack API Skill

### Authentication

**Method:** Bot Token (OAuth)

**Setup:**
1. Create Slack app at api.slack.com/apps
2. Add Bot Token Scopes: `chat:write`, `channels:read`
3. Install to workspace
4. Store token: `export SLACK_BOT_TOKEN="xoxb-..."`

**Headers:**
```bash
Authorization: Bearer ${SLACK_BOT_TOKEN}
Content-Type: application/json
```

### Common Operations

**Post Message:**
```bash
curl -X POST https://slack.com/api/chat.postMessage \
  -H "Authorization: Bearer $SLACK_BOT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "channel": "C1234567890",
    "text": "Message content",
    "blocks": [...]
  }'
```

**List Channels:**
```bash
curl https://slack.com/api/conversations.list \
  -H "Authorization: Bearer $SLACK_BOT_TOKEN"
```

**Upload File:**
```bash
curl -F file=@document.pdf \
  -F "channels=C1234567890" \
  -F "initial_comment=Check this out" \
  -H "Authorization: Bearer $SLACK_BOT_TOKEN" \
  https://slack.com/api/files.upload
```

### Response Format

**Success:**
```json
{
  "ok": true,
  "channel": "C1234567890",
  "ts": "1234567890.123456",
  "message": { ... }
}
```

**Error:**
```json
{
  "ok": false,
  "error": "channel_not_found"
}
```

### Common Errors

- `not_authed`: Token missing or invalid
- `invalid_auth`: Token has been revoked
- `channel_not_found`: Channel ID doesn't exist
- `rate_limited`: Too many requests, wait 1 minute

### Rate Limits

- Tier 1: 1 request per second
- Tier 2: 20 requests per minute
- Tier 3: 50+ requests per minute
- Check `Retry-After` header when rate limited
```

### Pattern 3: Database Query Skill

**Use when:** Skill needs to query databases, handle schemas, generate SQL.

**Structure:**
```markdown
## Database: [Database Name]

### Connection

**Setup:**
```bash
export DB_HOST="hostname"
export DB_USER="username"
export DB_PASSWORD="password"
export DB_NAME="database_name"
```

**Connect:**
```bash
[connection command]
```

### Schema

**Tables:**
- `table1` - [Description]
- `table2` - [Description]

**Relationships:**
```
table1.id → table2.foreign_key_id
```

For complete schema, see references/schema.md

### Common Queries

**Query Type 1:**
```sql
SELECT [fields]
FROM [table]
WHERE [conditions]
```

**Query Type 2:**
```sql
SELECT t1.field, t2.field
FROM table1 t1
JOIN table2 t2 ON t1.id = t2.foreign_id
WHERE [conditions]
```

### Query Guidelines

- Always use parameterized queries (prevent SQL injection)
- Include LIMIT clause for exploratory queries
- Use indexes for performance
- Avoid SELECT * in production queries

### Safety

**NEVER:**
- Concatenate user input into SQL strings
- Run UPDATE/DELETE without WHERE clause
- Expose raw SQL errors to users
```

**Example - BigQuery Skill:**
```markdown
## BigQuery Skill

### Authentication

```bash
gcloud auth application-default login
export GOOGLE_CLOUD_PROJECT="project-id"
```

### Schema

**Dataset: analytics**

**Tables:**
- `users` - User accounts and profiles
- `events` - User activity events
- `sessions` - User session data

**For detailed schemas, see:**
- references/schema-users.md
- references/schema-events.md
- references/schema-sessions.md

### Running Queries

**Using bq CLI:**
```bash
bq query --use_legacy_sql=false '
  SELECT event_name, COUNT(*) as count
  FROM `project.analytics.events`
  WHERE DATE(timestamp) = CURRENT_DATE()
  GROUP BY event_name
  ORDER BY count DESC
'
```

**Using Python:**
```python
from google.cloud import bigquery

client = bigquery.Client()
query = """
    SELECT event_name, COUNT(*) as count
    FROM `project.analytics.events`
    WHERE DATE(timestamp) = CURRENT_DATE()
    GROUP BY event_name
"""
results = client.query(query).result()
```

### Query Patterns

**Daily Active Users:**
```sql
SELECT DATE(timestamp) as date, COUNT(DISTINCT user_id) as dau
FROM `project.analytics.events`
WHERE DATE(timestamp) >= DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)
GROUP BY date
ORDER BY date
```

**Funnel Analysis:**
```sql
WITH step1 AS (
  SELECT DISTINCT user_id
  FROM `project.analytics.events`
  WHERE event_name = 'signup'
),
step2 AS (
  SELECT DISTINCT user_id
  FROM `project.analytics.events`
  WHERE event_name = 'first_purchase'
)
SELECT
  (SELECT COUNT(*) FROM step1) as step1_count,
  (SELECT COUNT(*) FROM step2) as step2_count,
  SAFE_DIVIDE(
    (SELECT COUNT(*) FROM step2),
    (SELECT COUNT(*) FROM step1)
  ) as conversion_rate
```

### Best Practices

- Preview results before running large queries: `--dry_run`
- Use partitioned tables with DATE() filters
- Avoid SELECT * - specify needed columns
- Use LIMIT for exploratory queries
- Check query cost: `--dry_run` shows bytes scanned
```

### Pattern 4: Multi-Service Orchestration

**Use when:** Skill coordinates multiple external services.

**Structure:**
```markdown
## Orchestration: [Workflow Name]

### Services Involved

1. **Service A** - [Role]
2. **Service B** - [Role]
3. **Service C** - [Role]

### Workflow

**Step 1: [Service A Operation]**
```bash
[command]
```
- Capture: [output needed]

**Step 2: [Service B Operation]**
```bash
[command using output from Step 1]
```
- Capture: [output needed]

**Step 3: [Service C Operation]**
```bash
[command using outputs from previous steps]
```

### Error Recovery

**If Step 1 fails:**
- [Recovery action]

**If Step 2 fails:**
- [Rollback Step 1]
- [Recovery action]

**If Step 3 fails:**
- [Rollback Steps 1-2]
- [Recovery action]
```

**Example - Deploy to Cloud with Monitoring:**
```markdown
## Cloud Deployment with Monitoring

### Services Involved

1. **GitHub** - Source code
2. **Google Cloud Build** - Build Docker image
3. **Google Cloud Run** - Deploy service
4. **Datadog** - Register deployment for monitoring

### Workflow

**Step 1: Trigger Cloud Build**
```bash
gcloud builds submit --tag gcr.io/PROJECT/SERVICE:${COMMIT_SHA}
```
- Capture: Build ID, Image URL
- Wait for: Build completion

**Step 2: Deploy to Cloud Run**
```bash
gcloud run deploy SERVICE \
  --image gcr.io/PROJECT/SERVICE:${COMMIT_SHA} \
  --region us-central1 \
  --platform managed
```
- Capture: Service URL
- Wait for: Deployment completion

**Step 3: Register Deployment in Datadog**
```bash
curl -X POST "https://api.datadoghq.com/api/v1/events" \
  -H "DD-API-KEY: ${DATADOG_API_KEY}" \
  -d '{
    "title": "Deployed SERVICE to production",
    "text": "Version: '"${COMMIT_SHA}"'",
    "tags": ["service:SERVICE", "env:production"]
  }'
```

**Step 4: Run Smoke Tests**
```bash
curl -f "${SERVICE_URL}/health" || exit 1
```

### Error Recovery

**If Build fails:**
- Check build logs: `gcloud builds log ${BUILD_ID}`
- Fix code issues
- Retry build

**If Deployment fails:**
- Check Cloud Run logs
- Previous version still running (automatic rollback)
- Fix issues and retry

**If Smoke Tests fail:**
- Rollback: `gcloud run services update-traffic --to-revisions=PREVIOUS=100`
- Investigate logs
- Alert team

### Monitoring

After deployment, check:
- Cloud Run metrics (requests, errors, latency)
- Datadog dashboards
- Log aggregation
- Alert for errors > threshold
```

## Best Practices for Integrations

### 1. Handle Authentication Securely

**✅ Do:**
```markdown
## Authentication

Store credentials securely:
- Use environment variables: `$API_KEY`
- Use credential managers: `gh auth token`
- Use cloud secret managers: `gcloud secrets`

Never hardcode credentials in skill or scripts.
```

**❌ Don't:**
```markdown
API_KEY = "sk-1234567890abcdef"  # Hardcoded secret
```

### 2. Implement Retry Logic

**For transient failures:**
```markdown
## Retry Strategy

For network errors or 5xx responses:
1. Wait 1 second, retry
2. Wait 2 seconds, retry
3. Wait 4 seconds, retry
4. Fail after 3 attempts

For rate limits (429):
- Wait time specified in Retry-After header
- Or exponential backoff: 10s, 30s, 60s
```

### 3. Validate Responses

**Don't assume success:**
```markdown
## Response Validation

After API call:
1. Check HTTP status code
2. Validate response JSON structure
3. Verify expected fields exist
4. Check for error indicators in response body
5. Log response for debugging if unexpected
```

### 4. Provide Clear Error Messages

**Translate technical errors to user-friendly messages:**
```markdown
## Error Mapping

**API Error: "insufficient_scope"**
→ User Message: "Missing permission: 'repo'. Run: gh auth refresh -s repo"

**API Error: "rate_limit_exceeded"**
→ User Message: "Rate limit reached. Available again in 15 minutes. Check limits: gh api rate_limit"

**API Error: "resource_not_found"**
→ User Message: "Repository not found. Verify name and access permissions."
```

### 5. Document Rate Limits

**Always include rate limit information:**
```markdown
## Rate Limits

- **Limit:** 5,000 requests per hour
- **Check remaining:** `curl -I [endpoint]` (see X-RateLimit-Remaining header)
- **Reset time:** Unix timestamp in X-RateLimit-Reset header
- **Strategy:** If < 100 remaining, pause non-critical requests
```

### 6. Handle Pagination

**For APIs with paginated responses:**
```markdown
## Pagination

**Pattern:**
```bash
page=1
while true; do
  response=$(curl "https://api.example.com/items?page=$page")
  # Process response
  [ -z "$(echo $response | jq '.next_page')" ] && break
  ((page++))
done
```

**Limits:**
- Default: 30 items per page
- Max: 100 items per page
- Use `per_page=100` for efficiency
```

### 7. Timeout Configuration

**Prevent hanging on slow APIs:**
```markdown
## Timeouts

**Connection timeout:** 10 seconds
**Read timeout:** 30 seconds

```bash
curl --connect-timeout 10 --max-time 30 [endpoint]
```

For long-running operations, use async patterns and polling.
```

### 8. Cache When Appropriate

**For data that doesn't change frequently:**
```markdown
## Caching Strategy

**Cache these:**
- Reference data (categories, constants)
- User profiles (TTL: 5 minutes)
- Slowly-changing configs

**Don't cache:**
- Real-time data
- User-specific sensitive data
- Frequently-updated resources

**Implementation:**
- Store in /tmp/ with timestamp
- Check age before use
- Refresh if stale
```

## Testing Integration Skills

**Test scenarios:**

1. **Happy Path**
   - All APIs respond successfully
   - Verify correct data flow

2. **Authentication Failures**
   - Missing credentials
   - Invalid credentials
   - Expired credentials

3. **Network Issues**
   - Timeout
   - Connection refused
   - DNS failures

4. **API Errors**
   - 4xx errors (client errors)
   - 5xx errors (server errors)
   - Rate limiting

5. **Malformed Responses**
   - Unexpected JSON structure
   - Missing fields
   - Wrong data types

6. **Partial Failures**
   - Step 1 succeeds, Step 2 fails
   - Test rollback/recovery

7. **Rate Limiting**
   - Simulate hitting rate limit
   - Verify retry logic

## Security Considerations

**For all integration skills:**

1. **Never log sensitive data** (tokens, passwords, PII)
2. **Validate all inputs** before sending to external APIs
3. **Use HTTPS** exclusively (never HTTP)
4. **Rotate credentials** regularly
5. **Principle of least privilege** (minimal permissions)
6. **Audit trail** (log who did what, when)
7. **Input sanitization** (prevent injection attacks)
8. **Output validation** (verify data from external sources)

**Credential management:**
- Use environment variables
- Use credential stores (1Password, Vault, Cloud Secrets)
- Never commit credentials to version control
- Rotate credentials if exposed
- Use short-lived tokens when possible
