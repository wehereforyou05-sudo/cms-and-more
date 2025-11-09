# CMS and More - N8N Templates & Tools

A collection of N8N workflow templates and automation tools for content management and web scraping.

## Available Templates

### N8N Email Scraper with Apify
A powerful workflow for scraping emails from websites using the Apify Web Scraper integration.

**Features:**
- Automated email extraction from websites
- Duplicate email removal
- Multi-page scraping
- Webhook integration for results
- Email validation and processing
- Detailed logging and statistics

**Files:**
- `n8n-email-scraper-apify.json` - The workflow template
- `N8N_EMAIL_SCRAPER_GUIDE.md` - Comprehensive documentation
- `SETUP_INSTRUCTIONS.md` - Quick 5-minute setup guide

**Quick Start:**
1. Read `SETUP_INSTRUCTIONS.md` for 5-minute setup
2. Import `n8n-email-scraper-apify.json` into your N8N instance
3. Add your Apify API credentials
4. Start scraping with HTTP requests to your webhook

## Directory Structure

```
.
├── README.md                           # This file
├── SETUP_INSTRUCTIONS.md               # Quick setup guide
├── N8N_EMAIL_SCRAPER_GUIDE.md         # Full documentation
├── n8n-email-scraper-apify.json       # Email scraper workflow
├── cms.zip                            # CMS files archive
├── images.zip                         # Images archive
└── paths.txt                          # Path references
```

## Prerequisites

- **N8N Instance** - Self-hosted or cloud (https://n8n.io)
- **Apify Account** - For web scraping (https://apify.com)
- **API Credentials** - Apify API token

## Getting Started

### Option 1: Quick Setup (5 minutes)
See `SETUP_INSTRUCTIONS.md`

### Option 2: Full Documentation
See `N8N_EMAIL_SCRAPER_GUIDE.md` for comprehensive details

## Technologies Used

- **N8N** - Workflow automation platform
- **Apify** - Web scraping and data extraction
- **JavaScript** - Custom page functions
- **Regular Expressions** - Email extraction
- **HTTP/REST APIs** - Integration

## Template Details

### Email Scraper Workflow

The email scraper template provides:

1. **Webhook Trigger** - Accept HTTP requests with scraping parameters
2. **Apify Integration** - Execute web scraping at scale
3. **Email Extraction** - Find all email addresses on pages
4. **Data Processing** - Deduplicate and format results
5. **Result Delivery** - Send results via webhook or internal storage

### Workflow Parameters

```json
{
  "url": "https://example.com",           // Starting URL (required)
  "glob": "https://example.com/**",       // URL pattern (optional)
  "maxPages": 10,                         // Max pages to scrape (optional)
  "webhookUrl": "https://..."             // Results webhook (optional)
}
```

### Output Format

```json
{
  "timestamp": "2024-01-15T10:30:45.123Z",
  "status": "completed",
  "totalUniqueEmails": 25,
  "emails": ["contact@example.com", ...],
  "pagesSummary": [...]
}
```

## Usage Examples

### Basic Scraping
```bash
curl -X POST https://your-n8n.com/webhook/email-scraper \
  -H "Content-Type: application/json" \
  -d '{"url": "https://example.com", "maxPages": 5}'
```

### Advanced with Webhook
```bash
curl -X POST https://your-n8n.com/webhook/email-scraper \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com",
    "glob": "https://example.com/contact/**",
    "maxPages": 20,
    "webhookUrl": "https://your-server.com/results"
  }'
```

## Features & Capabilities

### Data Extraction
- Email addresses with regex validation
- Page titles and headings
- All hyperlinks on pages
- Page text content
- Structured data organization

### Processing
- Automatic deduplication
- Email validation
- Multi-page aggregation
- Summary statistics
- Timestamp tracking

### Integration
- Webhook triggers
- Webhook results delivery
- Configurable parameters
- Rate limiting support
- Error handling

## Best Practices

1. **Start Small** - Test with maxPages=5 first
2. **Respect robots.txt** - Check website scraping policies
3. **Use Specific Globs** - Narrow down pages to improve performance
4. **Monitor Apify Usage** - Track costs and API calls
5. **Handle Duplicates** - Template removes email duplicates
6. **Error Handling** - Check N8N execution logs for issues

## Performance Tips

- Reduce `maxPages` to speed up scraping
- Use specific `glob` patterns
- Enable caching for repeated URLs
- Use Apify Proxy for IP rotation
- Monitor execution times and adjust accordingly

## Troubleshooting

See `SETUP_INSTRUCTIONS.md` for common issues and solutions.

## Configuration

### Apify Settings

The template uses Apify's Web Scraper with:
- Custom page function for email extraction
- Regex pattern: `[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}`
- Optional proxy rotation
- Configurable crawl depth

## Security Considerations

- Store API keys in N8N credentials (not in code)
- Use HTTPS for webhook endpoints
- Validate input parameters
- Follow website terms of service
- Respect privacy regulations (GDPR, etc.)

## Future Enhancements

- [ ] Phone number extraction
- [ ] Social media scraping
- [ ] Custom field extraction
- [ ] Database integration templates
- [ ] Multi-language support
- [ ] Advanced filtering options

## Support

- Check `N8N_EMAIL_SCRAPER_GUIDE.md` for detailed documentation
- Review `SETUP_INSTRUCTIONS.md` for quick help
- Visit N8N docs: https://docs.n8n.io/
- Visit Apify docs: https://docs.apify.com/

## License

This template collection is provided as-is for use with N8N and Apify services.

## Contributing

Have improvements or new templates? Feel free to contribute!