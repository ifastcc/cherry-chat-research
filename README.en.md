# cherry-chat-research

[简体中文](./README.md)

`cherry-chat-research` is a skill for studying chat data from Cherry Studio.

It is not a page inside Cherry Studio, and it is not a built-in dashboard.  
It is meant for agents like `Codex` and `Claude Code` that can load skills. If an agent can use skills and can reach Cherry's local history API, it can use this skill.

The easiest way to think about it is this:

- Cherry Studio exposes the data
- the agent reads that data through this skill
- the agent decides how to investigate it and how to present the result

So this is not “a Cherry Studio feature” in the narrow sense. It is an agent skill that uses Cherry chat history as its source material.

## What It Is Good For

This skill is a good fit when you want more than a stats page.

Typical use cases:

- reviewing what you have been returning to lately
- studying recurring themes, motives, tensions, and changes over time
- asking an agent to write a profile, essay, report, or reflective summary based on your chat history
- exporting a raw research workspace for deeper reading

If all you want is a fixed report with a few counts and keyword lists, this skill can help around the edges, but that is not really what it was built for.

## Before You Use It

Make sure Cherry Studio's local API server is running.

Inside Cherry Studio:

- `Settings -> API Server`

Check that:

- the server is on
- you can see the local address and API key

If you are using the `ifastcc/cherry-studio` fork, you usually do not need to set environment variables by hand.  
The helper client will first try to discover Cherry's local connection profile automatically.

## Install

```bash
npx skills add ifastcc/cherry-chat-research --skill cherry-chat-research -a codex -a claude-code
```

## How To Use It

The normal way to use this skill is not by running a script first.  
Just ask your agent to investigate your Cherry Studio history.

For example:

- “Study my Cherry Studio chat history from the last month and tell me what I keep circling around.”
- “Write something closer to a character reading than a dashboard.”
- “Look at my recent chats and tell me whether my interests or anxieties have shifted.”

This skill works best when the agent is allowed to explore.  
It can browse topics, search messages, inspect transcripts, and then decide how the final output should be shaped.

## Manual Configuration

Most of the time, you will not need this.

If auto-discovery fails, set the API address and key yourself:

```bash
export CHERRY_API_BASE_URL=http://127.0.0.1:23333/v1
export CHERRY_API_KEY=your_api_key
```

You can also point directly at a connection profile file:

```bash
export CHERRY_API_CONNECTION_FILE=/absolute/path/to/api-server.json
```

## Exporting A Research Workspace

The repo also includes a script that exports Cherry history into a readable workspace for deeper investigation.

Example:

```bash
python .agents/skills/cherry-chat-research/scripts/analyze_chat_history.py \
  --topic-limit 20 \
  --search "AI" \
  --search "anxiety" \
  --output-dir /tmp/cherry-chat-research
```

That gives you:

- a topic catalog
- readable transcripts
- search result files
- a workspace you can keep exploring with an agent

## Repo Layout

The actual skill source lives here:

```text
.agents/skills/cherry-chat-research/
```

Key files:

- `SKILL.md`
- `agents/openai.yaml`
- `scripts/cherry_history_client.py`
- `scripts/analyze_chat_history.py`

## Who This Is For

If you just want a generic prompt pack, this is probably not the right repo.  
If you want an agent to seriously study your Cherry Studio chat history, it is.

## License

AGPL-3.0
