# Project 2: AI-Driven Smart Fleet & Logistics Dispatcher

## 📌 Project Overview
Built for modern supply-chain and courier operational challenges. This event-driven orchestrator intercepts high-frequency delivery order webhooks, coordinates with database records to track driver locations, and deploys intelligent routing tasks autonomously through a conversational AI loop.

## ⚙️ How It Works (Workflow Logic)
1. **Trigger:** An instant Webhook Node catches raw transactional payloads from incoming customer checkouts.
2. **Fleet Evaluation:** A PostgreSQL/Supabase node pulls a relational array of online drivers, vehicle volume capabilities, and real-time GPS proximity.
3. **Agentic Dispatching:** The Claude AI Node runs multi-conditional optimization trees to choose the single best driver for that specific package type.
4. **Conditional Routing:** A Switch Node reads AI outputs; valid matches move forward, while anomalies route into a priority support triage line.
5. **Driver Notification:** The chosen driver receives a dynamic interactive task template via the WhatsApp Cloud API.
6. **Operations Broadcast:** Central logs synchronize directly to Notion database clusters and internal Slack monitoring loops.

## 🧠 Claude AI System Prompt
```text
Context: You are an autonomous AI Logistics Router. You must assign the incoming delivery order to the single best-fit driver from the available fleet list based on strict operational logic.

Incoming Order:
- Package Volume: {{ $json.body.package_size }} (e.g., Large)

Available Fleet List:
{{ $json.available_drivers }}

Optimization Hierarchy:
1. Vehicle Match: If package volume is "Large", filter out drivers using motorcycles. Only choose drivers with vans/trucks.
2. Proximity: From the filtered list, select the driver with the shortest distance to the pickup location.
3. Availability: Ensure the driver's current active order count is 0.

Output Format (Strict JSON):
{
  "assigned_driver_id": "string",
  "assigned_driver_name": "string",
  "route_efficiency_score": number (scale 1-100),
  "dispatch_reason": "Explain why this driver was chosen based on the hierarchy."
}
