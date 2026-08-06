# 🔍 AI Research Agent

An autonomous research agent that searches the web, reads articles, and generates structured research reports using Claude AI — triggered automatically via n8n.

## What it does

1. **Searches** DuckDuckGo for any topic you give it
2. **Scrapes** the top articles automatically
3. **Sends** the content to Claude API for deep analysis
4. **Generates** a structured markdown report with insights, trends, and follow-up questions
5. **Delivers** the report to your inbox via n8n automation

## Architecture

```
n8n Scheduler / Form
        ↓
HTTP POST → Python Flask Server
        ↓
DuckDuckGo Search (top 6 results)
        ↓
BeautifulSoup Scraper (reads each page)
        ↓
Claude API (summarises + analyses)
        ↓
Markdown Report Generator
        ↓
n8n → Gmail delivery
```

## Tech Stack

| Tool | Purpose |
|---|---|
| Python | Core agent logic |
| Claude API (claude-sonnet-4-6) | AI summarisation & analysis |
| DuckDuckGo Search | Free web search (no API key needed) |
| BeautifulSoup | Web scraping |
| Flask | Webhook server for n8n |
| n8n | Automation trigger + Gmail delivery |

## Setup

### 1. Clone the repo
```bash
git clone https://github.com/yourusername/ai-research-agent
cd ai-research-agent
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Add your Claude API key
Open `agent.py` and replace:
```python
CLAUDE_API_KEY = "your-claude-api-key-here"
```

### 4. Run directly from terminal
```bash
# Research any topic
python agent.py --topic "AI in healthcare India 2025"

# Control number of sources
python agent.py --topic "your topic" --results 8
```

### 5. Start webhook server for n8n
```bash
python agent.py --server
```
Then in n8n, create an HTTP Request node:
- Method: POST
- URL: http://localhost:5000/research
- Body: `{"topic": "your topic here"}`

## Sample Output

```
# AI Research Report

**Topic:** AI in healthcare India 2025
**Generated:** 29 July 2026, 10:30 AM
**Sources read:** 6

## Executive Summary
...

## Key Insights
- ...

## Trends Identified
- ...

## Follow-up Questions
1. ...
```

## n8n Workflow Setup

1. Add a **Schedule Trigger** node (e.g. every morning at 8 AM)
2. Add a **Set** node to define the topic
3. Add an **HTTP Request** node → POST to `http://localhost:5000/research`
4. Add a **Gmail** node → send `{{ $json.report }}` to your email

## LinkedIn Post

Built this as part of my AI Automation Engineer portfolio.
Full walkthrough: [link to your post]

---
Built by Puru Tiwari · Claude API + DuckDuckGo + Python + n8n

