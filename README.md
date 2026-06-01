# n8n Lead Generation Automation

An automated lead generation pipeline built with n8n that scrapes local businesses, scores them using AI, extracts contact info, and saves everything to Google Sheets — fully on autopilot.

# How the Lead Generation Works

## Quick Overview
You provide a **location and distance**, and this workflow automatically finds nearby businesses, scores them with AI, extracts their contact details (email, phone), and saves the best leads to Google Sheets.

No manual searching. No copy-pasting. Just qualified leads delivered to a spreadsheet.

## The Full Flow

```
Input (Location + Distance)
        ↓
HTTP Node → Scrapes businesses from Overpass API
        ↓
Merge → Combines all results
        ↓
CODE Node → Extracts name, contact info, address, etc.
        ↓
LOOP → Processes each business one by one
        ↓
AI Agent → Scores each lead based on quality
        ↓
CODE Node → Converts LLM output to key-value pairs
        ↓
IF → Score high enough?
    ├── False → Skip this lead
    └── True → Check if contact info exists
                    ↓
               IF → Has website, phone, etc.?
                    ├── False → Save basic info to Google Sheets
                    └── True → Check if only website present
                                    ↓
                               IF → Has website only?
                                    ├── True → HTTP Node scrapes the website
                                    │             ↓
                                    │          CODE → Extract email & phone
                                    │             ↓
                                    │          IF → Email/phone found?
                                    │               ├── True → Save to Google Sheets
                                    │               └── False → Skip
                                    └── False → Save existing info to Google Sheets
```

## Step-by-Step Breakdown

**Step 1 — Input**
- You provide: `Location` (city, area, coordinates) and `Distance` (radius in meters/km)
- This tells the workflow where to look for businesses

**Step 2 — Scrape Businesses**
- HTTP Node calls the **Overpass API** (OpenStreetMap data)
- Fetches all businesses within the given radius
- Returns raw data with names, coordinates, tags

**Step 3 — Extract Details**
- CODE node cleans up the raw data
- Pulls out: business name, address, phone, website, category, etc.
- Structures everything into a clean format

**Step 4 — Loop Through Each Business**
- LOOP node processes businesses one at a time
- Prevents API rate limits and keeps things organized

**Step 5 — AI Lead Scoring**
- AI Agent reads each business's details
- Uses a specific prompt to score the lead (e.g. 0–10)
- Based on completeness, relevance, potential value
- CODE node converts the AI's response into a usable key-value format

**Step 6 — Score Filter (IF Node)**
- Checks if the score is greater than a set threshold
- Low score → skipped entirely
- High score → moves to contact info check

**Step 7 — Contact Info Check (IF Node)**
- Checks if the business already has phone, email, or website
- If nothing found → saved with basic info only
- If something found → deeper check begins

**Step 8 — Website Scraping (IF Node)**
- If only a website URL is present (no direct email/phone)
- HTTP Node visits the website and scrapes it
- CODE node tries to extract email and phone from the page
- IF node checks if extraction was successful
  - Found → saved to Google Sheets with full contact info
  - Not found → skipped

**Step 9 — Save to Google Sheets**
- All qualified leads with contact info are saved
- Each row = one business with name, address, phone, email, score, website

## Use Cases

**Sales Prospecting**
- Find local businesses in any city or area
- Get their contact info automatically
- Focus your outreach on high-scored leads only

**Agency Lead Gen**
- Identify businesses with weak online presence
- Target them with web design or marketing services
- Scale prospecting across multiple locations

**Market Research**
- Map out all businesses in a category in a region
- Understand density, contact availability, digital presence
- Export clean data to sheets for analysis

**Cold Outreach Campaigns**
- Build a fresh leads list for any location in minutes
- Enrich with emails and phone numbers automatically
- Feed directly into your CRM or outreach tool

## What Gets Saved to Google Sheets

| Column | Description |
|---|---|
| Business Name | Name of the business |
| Address | Full street address |
| Phone | Phone number (if found) |
| Email | Email (scraped from website if needed) |
| Website | Website URL |
| AI Score | Lead quality score from AI Agent |
| Category | Type of business |

## Tech Stack

- **Automation**: n8n — visual workflow engine
- **Data Source**: Overpass API (OpenStreetMap) — free business data
- **AI Scoring**: AI Agent node (LLM) — scores lead quality
- **Web Scraping**: HTTP Node — fetches website content
- **Data Output**: Google Sheets — stores all qualified leads
- **Logic**: IF nodes — routes leads based on score and data availability
- **Processing**: CODE nodes — cleans, transforms, and extracts data
