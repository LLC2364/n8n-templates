# n8n Templates

5 Production-Ready n8n Automation Templates — Save 20+ hours/week.

## Templates

### 1. GitHub Issue → Telegram Sync (`github-telegram-sync.json`)
Automatically send Telegram notifications when new GitHub issues are created or updated.

**Trigger:** GitHub Webhook  
**Nodes:** Webhook → Code (parse issue) → Telegram Bot Message  
**Use case:** Get real-time alerts for new issues in your repositories.

---

### 2. RSS Feed → Telegram Digest (`rss-telegram-notifier.json`)
Monitor RSS feeds and send new articles to Telegram as a digest.

**Trigger:** Schedule (Cron)  
**Nodes:** RSS Feed Read → Filter (new items) → Format Digest → Telegram Bot Message  
**Use case:** Monitor blogs, news sources, or YouTube channel feeds.

---

### 3. Webhook → Telegram Alert (`webhook-telegram-alert.json`)
Generic webhook receiver that forwards HTTP POST/GET data to Telegram.

**Trigger:** Webhook  
**Nodes:** Webhook → Telegram Bot Message  
**Use case:** Alerts from monitoring tools, CI/CD pipelines, or custom scripts.

---

### 4. Daily Summary → Telegram (`daily-summary-cron.json`)
Send a daily summary message to Telegram on a schedule.

**Trigger:** Cron (daily at 9:00 AM)  
**Nodes:** Schedule Trigger → Code (build summary) → Telegram Bot Message  
**Use case:** Morning briefings, daily task reminders, daily stats reports.

---

### 5. Email Forward to Telegram (`email-telegram-forward.json`)
Forward emails to a Telegram chat via a dedicated email address.

**Trigger:** Email Trigger (IMAP)  
**Nodes:** Email Trigger → Code (format) → Telegram Bot Message  
**Use case:** Get important emails sent directly to Telegram.

---

## Installation

1. Open n8n
2. Import the JSON file (Workflows → Import from JSON)
3. Configure your Telegram Bot token and chat ID
4. Activate the workflow

## Configuration

Each template requires:
- **Telegram Bot Token:** Create via @BotFather in Telegram
- **Telegram Chat ID:** Your chat ID or group ID
- **GitHub Webhook / RSS URL / IMAP credentials:** As needed per template

## License

MIT — Use freely, modify as needed.
