# Secrets Vault

<!-- YOUR CREDENTIALS — agents read this to authenticate on your behalf.
     
     ⚠️  NEVER commit this file. NEVER push it. NEVER share it.
     ⚠️  This file is gitignored by default.
     
     HOW IT WORKS:
     - Setup creates this file with your service names (values blank)
     - You fill in values as you configure each service
     - Agents read from here when they need to make authenticated calls
     - Agents must NEVER echo, log, or include these values in responses
     
     MAINTENANCE:
     - Add new entries when you start using a new service
     - Remove entries when you revoke access
     - Each developer/user has their own copy with their own credentials
-->

## API Keys

| Name | Value | Purpose | Used By |
|------|-------|---------|---------|
| PIXELLAB_API_KEY | | AI pixel art generation | Gateway, PixelLab Pipeline |

## Tokens

| Name | Value | Purpose | Used By |
|------|-------|---------|---------|
| GITHUB_TOKEN | | GitHub API / gh CLI | All repos |
| JIRA_TOKEN | | Jira issue tracking | Work projects |

## Database Connections

| Name | Value | Purpose | Used By |
|------|-------|---------|---------|
| DB_CONNECTION_STRING | | Primary database | |

## Cloud Credentials

<!-- AWS, Azure, GCP — note if managed via CLI config instead -->

| Name | Value | Purpose | Used By |
|------|-------|---------|---------|
| AWS_ACCESS_KEY_ID | | AWS services | Gateway |
| AWS_SECRET_ACCESS_KEY | | AWS services | Gateway |

## Notes

- Fill in values as you set up services — agents will read this when they need to authenticate
- If a service uses its own CLI auth (e.g., `gh auth login`, `aws configure`), note that here instead of duplicating credentials
- AWS: managed via `~/.aws/credentials` — only add here if agents need explicit access
- Keep this file in sync when adding/removing services
