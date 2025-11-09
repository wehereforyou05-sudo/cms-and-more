# N8N Email Scraper Template with Apify

A comprehensive n8n workflow template for scraping emails from websites using the Apify Web Scraper integration.

## Overview

This template automates the process of:
- Scraping websites using Apify's Web Scraper actor
- Extracting email addresses from page content
- Deduplicating found emails
- Processing and formatting results
- Webhook notifications with results

## Template Features

### 1. **Webhook Trigger**
- Accepts HTTP POST requests with scraping parameters
- Endpoint: `/webhook/email-scraper`
- Required parameters:
  - `url` (string): The starting URL to scrape
  - `glob` (string, optional): URL pattern to match pages (default: `*` for all)
  - `maxPages` (number, optional): Maximum pages to scrape (default: 10)
  - `webhookUrl` (string, optional): Webhook to send results to

### 2. **Apify Web Scraper**
- Uses Apify's `apify/web-scraper` actor
- Extracts emails using regex pattern: `/[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/g`
- Collects additional data:
  - Page title
  - All links on the page
  - Headings (H1, H2, H3)
  - Full page text

### 3. **Email Processing**
- Extracts unique emails from all scraped pages
- Removes duplicate email addresses
- Groups results by page
- Generates summary statistics

### 4. **Output Formats**
- JSON response with:
  - `uniqueEmails`: Array of all unique emails found
  - `pages`: Detailed information per scraped page
  - `timestamp`: When the scraping completed
  - `totalPagesScraped`: Number of pages processed

## Prerequisites

### Required API Keys
1. **Apify API Key**
   - Sign up at https://apify.com
   - Get your API token from Account Settings
   - Create a credential in n8n named `apifyApi`

2. **N8N Webhook URL** (optional)
   - For sending results to external systems
   - Configure your webhook endpoint URL

## Installation Steps

### 1. Import the Template
```
1. Open your N8N instance
2. Go to Workflows → Templates
3. Click "Import from JSON"
4. Copy the contents of `n8n-email-scraper-apify.json`
5. Paste into the import dialog
6. Click Import
```

### 2. Configure Credentials
```
1. Navigate to Credentials in N8N
2. Create new credential: "Apify API"
3. Select credential type: "Apify"
4. Enter your Apify API token
5. Save the credential
```

### 3. Update Node Credentials
```
1. Open the imported workflow
2. Select the "Apify Web Scraper" node
3. In the credentials section, select the "Apify API" credential you created
4. Click Save
```

## Usage Examples

### Example 1: Scrape Single Website
**Request:**
```bash
curl -X POST https://your-n8n.com/webhook/email-scraper \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com",
    "maxPages": 5
  }'
```

**Response:**
```json
{
  "status": "Scraping started",
  "url": "https://example.com"
}
```

### Example 2: Scrape Specific Pages
**Request:**
```bash
curl -X POST https://your-n8n.com/webhook/email-scraper \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com",
    "glob": "https://example.com/contact/**",
    "maxPages": 20,
    "webhookUrl": "https://your-webhook.com/results"
  }'
```

### Example 3: Deep Crawl
**Request:**
```bash
curl -X POST https://your-n8n.com/webhook/email-scraper \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com",
    "glob": "https://example.com/**",
    "maxPages": 100
  }'
```

## Workflow Nodes Explained

### Node 1: Webhook Trigger
- **Type**: Webhook
- **Purpose**: Receives scraping requests
- **Input**: URL and configuration parameters

### Node 2: Apify Web Scraper
- **Type**: HTTP Request to Apify API
- **Purpose**: Executes the web scraping actor
- **Outputs**: Dataset ID for retrieving results

### Node 3: Get Apify Results
- **Type**: HTTP Request
- **Purpose**: Fetches scraped data from Apify
- **Outputs**: Array of pages with extracted data

### Node 4: Process Emails
- **Type**: Set (Expression)
- **Purpose**: Extracts and deduplicates emails
- **Outputs**:
  - `uniqueEmails`: List of all unique emails
  - `pages`: Details per page

