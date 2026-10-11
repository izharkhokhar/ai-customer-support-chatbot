# Workflow Overview

## AI Customer Support Chatbot

Main path:

```text
When chat message received
        ↓
AI Agent
        ↓
Detect Topic
        ↓
Set Topic branch
        ↓
Log Conversation
        ↓
Return Chat Response
```

AI Agent connections:

```text
AI Agent
├── OpenAI Chat Model
├── Simple Memory
├── Knowledge Base Vector Store
└── human_escalation tool
```

The AI Agent returns intermediate steps so the conversation logger can determine whether the `human_escalation` tool was actually called. The final `Return Chat Response` node strips the internal logging data and returns only the assistant's `output` field to the customer.

## Knowledge loading

```text
Manual Trigger
    ↓
Knowledge Text
    ↓
Simple Vector Store
       ├── Default Data Loader
       └── OpenAI Embeddings
```

The loader and retrieval nodes share the same `vector_store_key`.

## Topic classification

Messages are classified into one of:

- Services
- Consultation
- Payments / Refunds
- Business Info
- Support / Complaints
- Other

Each classifier output passes through a small Set/Edit Fields node that adds a `topic` value before all branches merge into the same conversation logging node.

## Human Escalation Handler

```text
When Executed by Another Workflow
        ↓
Create Ticket ID
        ↓
Append Escalation to Google Sheets
        ↓
Notify Human Support by Gmail
        ↓
Escalation Result
```

Ticket format:

```text
ESC-YYYYMMDD-<n8n execution id>
```

The handler returns:

```json
{
  "success": true,
  "message": "Human support has been notified successfully.",
  "ticket_id": "ESC-YYYYMMDD-1234"
}
```
