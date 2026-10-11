# Setup Guide

## 1. Import the escalation workflow first

Import:

`workflows/human-escalation-handler.json`

Reconnect/configure:

- Google Sheets credential
- Gmail credential
- escalation Google Sheet
- human-support recipient email

Create a Google Sheet called `AI Chatbot Escalations` with these headers:

```text
Ticket ID | Date | Customer Message | Escalation Reason | Session ID | Status
```

In the Gmail node, replace `support@example.com` with the real support recipient.

## 2. Import the main chatbot workflow

Import:

`workflows/ai-customer-support-chatbot.json`

Reconnect/configure:

- OpenAI credential for the AI Agent
- OpenAI credential for embeddings
- OpenAI credential for topic classification
- Google Sheets credential
- conversation-log Google Sheet

Create a Google Sheet called `AI Chatbot Conversation Logs` with these headers:

```text
Date | Session ID | Customer Message | AI Response | Escalated | Topic
```

## 3. Connect the human escalation workflow

Open the `human_escalation` tool node in the main chatbot workflow.

Select the imported:

`Human Escalation Handler`

This replaces the public placeholder workflow ID.

## 4. Load the demo knowledge base

The main workflow contains a manual knowledge-loading branch.

Run the manual trigger once after configuring the OpenAI credential so the demo knowledge is inserted into the in-memory vector store.

The loading and retrieval vector-store nodes must use the same memory key:

`vector_store_key`

## 5. Test the chatbot

Recommended tests:

```text
What services do you offer?
What does your SEO package include?
Can I get a refund?
I want to dispute a payment and speak to a human.
What are your opening hours?
Do you sell laptops?
```

Confirm:

- grounded answers
- no invented package/service details
- topic classification
- conversation log row
- correct `Escalated` value
- escalation ticket creation when appropriate
- human-support email notification
- ticket ID returned to the customer
- only the clean chatbot response is shown to the customer

## 6. Activate

After testing:

- activate the Human Escalation Handler
- activate the AI Customer Support Chatbot

## Important limitation

The included Simple Vector Store is in-memory and is best suited to a demo/portfolio deployment. Its contents can be lost after a restart. For a persistent client deployment, use a persistent vector database supported by your n8n environment.
