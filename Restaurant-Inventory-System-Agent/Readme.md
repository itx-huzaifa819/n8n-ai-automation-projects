# Restaurant Inventory System Agent

An AI-powered inventory management system for restaurants, built with **n8n**, **Google Gemini**, and **Airtable**. Users query inventory through natural language instead of manually searching records.

## Overview

The system accepts natural-language questions — e.g. *"Do we have chicken breast?"* or *"What items are low in stock?"* — and returns relevant inventory information by querying Airtable through an AI agent.

## Architecture

```
User → n8n Chat Trigger → AI Agent (Google Gemini + Memory) → Airtable Tool → Response
```

## Tech Stack

| Component | Role |
|---|---|
| n8n | Workflow orchestration and chat interface |
| Google Gemini | LLM powering the AI agent |
| Airtable | Inventory data store |
| Simple Memory | Conversational context retention |

## Key Features

- **Natural-language queries** — no manual database searches
- **Live Airtable integration** — dynamic record retrieval
- **Conversational memory** — context persists across turns

## Airtable Schema

| Field | Description |
|---|---|
| Item Name | Name of the inventory item |
| Category | Inventory category |
| Quantity | Available quantity |
| Unit | Measurement unit |
| Status | Current inventory status |

*Fields can be customized to fit specific restaurant needs.*

## Setup

1. **Deploy n8n** — self-hosted or n8n Cloud.
2. **Import the workflow** — load `workflow/restaurant-inventory-system.json`.
3. **Connect Google Gemini** — add a Gemini credential and link it to the Chat Model node.
4. **Connect Airtable** — set `Base ID` / `Table ID` and attach your Airtable credential.
5. **Activate** — enable the workflow and open the chat interface.

## Example Queries

```
What items do we have in inventory?
Do we have chicken?
Search for tomatoes.
```

## Roadmap

- Low-stock alerts and automatic stock deduction
- Add/update/delete inventory via chat
- Expiry-date tracking and supplier management
- Analytics dashboard, multi-restaurant support, role-based access

## Project Structure

```
Restaurant-Inventory-System/
├── workflow/
│   └── restaurant-inventory-system.json
├── README.md
```

## Author

**Huzaifa Sajid** — Computer Science Student | AI & Automation Enthusiast

---
⭐ If you find this project useful, consider starring the repository.
