<p align="center">
  <img src="assets/logo.svg" alt="Maximem Vity" width="96">
</p>

<h1 align="center">Maximem Vity for Cursor</h1>

<p align="center">
  Personal AI memory for the Cursor agent, powered by the hosted Vity MCP server.
</p>

<p align="center">
  <a href="https://docs.maximem.ai/vity/mcp">Documentation</a> ·
  <a href="https://app.maximem.ai">Dashboard</a> ·
  <a href="mailto:support@maximem.ai">Support</a>
</p>

---

## Overview

Maximem Vity gives AI tools a shared, long-term memory of you. This plugin connects
Cursor to your Vity memory, so the agent can:

- Recall your preferences, decisions, projects, and relationships before it answers.
- Understand what you already know about a topic and adjust its explanations.
- Stage new facts about you for your review.
- Draft and rewrite text in your own writing voice.

Any MCP client connected with your key, such as Grok or Claude Code, works with the
same memory.

## What's Included

| Component | Location | Description |
|---|---|---|
| MCP server | `mcp.json` | Connects to the hosted Vity server at `https://vity-mcp.maximem.ai/mcp` over Streamable HTTP. |
| Skill | `skills/vity/SKILL.md` | Guides the agent on when to use memory, which tool fits each task, and how to handle errors. |

## Requirements

- Cursor with plugin support.
- A Maximem Vity account at [app.maximem.ai](https://app.maximem.ai).
- A Vity API key. Vity keys begin with `mx_`.

## Installation

### 1. Create an API key

1. Sign in to the [Vity dashboard](https://app.maximem.ai).
2. Open **API Keys** from the sidebar.
3. Select **Generate API Key** and copy the key.

### 2. Install the plugin

Install **Maximem Vity** from the Cursor Marketplace and set `VITY_API_KEY` to your key.
Cursor asks for the value at install, and you can change it later from the plugin's
**Configure** option.

The plugin sends the key to the Vity server in the `Authorization: Bearer` header. The
key is never stored in this repository.

## Verifying the Connection

Start a new chat in Cursor and enter:

```text
What do you know about me?
```

The agent calls `recall_context` and answers from your memory. If it reports that it
has no information about you, the connection is working and your memory is empty.

To test staging a new fact, enter:

```text
Remember that I prefer short, bullet-point status updates.
```

The agent calls `remember` and confirms that the fact is pending your review. It
becomes part of your memory only after you approve it.

## Tools

### Read operations

| Tool | Description |
|---|---|
| `recall_context` | Returns memories relevant to the current request as a ready-to-use context block. |
| `search_memories` | Finds specific remembered facts and returns the matching entries. |
| `get_user_context` | Builds a structured profile of you in relation to a topic, verified against your memories. |
| `query_knowledge` | Reports what you know about a subject and your level of expertise. |
| `get_voice_card` | Returns your writing-style profile. |
| `rewrite_as_me` | Rewrites a draft in your voice. The result is returned to the agent and is not saved or sent. |

### Write operations

| Tool | Description |
|---|---|
| `remember` | Stages a single fact for your review. Nothing is saved to your memory until you approve it. |
| `capture_conversation` | Sends a conversation to your memory, where Vity extracts the details worth keeping. The skill instructs the agent to ask for your confirmation first. |
| `summarize` | Returns a summary tailored to what you already know. It may also add a brief note to your memory recording that you read the content. |

### Prompts and resources

- **Prompts:** `write_as_me`, `brief_me_on`, `catch_me_up`
- **Resources:** `vity://voice-card`, `vity://knowledge/topics`

### Dashboard-only actions

Deleting memories and managing API keys are available only in the
[Vity dashboard](https://app.maximem.ai). These actions cannot be performed through
this plugin.

## Security and Privacy

- **Key scope:** Each API key can access only its owner's memory. No tool accepts a
  user, account, or vault identifier.
- **Key handling:** Treat your API key like a password. We recommend a separate key for
  each tool, so you can revoke one without affecting the others.
- **No secrets in this repository:** The plugin references the key only through the
  `VITY_API_KEY` variable.

## Troubleshooting

| Issue | Resolution |
|---|---|
| "Invalid or revoked Vity API key" | Generate a new key in the dashboard and update `VITY_API_KEY`. |
| "Vity rate limit reached for this API key" | Wait before trying again. |
| "Vity is unreachable" | The service is temporarily unavailable. Try again shortly. |

## Support

- Documentation: [docs.maximem.ai/vity/mcp](https://docs.maximem.ai/vity/mcp)
- Email: [support@maximem.ai](mailto:support@maximem.ai)

## License

This project is licensed under the [Apache License 2.0](LICENSE).
