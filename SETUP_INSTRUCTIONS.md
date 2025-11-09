# Quick Setup Instructions for N8N Email Scraper

## 5-Minute Setup Guide

### Step 1: Get Your Apify API Key
1. Go to https://apify.com and create an account
2. Click your avatar → Account → Integrations
3. Copy your API token
4. Keep it safe - you'll need it in the next step

### Step 2: Import Template into N8N
1. Log into your N8N instance
2. Click **Workflows** in the top menu
3. Click **Create New** → **Import from File/JSON**
4. Upload or paste the `n8n-email-scraper-apify.json` file
5. Click **Import**

### Step 3: Add Apify Credentials
1. In the workflow, you'll see the nodes
2. Click on **Apify Web Scraper** node (orange node)
3. Look for the **Authentication** section
4. Select or create new credential:
   - Type: **Apify**
   - API Key: Paste your Apify token from Step 1
   - Name: `apify_credentials`
5. Click **Save**

### Step 4: Configure Optional Webhook
If you want to send results to another service:
1. Click on **Send Results Webhook** node
2. Replace `{{ $json.webhookUrl }}` with your actual webhook URL
3. Or keep it dynamic and pass `webhookUrl` in requests

### Step 5: Activate the Workflow
1. Click the **Activate** toggle in the top-right
2. You'll get a webhook URL displayed
3. Save this URL - you'll use it to trigger scraping

## Test Your Setup

### Using curl:
```bash
curl -X POST https://your-n8n-instance.com/webhook/email-scraper \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com",
    "maxPages": 5
  }'
```

### Using Postman:
1. Create new POST request
2. URL: `https://your-n8n-instance.com/webhook/email-scraper`
3. Body (raw JSON):
```json
{
  "url": "https://example.com",
  "maxPages": 5
}
```
4. Send

### Expected Response:
```json
{
  "status": "Scraping started",
  "url": "https://example.com"
}
```

## Monitor Execution

1. After sending a request, watch the execution in real-time
2. Go to **Executions** tab in N8N
3. Click on the latest execution
4. See each node's input/output data

## Get Your Results

After the workflow completes:

1. **In N8N UI**: Check the Final Output node's data
2. **Via Webhook**: Results are sent to the webhook URL you provided
3. **In Apify Dashboard**: View detailed scraping logs at https://console.apify.com

## Customize for Your Needs

### To scrape specific pages only:
```json
{
  "url": "https://example.com",
  "glob": "https://example.com/contact/**",
  "maxPages": 20
}
```

### To scrape deeper (more pages):
```json
{
  "url": "https://example.com",
  "glob": "https://example.com/**",
  "maxPages": 50
}
```

### To send results to your webhook:
```json
{
  "url": "https://example.com",
  "maxPages": 10,
  "webhookUrl": "https://your-server.com/email-results"
}
```

## Common Issues

| Issue | Solution |
|-------|----------|
| "Credentials not found" | Check that `apify_credentials` is selected in Apify node |
| "Invalid API key" | Re-check your Apify token hasn't expired |
| "No emails found" | Website may not have visible emails, try different glob pattern |
| "Timeout" | Reduce maxPages or check your N8N timeout settings |
| "Rate limit" | Space out requests, or upgrade your Apify plan |

## Next Steps

1. **Schedule it**: Add a Schedule trigger instead of Webhook
2. **Save to database**: Add PostgreSQL/MongoDB node for results
3. **Send emails**: Add email node to notify about found emails
4. **Filter results**: Add conditional logic to only process certain domains
5. **Advanced scraping**: Customize the page function in Apify node

## Getting Help

- **N8N Docs**: https://docs.n8n.io/
- **Apify Docs**: https://docs.apify.com/web-scraper
- **Check logs**: N8N shows detailed error messages in execution logs
