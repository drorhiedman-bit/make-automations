# Make automations: AiDO lesson 5

Make.com scenarios built for lesson 5 (Make + databases), exported as Blueprint JSON.

## Dairy heat stress alert

[`blueprints/dairy-heat-stress-alert.blueprint.json`](blueprints/dairy-heat-stress-alert.blueprint.json)

Every day at 18:00 (Asia/Jerusalem) the scenario checks tomorrow's weather forecast for a dairy farm, calculates heat stress (THI), and alerts the farmer only when action is needed.

**Flow**

1. **HTTP**: Open-Meteo forecast for tomorrow (max/min temperature, min/max humidity). Free, no API key.
2. **Set variables**: day THI (Tmax, RHmin) and night THI (Tmin, RHmax).
   `THI = (1.8T + 32) - (0.55 - 0.0055 RH) * (1.8T - 26)`
3. **Set variables**: the farm rules.
   - Showers and drippers: only when the night minimum is 20°C or higher.
   - Fans: when day THI is 72 or higher.
   - Level: none / mild / moderate / severe.
4. **Router**
   - **Alert** (rule triggered): Make AI writes a short Hebrew recommendation, then Telegram bot, then Gmail, then Airtable log row.
   - **No alert**: Airtable log row only.

**Cost on the Make Free plan:** about 7 credits on an alert day and 4 on a quiet day, so at most about 210 of 1,000 credits a month.

## Importing

1. In Make: Create a new scenario › `...` › Import Blueprint.
2. Reconnect Telegram Bot, Gmail, Airtable and Make's AI Provider.
3. Replace the placeholders `<YOUR_TELEGRAM_CHAT_ID>`, `<RECIPIENT_EMAIL>`, `<AIRTABLE_BASE_ID>` and `<AIRTABLE_TABLE_ID>`.
4. Set the schedule to daily at 18:00.

No tokens, passwords or personal email addresses are stored in this repo.
