# WhatsApp AI Assistant (n8n Workflow)

This is an **n8n automation workflow** that listens for incoming WhatsApp messages, sends them to an AI Agent (powered by Google Gemini) for processing, and then sends the reply back on WhatsApp.

## Workflow Overview

```
WhatsApp Trigger  →  AI Agent (LangChain)  →  Send Message (WhatsApp)
                          ↑
                Google Gemini Chat Model
```

## Nodes

| Node | Type | Function |
|---|---|---|
| **WhatsApp Trigger** | `n8n-nodes-base.whatsAppTrigger` | Listens for incoming WhatsApp messages (event: `messages`) and triggers the workflow. |
| **AI Agent** | `@n8n/n8n-nodes-langchain.agent` | LangChain-based AI Agent node that processes the incoming message — uses **Google Gemini Chat Model** as its language model. |
| **Google Gemini Chat Model** | `@n8n/n8n-nodes-langchain.lmChatGoogleGemini` | Provides the Gemini LLM to the AI Agent so it can generate an intelligent response to the user's message. |
| **Send message** | `n8n-nodes-base.whatsApp` | Sends the final response back to the user via the WhatsApp Business API. |

## Connections (Flow)

1. `WhatsApp Trigger` → `AI Agent`
2. `Google Gemini Chat Model` → `AI Agent` (as language model)
3. `AI Agent` → `Send message`

## Required Credentials

The following credentials must be configured in n8n to run this workflow:

- **WhatsApp OAuth account** — for the WhatsApp Trigger node (incoming messages)
- **WhatsApp account** — for the Send Message node (outgoing messages)
- **Google Gemini (PaLM) API account** — for LLM access in the AI Agent

## ⚠️ Known Issues / To Fix Before Production Use

A few things in this JSON are currently in a template/placeholder state and should be fixed before using it in production:

1. **Hardcoded reply text** — The `Send message` node's `textBody` is currently a fixed string (`"Hey Buddy , Whatsup ?"`) and is **not dynamically linked** to the AI Agent's actual output. Fix by replacing `textBody` with an expression referencing the Agent's output (e.g. `={{ $json.output }}`).
2. **Empty recipient number** — `recipientPhoneNumber` is blank (`----`); it should be dynamically mapped from the sender's number coming from the trigger (e.g. `={{ $json.from }}`), otherwise replies will always go to a single fixed number.
3. **Missing credential IDs/names** — All credential `id` fields are empty; after importing, these accounts will need to be reselected/reconnected.
4. **Workflow inactive** — `"active": false` — needs to be activated before deployment.

## Setup Steps

1. Import this JSON file into your n8n instance.
2. Connect/authenticate all three credentials (WhatsApp Trigger, WhatsApp Send, Google Gemini).
3. Fix the issues listed above (dynamic reply text + dynamic recipient number).
4. Activate the workflow.
5. Send a test message to your WhatsApp Business number to verify.

## Tech Stack

- **n8n** — workflow automation platform
- **LangChain Agent node** — AI orchestration
- **Google Gemini** — LLM (chat model)
- **WhatsApp Business API** — messaging channel
