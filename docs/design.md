# AI News Bot - UX Specification Document

## COMMAND TREE

| Command        | Description                                                                 | Access Level |
|----------------|-----------------------------------------------------------------------------|--------------|
| `/start`       | Initialize bot configuration flow (channel ID setup)                        | Admin        |
| `/setup`       | Re-enter configuration mode                                                 | Admin        |
| `/sources`     | List/configure whitelisted news sources                                     | Admin        |
| `/status`      | Show current bot configuration and operational status                     | Admin        |
| `/restart`     | Restart monitoring process                                                  | Admin        |
| `/help`        | Show available commands                                                   | All          |

## DIALOG STATE MACHINE

**States:**
1. **Idle** (default state - autonomous news delivery)
2. **Configuring Channel** (awaiting channel ID input)
3. **Configuring Sources** (awaiting source selection)
4. **Error Recovery** (handling delivery failures)

**Transitions:**
- `/start` → Configuring Channel  
- `/setup` → Configuring Channel  
- `/sources` → Configuring Sources  
- Valid channel ID input → Idle  
- Invalid input → Error Recovery  
- Successful delivery → Idle  
- Failed delivery → Error Recovery  

## INLINE-KEYBOARD LAYOUTS

### Configuration Menu (shown during setup)
```
[Configure Channel] [View Current Sources]
[Restart Monitoring] [Check Status]
```

### Source Selection Menu
```
[TechCrunch AI] [VentureBeat AI] [arXiv AI]
[Add Custom Source] [Remove Source]
```

### Error Recovery Menu
```
[Retry Last Failed] [View Error Log]
[Return to Idle] [Configure Sources]
```

## MESSAGE COPY & TONE

**Startup Message:**
> "I'm the AI News Bot! Let's set up your team channel first. Please share the Telegram channel ID where you want me to post updates."

**News Delivery Format:**
> 📰 **New [NLP/Robotics] Development**  
> _[Source Name] • [Relative Time]_  
> "This paper introduces..." [Read more]([URL])

**Error Messages:**
- Channel Error:  
  > "⚠️ I can't post to the configured channel. Please verify the channel ID or my permissions."
- Duplicate Detection:  
  > "🔄 Skipping duplicate article from [Source]."
- Network Failure:  
  > "❗ Failed to fetch updates from [Source]. Retrying in 5 minutes..."

**Tone:** Professional yet approachable, with clear visual hierarchy using emojis and formatting.

## EDGE CASES

| Scenario                          | Handling                                                                 |
|----------------------------------|--------------------------------------------------------------------------|
| Invalid channel ID               | Prompt with "❌ Invalid channel ID. Please try again or check permissions." |
| Missing source API access        | Fallback to web scraping with warning message                             |
| Article without snippet          | Post title + URL only with "(No summary available)" note                 |
| No new articles in polling cycle | No message sent; logs "ℹ️ No new articles found in this cycle"            |
| Unknown commands                 | "❓ I don't handle that command. Use /help to see available options."      |
| Missing configuration            | Redirect to configuration flow with "⚙️ Please configure me first"        |

## i18n REQUIREMENTS

**Translatable Strings:**
- All user-facing messages (error messages, prompts, button labels)
- News article categories ("NLP", "Robotics")
- Time formatting (relative time expressions)
- Source names (e.g., "TechCrunch AI")

**Non-Translatable:**
- Technical identifiers (command names)
- Bot token and channel ID values
- Article hashes and timestamps