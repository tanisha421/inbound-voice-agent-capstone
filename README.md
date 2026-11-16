# Inbound Voice Agent Capstone

This repository contains an importable n8n workflow and assessment documentation for an inbound welcome + FAQ voice agent.

## Files
- `inbound_voice_agent_n8n_workflow.json` — import into n8n.
- `Inbound_Voice_Agent_Capstone_Submission.pdf` — implementation guide, configuration checklist, test plan, and submission notes.

## Architecture
- `POST /webhook/voice-agent/faq` — matches a caller question against the demo FAQ list and returns `faq_match` or a safe `unclear` fallback.
- `POST /webhook/voice-agent/events` — accepts voice-platform events, normalizes event names, captures transcript payloads in n8n execution history, and creates a basic intent/escalation/sentiment estimate.

## Import and configure
1. In n8n, choose **Workflows → Import from File** and select the JSON.
2. Save the workflow. Open each Webhook node and copy its **Production URL** after activation.
3. Replace demo FAQ answers in **Match FAQ and Classify Intent** with approved company information. Do not leave placeholders.
4. Configure the voice platform (Vapi/Retell/Bland) with the approved assistant prompt, greeting, voice, and call-ending behavior.
5. Configure the platform's server-event URL to the `voice-agent/events` production webhook URL.
6. If using the FAQ webhook as a tool, create a server tool in the voice platform pointing to the `voice-agent/faq` production URL. Pass the caller's question in `query`.
7. Connect the Twilio number to the voice platform using the platform's supported number import/SIP process.
8. Test with a real or simulated call before submitting.

## Important limitations
- The workflow does not provision Twilio or create a Vapi/Retell/Bland assistant automatically. Those steps require account access, phone-number ownership, and credentials.
- The FAQ matcher is a transparent keyword-based starter, not semantic search. Extend it with approved knowledge retrieval if required.
- Sentiment is a simple keyword estimate, not a validated sentiment model.
- Configure the exact webhook event subscription and tool response schema required by your selected voice platform. Platform event payloads can differ.
- Do not put API keys or Twilio credentials directly in Code nodes. Use n8n credentials or the voice platform's secure credential store.

## Test payloads

FAQ:
```json
{ "query": "What are your opening hours?" }
```

Unmatched FAQ:
```json
{ "query": "Can you guarantee my application will be approved?" }
```

Event:
```json
{
  "type": "transcript.received",
  "call": { "id": "demo-call-001" },
  "transcript": "Hello, what are your opening hours? Thank you."
}
```

## GitHub publication
Create a new repository at https://github.com/new, upload these files, and use the repository URL as your LMS GitHub link. This workspace cannot publish to your GitHub account without an authenticated GitHub connection.
