---
name: vity
description: >-
  Use the user's Maximem Vity personal memory through the Vity MCP server: recall
  their preferences, history, decisions, projects and relationships, check what they
  already know about a topic, stage new facts about them for review, and draft or
  rewrite messages in their own writing voice. Use whenever an answer depends on who
  the user is ("what do you know about me", "what did I decide about X", "catch me up
  on Y"), when the user says "remember this", "write this as me" or "in my voice",
  or when "Vity", "Maximem" or their memory vault comes up.
---

# Maximem Vity

Vity is the user's personal AI memory. The `vity` MCP server bundled with this plugin
reads from and writes to **their own** vault, identified only by their API key. No tool
takes a user, account or vault argument, so you never need to pass an identity.

## Before you act

1. **Recall before answering personal questions.** If the answer depends on the user's
   preferences, history, projects or people, call `recall_context` with their actual
   words first. Do not answer from guesses and then check.
2. **Memory is evidence, not certainty.** Speak from what the tools return. If recall
   comes back empty or low-confidence, say you don't have that in their memory rather
   than inventing it.
3. **`remember` does not save immediately.** It stages a statement for the user to
   approve in their Vity dashboard. Tell them it is "noted and pending your review",
   never "saved" or "remembered".
4. **Only stage what you were told.** Do not stage inferences, transient chatter, or
   anything the user asked you to keep private.
5. **Ask before `capture_conversation`.** It sends the whole exchange to the user's
   memory, so tell them what you are about to capture and wait for a yes. A single
   `remember` the user asked for ("remember that…") needs no extra confirmation.
6. **Drafting is not sending.** `rewrite_as_me` and `get_voice_card` only produce text.
   Show the draft to the user; never treat it as approval to post or send it anywhere.
7. **Some tools cost credits or take seconds.** `search_memories` and
   `query_knowledge` cost 1 credit each, and `summarize` costs 20. `get_user_context`,
   `summarize` and `rewrite_as_me` take several seconds. Use the cheaper tool when it
   is enough.

## Choosing a tool

| You want to | Use |
|---|---|
| Background to personalise an answer (fast) | `recall_context` (pass the user's words as `current_prompt`; `strategy: "recency"` for "what was I working on") |
| A specific remembered fact ("what's my sister's name") | `search_memories` (lower `min_score` to ~0.15 if nothing matches) |
| A grounded profile before high-stakes drafting or advice | `get_user_context` with `task_hint` (slower, verified against memory) |
| How much the user knows about a subject | `query_knowledge` (treat `user_accessed_knowledge` as weak evidence, not expertise) |
| Write something in their voice yourself | `get_voice_card`, then draft to that spec |
| Have Vity restyle a finished draft | `rewrite_as_me` with `host` (slack, email, linkedin, x) and `recipient_hint` |
| A summary personalised to what they already know, or of a URL | `summarize` (for plain text already in context, summarize it yourself; it may also note in their memory that they read the content) |
| Keep one durable fact for future sessions | `remember`, as a self-contained third-person statement with a `category` |
| Keep a whole substantive session | `capture_conversation` (up to 50 messages; Vity extracts what matters) |

The server also exposes prompts (`write_as_me`, `brief_me_on`, `catch_me_up`) and
resources (`vity://voice-card`, `vity://knowledge/topics`).

## Writing good memories

Write each `remember` statement so it still makes sense months later, with none of
this conversation around it:

- Good: "Prefers async standups over daily calls."
- Bad: "yes I do", "the thing we discussed".

## Not supported through this server

Deleting memories and viewing or approving staged facts happen in the Vity dashboard
at https://app.maximem.ai, not over MCP. If the user asks to delete something, point
them there.

## When Vity fails

Tool errors come back as plain sentences meant for you to relay. Never claim a memory
was recalled, staged or captured when the call failed; say what happened and, for a
write, give the user the text so it is not lost.

| Error says | Tell the user |
|---|---|
| Invalid or revoked API key | Generate a fresh key at https://app.maximem.ai (**Settings → API Keys**) and update `VITY_API_KEY` |
| Key not permitted for that action | Re-issue the key with the needed scope in the dashboard |
| Rate limit reached | Wait and retry later; do not loop on the call |
| Unreachable or temporary error | Vity is down for the moment; answer without memory and say so |

The server needs a Vity API key (starts with `mx_`) in `VITY_API_KEY`. A `synap_` key
belongs to a different product and will be rejected. Docs:
https://docs.maximem.ai/vity/mcp.
