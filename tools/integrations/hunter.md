# Hunter.io

Email finding and verification platform for outreach and link building.

## Capabilities

| Integration | Available | Notes |
|-------------|-----------|-------|
| API | ✓ | REST API for domain search, email finder, verification |
| MCP | - | Not available |
| CLI | [✓](../clis/hunter.js) | Zero-dependency Node.js CLI |
| SDK | - | API-only |

## Authentication

- **Type**: API Key (query parameter)
- **Parameter**: `api_key={key}`
- **Env var**: `HUNTER_API_KEY`
- **Get key**: [Hunter dashboard > API](https://hunter.io/api-keys)

## Common Agent Operations

### Find emails for a domain

```bash
node tools/clis/hunter.js domain search --domain example.com --limit 10
```

### Find a specific person's email

```bash
node tools/clis/hunter.js email find --domain example.com --first-name John --last-name Doe
```

### Verify an email address

```bash
node tools/clis/hunter.js email verify --email john@example.com
```

### Count emails available for a domain

```bash
node tools/clis/hunter.js domain count --domain example.com
```

### Manage leads

```bash
# List leads
node tools/clis/hunter.js leads list --limit 20

# Create a lead
node tools/clis/hunter.js leads create --email john@example.com --first-name John --last-name Doe --company "Example Inc"

# Delete a lead
node tools/clis/hunter.js leads delete --id 12345
```

### Manage campaigns

```bash
# List campaigns
node tools/clis/hunter.js campaigns list

# Get campaign details
node tools/clis/hunter.js campaigns get --id 12345

# Start/pause a campaign
node tools/clis/hunter.js campaigns start --id 12345
node tools/clis/hunter.js campaigns pause --id 12345
```

### Check account usage

```bash
node tools/clis/hunter.js account info
```

## Rate Limits

- Free plan: 25 searches/month, 50 verifications/month
- Paid plans scale with tier
- API rate limit: 10 requests/second

## Use Cases

- **Link building**: Find email contacts at target domains for outreach
- **Prospecting**: Build lead lists from company domains
- **Verification**: Clean email lists before sending campaigns

### Reading complete domain and lead-list results

The CLI accepts `--offset` and `--limit` on `domain search` and `leads-lists get`. Fetch each page explicitly, advancing the offset by the returned page size until the provider returns no further records. Existing omitted pagination retains the provider default; these commands do not automatically spend credits on subsequent pages.

```bash
node tools/clis/hunter.js domain search --domain example.com --limit 100 --offset 100
node tools/clis/hunter.js leads-lists get --id 123 --limit 100 --offset 100
```

The flags require nonnegative whole-number offsets and limits from 1 to 100. Provider entitlement and total-result limits still apply. See [Hunter’s V2 API reference](https://hunter.io/api-documentation).