### Node 5: Send Results Webhook
- **Type**: HTTP Request
- **Purpose**: Sends results to external webhook (optional)
- **Triggered when**: `webhookUrl` is provided

### Node 6: Final Output
- **Type**: Function
- **Purpose**: Formats final output
- **Outputs**: Structured JSON with results

## Configuration Options

### Apify Web Scraper Settings

#### Page Function
The template includes a custom page function that:
- Extracts all text content
- Uses regex to find email addresses
- Collects all links
- Extracts heading text
- Removes duplicate emails

#### Proxy Configuration
- **useApifyProxy**: Set to `true` for IP rotation (requires Apify Proxy subscription)
- **apifyProxyCountry**: Specify country code for location-based scraping

#### Rate Limiting
- Adjust `maxPagesPerCrawl` in the webhook parameters
- Recommended: 10-50 pages for general use
- Max: 1000 pages (depends on Apify plan)

## Email Extraction

The template uses this regex pattern to find emails:
```regex
[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
```

This pattern matches:
- Standard email format (user@domain.com)
- Emails with dots and numbers
- Emails with + addressing
- International domains (2+ character TLDs)

## Output Format

### Success Response
```json
{
  "timestamp": "2024-01-15T10:30:45.123Z",
  "status": "completed",
  "totalUniqueEmails": 25,
  "emails": [
    "contact@example.com",
    "info@example.com",
    "support@example.com"
  ],
  "pagesSummary": [
    {
      "url": "https://example.com",
      "title": "Example Website",
      "emailsFound": 3,
      "emails": ["contact@example.com", "info@example.com"]
    }
  ]
}
```

## Troubleshooting

### Issue: "Apify API authentication failed"
**Solution:**
- Verify your Apify API key is correct
- Check credentials are properly configured in N8N
- Ensure API key hasn't expired

### Issue: "No emails found"
**Solution:**
- Check if the website has any contact information
- Try adjusting the `glob` pattern to specific pages
- Verify the website isn't blocking scraping
- Check robots.txt permissions

### Issue: "Maximum pages exceeded"
**Solution:**
- Reduce `maxPages` parameter
- Use more specific `glob` patterns
- Check Apify account usage and plan limits

### Issue: "Timeout errors"
**Solution:**
- Reduce `maxPages`
- Increase timeout in n8n execution settings
- Use Apify Proxy to avoid IP blocking

## Advanced Usage

### Custom Email Validation
Modify the page function to validate emails:
```javascript
// Add to page function
pageData.emails = pageData.emails.filter(email => {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
});
```

### Store Results in Database
Add a node after "Process Emails" to save to:
- PostgreSQL
- MongoDB
- Google Sheets
- Airtable

### Schedule Regular Scraping
Use N8N's schedule trigger instead of webhook:
1. Replace Webhook node with Schedule node
2. Set frequency (daily, weekly, etc.)
3. Configure fixed URL or load from database

## Performance Tips

1. **Use specific globs**: Narrow down which pages to scrape
2. **Limit maxPages**: Start with 10, increase as needed
3. **Cache results**: Add caching to avoid re-scraping
4. **Use proxy rotation**: Enable Apify Proxy for large-scale scraping
5. **Batch requests**: Use rate limiting to avoid IP bans

## Legal & Ethical Considerations

- **Check robots.txt**: Respect website scraping policies
- **Review Terms of Service**: Ensure you have permission
- **Rate limiting**: Don't overload servers
- **Data privacy**: Handle collected emails responsibly
- **GDPR compliance**: Comply with applicable regulations

## Support & Resources

- **Apify Documentation**: https://docs.apify.com/
- **N8N Documentation**: https://docs.n8n.io/
- **Apify Forum**: https://forum.apify.com/
- **N8N Community**: https://community.n8n.io/

## License

This template is provided as-is for use with N8N and Apify services.

## Version History

- **v1.0** (2024-01-15): Initial template release
  - Basic email extraction
  - Apify Web Scraper integration
  - Webhook results notification
  - Email deduplication
