# cherry-chat-research

[简体中文](./README.md)

`cherry-chat-research` is a skill for agents that want to study chat history from Cherry Studio.

It is not a page inside Cherry Studio, and it is not a built-in dashboard. Cherry exposes the data through a local API. The agent uses this skill to read that data, investigate it, and decide how the final result should be written.

## Important: this only works with this fork

This skill is built for [`ifastcc/cherry-studio`](https://github.com/ifastcc/cherry-studio).

It depends on that fork's local `/v1/history` API and connection-profile discovery. Official Cherry Studio does not expose the same surface, so this skill should be treated as unsupported there.

If you have not installed that fork yet, start there first.

## What It Is Good For

This skill is a good fit when you want more than a stats page.

Typical uses:

- reviewing what you keep returning to lately
- studying recurring themes, tensions, motives, and changes over time
- asking an agent to write a profile, essay, report, or reflective reading based on your chat history
- exporting a research workspace for deeper reading

If all you want is a small dashboard with counts and keyword lists, this is probably not the right tool.

## Before You Use It

Turn on the local API server inside `ifastcc/cherry-studio`:

- `Settings -> API Server`

Make sure:

- the server is running
- you can see the local address and API key

Most of the time, that is enough. The skill will try to discover Cherry's local connection file automatically.

## Install

```bash
npx skills add ifastcc/cherry-chat-research -a codex -a claude-code
```

Restart your agent after installation.

## How To Use It

The normal way to use this skill is not by running a script first. Just ask your agent to investigate your Cherry history.

For example:

- “Study my Cherry chat history from the last month and tell me what I keep circling around.”
- “Write something closer to a character reading than a dashboard.”
- “Look at my recent chats and tell me whether my interests or anxieties have shifted.”

This skill works best when the agent is allowed to explore. It can browse topics, search messages, inspect transcripts, and then decide what kind of output makes sense.

## Configuration

By default, the skill tries to discover Cherry's local connection profile automatically.

If that fails, set the API address and key yourself:

```bash
export CHERRY_API_BASE_URL=http://127.0.0.1:23333/v1
export CHERRY_API_KEY=your_api_key
```

You can also point directly at a connection file:

```bash
export CHERRY_API_CONNECTION_FILE=/absolute/path/to/api-server.json
```

## How To Check That It Works

Ask the agent to do something small first, for example:

- “List my latest 10 topics.”
- “Search my recent chats for AI-related messages.”

If the agent can read topics or transcripts, the connection is working.

## Exporting A Research Workspace

The repo also includes a script that exports Cherry history into a readable workspace.

Example:

```bash
python scripts/analyze_chat_history.py \
  --topic-limit 20 \
  --search "AI" \
  --search "anxiety" \
  --output-dir /tmp/cherry-chat-research
```

That gives you:

- `catalog.md`
- `topics/*.md`
- `searches/*.md`
- `manifest.json`

## Repo Layout

This is a single-skill repo. The main files live near the root:

- `SKILL.md`
- `agents/openai.yaml`
- `scripts/cherry_history_client.py`
- `scripts/analyze_chat_history.py`

## License

AGPL-3.0
