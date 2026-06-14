# AI News Bot - GENERAL Design Document

## Summary
This Telegram bot delivers real-time updates on AI news articles specifically focused on **Natural Language Processing (NLP)** and **Robotics** to a shared team channel. It autonomously monitors reputable AI news sources (e.g., TechCrunch AI, VentureBeat AI, arXiv AI sections) for new publications, filters them by relevance to the specified topics, and posts concise summaries (title, snippet, link) to a pre-configured channel. The bot is designed for teams requiring immediate access to cutting-edge developments in NLP and Robotics without manual curation or user interaction.

## Core Entities
- **AI News Article**: 
  - Title (string)
  - URL (URI)
  - Source (string, e.g., "TechCrunch AI")
  - Snippet (string, first 200 characters of content)
  - Timestamp (ISO 8601 datetime)
  - Hash (unique identifier for deduplication, e.g., SHA-256 of URL + snippet)
- **Configuration**: 
  - Telegram Bot Token (string)
  - Target Channel ID (string)
  - Whitelisted News Sources (array of strings)
  - Topic Filters (array: ["NLP", "Robotics"])
- **Sent Article Log**: 
  - Article Hash (foreign key to AI News Article)
  - Delivery Status (enum: "success", "failed", "skipped")

## External Dependencies
- **Telegram Bot API**: 
  - `sendMessage` for posting to channels
  - `getUpdates` for potential future admin commands (though not required now)
- **News Source APIs/Feeds**: 
  - RSS feeds or official APIs from TechCrunch AI, VentureBeat AI, and arXiv.org
  - Web scraping (fallback) for sources without APIs
- **Persistence**: 
  - Lightweight database (SQLite or similar) to store:
    - Configuration parameters
    - Sent article hashes
    - Last-checked timestamps for each source

## Full Feature List
- [x] **Automated News Monitoring**: Polls configured sources every 5 minutes for new articles.
- [x] **Topic Filtering**: Uses keyword matching and metadata analysis to identify NLP/Robotics relevance.
- [x] **Duplicate Detection**: Prevents resending articles using hash-based comparison.
- [x] **Structured Formatting**: Posts articles with title, snippet, and link in a consistent Telegram message format.
- [x] **Channel Delivery**: Sends updates exclusively to a pre-configured shared team channel.
- [x] **Error Resilience**: Retries failed deliveries and logs errors for manual review.

## Non-Goals
- Daily aggregated digests (only real-time updates are supported)
- User-facing commands or interactive features (e.g., `/subscribe`, `/search`)
- News coverage beyond NLP and Robotics (e.g., Computer Vision, Machine Learning)
- Direct messaging to individual users (all updates go to the shared channel)
- Paid subscription models or payment integrations