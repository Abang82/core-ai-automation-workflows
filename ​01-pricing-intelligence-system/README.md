# Project 1: Autonomous Competitor Pricing Intelligence System

## 📌 Project Overview
This project solves the problem of high-repetition manual price monitoring in competitive retail and e-commerce markets. It is a completely autonomous system that scrapes competitor data daily, uses Claude AI to evaluate pricing thresholds, and dynamically adjusts database pricing entries with zero human interaction.

## ⚙️ How It Works (Workflow Logic)
1. **Trigger:** A daily Cron Node runs automatically at 06:00 AM (WIB).
2. **Data Scraping:** An HTTP Request Node extracts raw HTML/JSON data from target competitor stores.
3. **Data Cleaning:** A JavaScript Code Node parses raw structures into a clean JSON array containing candidate product names and prices.
4. **AI Analysis:** The Claude AI API node processes the market prices using structured business rules to decide on an optimal corporate response.
5. **Database Sync:** The system updates the live Notion/Google Sheets inventory dashboard.
6. **Notification:** An automated executive report is broadcasted to the management Slack channel.

## 🧠 Claude AI System Prompt
```text
Context: You are an expert Enterprise Pricing Strategy Analyst. Your job is to analyze our current product prices against our direct competitor's scraped data and formulate the most profitable response.

Input Data:
- Our Current Base Price: Rp150,000
- Competitor Scraped Data: {{ $json.competitor_data }}

Rules for Decision Making:
1. If the competitor's price is lower by less than 10%, match their price exactly to stay competitive.
2. If the competitor's price is lower by more than 20%, do not match it (to preserve profit margins). Instead, trigger a "Value Bundle Strategy" status.
3. If the competitor's price is higher, increase our price by 5% below their price to maximize our revenue while remaining the cheaper option.

Output Format (Strict JSON):
{
  "suggested_price": number,
  "strategy_applied": "Price Match" | "Value Bundle" | "Premium Skimming",
  "justification_en": "Clear 1-sentence analytical reason in English."
}
