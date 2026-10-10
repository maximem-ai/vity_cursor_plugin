# Maximem Vity for Cursor

Personal AI memory over MCP. This plugin connects Cursor to your Vity memory so the
agent can recall what you've told it, look up what you already know, stage new facts
for your review, and write in your voice. It's the same memory you use in Grok, Claude
Code and any other MCP client.

## What's included

| Component | File | Purpose |
|---|---|---|
| MCP server | `mcp.json` | The hosted Vity server at `https://vity-mcp.maximem.ai/mcp` (Streamable HTTP) |
| Skill | `skills/vity/SKILL.md` | Tells the agent when to recall memory, which tool to use, and how to handle errors |

## Setup

1. Sign in at https://app.maximem.ai and go to **Settings → API Keys → Generate New Key**.
   Vity keys start with `mx_`.
2. Install **Maximem Vity** from the Cursor Marketplace.
3. When Cursor asks for `VITY_API_KEY`, paste your key. It is sent to the server as
   `Authorization: Bearer <your key>`.

The key is never stored in this repo. It only reaches your own vault, so treat it like
a password and use a separate key per tool so you can revoke one without breaking the
others.

## Try it

In a new chat, ask:

```
What do you know about me?
```

The agent should call `recall_context` and answer from your vault. If it says it has
no information, the connection works but your vault is empty. Then try:

```
Remember that I prefer short, bullet-point status updates.
```

The agent should call `remember` and tell you the fact is pending your review. Approve
it in the dashboard, and it becomes part of your memory.

## What the tools can read and change

**Read only**

- `recall_context`, `get_user_context`, `search_memories`: your memories
- `query_knowledge`: what Vity has learned you know about
- `get_voice_card`: your writing-style profile
- `rewrite_as_me`: returns a rewritten draft; nothing is saved or sent

**Write**

- `summarize`: returns a personalised summary, and may also add a one-line note to
  your memory that you read this content, so your briefings know about it.
- `remember`: stages one fact. It is **not** saved until you approve it in the dashboard.
- `capture_conversation`: sends a conversation to your memory, and Vity extracts what
  is worth keeping. The skill tells the agent to ask you first.

**Not possible through this plugin**

Deleting memories, approving staged facts, and changing account settings. Those are
dashboard-only, at https://app.maximem.ai.

Also included: prompts `write_as_me`, `brief_me_on`, `catch_me_up`, and resources
`vity://voice-card`, `vity://knowledge/topics`.

## Troubleshooting

- **"Invalid or revoked Vity API key"**: generate a new key and update `VITY_API_KEY`.
- **A `synap_` key is rejected**: that key is for a different Maximem product. Use an
  `mx_` key.
- **Rate limit reached**: wait and try again later.

## Links

- Docs: https://docs.maximem.ai/vity/mcp
- Dashboard: https://app.maximem.ai
- Support: support@maximem.ai

## License

[Apache-2.0](LICENSE)
