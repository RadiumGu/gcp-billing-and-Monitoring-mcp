# GCP Billing and Monitoring MCP Server

A Model Context Protocol server that connects to Google Cloud services to provide context and tools for interacting with your Google Cloud resources.

## Services

Supported Google Cloud services:

- [x] [Billing](https://cloud.google.com/billing)
- [x] [Logging](https://cloud.google.com/logging)
- [x] [Monitoring](https://cloud.google.com/monitoring)

### Billing

Manage and analyse Google Cloud billing with cost optimisation insights:

**Tools:** `gcp-billing-list-accounts`, `gcp-billing-get-account-details`, `gcp-billing-list-projects`, `gcp-billing-get-project-info`, `gcp-billing-list-services`, `gcp-billing-list-skus`, `gcp-billing-analyse-costs`, `gcp-billing-detect-anomalies`, `gcp-billing-cost-recommendations`, `gcp-billing-service-breakdown`

*Example prompts:*
- "Show me all my billing accounts"
- "Analyse costs for project my-app-prod-123 for the last 30 days"
- "Generate cost recommendations for billing account billingAccounts/123456-789ABC-DEF012"
- "Check for billing anomalies in project my-ecommerce-456"

### Logging

Query and filter log entries from Google Cloud Logging:

**Tools:** `gcp-logging-query-logs`, `gcp-logging-query-time-range`, `gcp-logging-search-comprehensive`

*Example prompts:*
- "Show me logs from project my-app-prod-123 from the last hour with severity ERROR"
- "Search for logs containing 'timeout' from service my-api in project backend-456"
- "Query logs for resource type gce_instance in project compute-prod-789"

### Monitoring

Retrieve and analyse metrics from Google Cloud Monitoring:

**Tools:** `gcp-monitoring-query-metrics`, `gcp-monitoring-list-metric-types`, `gcp-monitoring-query-natural-language`

*Example prompts:*
- "Show me CPU utilisation metrics for project web-app-prod-123 for the last 6 hours"
- "List available metric types for Compute Engine in project infrastructure-456"
- "Query memory usage for instances in project backend-services-789"

## Quick Start

Once configured, you can interact with Google Cloud services using natural language:

```
"What are my current billing costs for project my-webapp-prod-123?"
"Find logs containing 'database timeout' from project backend-prod-321 yesterday"
"What's the CPU usage of Compute Engine instances in project infrastructure-987?"
```

## Authentication

This server supports two methods of authentication with Google Cloud:

1. **Service Account Key File** (Recommended): Set the `GOOGLE_APPLICATION_CREDENTIALS` environment variable to the path of your service account key file. This is the standard Google Cloud authentication method.

2. **Environment Variables**: Set `GOOGLE_CLIENT_EMAIL` and `GOOGLE_PRIVATE_KEY` environment variables directly. This is useful for environments where storing a key file is not practical.

The server will also use the `GOOGLE_CLOUD_PROJECT` environment variable if set, otherwise it will attempt to determine the project ID from the authentication credentials.

## Installation

```bash
# Clone the repository
git clone https://github.com/RadiumGu/gcp-billing-and-Monitoring-mcp.git
cd gcp-billing-and-Monitoring-mcp

# Install dependencies
pnpm install

# Build
pnpm build
```

Authenticate to Google Cloud:

```bash
gcloud auth application-default login
```

Configure the `mcpServers` in your client:

```json
{
  "mcpServers": {
      "gcp-billing-and-monitoring-mcp": {
          "command": "node",
          "args": [
              "/path/to/gcp-billing-and-Monitoring-mcp/dist/index.js"
          ],
          "env": {
              "GOOGLE_APPLICATION_CREDENTIALS": "/path/to/your/service-account-key.json"
          }
      }
  }
}
```

## Development

### Starting the server

```bash
# Build the project
pnpm build

# Start the server
pnpm start
```

### Development mode

```bash
# Build the project
pnpm build

# Start the server and inspector
npx -y @modelcontextprotocol/inspector node dist/index.js
```

## Troubleshooting

### Server Timeout Issues

If you encounter timeout issues when running the server with Smithery, try the following:

1. Enable debug logging by setting `debug: true` in your configuration
2. Ensure `lazyAuth: true` is set to defer authentication until it's actually needed
3. Ensure your credentials file is accessible and valid
4. Check the logs for any error messages

**Important**: Authentication is still required for operation, but with lazy loading enabled, the server will start immediately and authenticate when needed rather than during initialization.

### Authentication Issues

The server supports two methods of authentication:

1. **Service Account Key File**: Set `GOOGLE_APPLICATION_CREDENTIALS` environment variable to the path of your service account key file
2. **Environment Variables**: Set `GOOGLE_CLIENT_EMAIL` and `GOOGLE_PRIVATE_KEY` environment variables

If you're having authentication issues, make sure:

- Your service account has the necessary permissions
- The key file is properly formatted and accessible
- Environment variables are correctly set
