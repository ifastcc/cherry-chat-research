# cherry-skills

Skills for the `ifastcc/cherry-studio` fork.

Right now this repo ships one public skill:

- `cherry-chat-research`

## What It Does

`cherry-chat-research` lets an agent study Cherry Studio chat history through the local `/v1/history` API.

It is meant for open-ended research, not fixed dashboards:

- read topic catalogs
- search message history
- inspect transcripts
- export a raw research workspace
- write a report, essay, profile, or HTML artifact in its own style

## Requirements

Before using the skill:

1. Run the `ifastcc/cherry-studio` fork or another build that exposes `/v1/history`.
2. In Cherry Studio, open `设置 -> API 服务器`.
3. Turn the API server on.

The helper client will first try to auto-discover Cherry Studio's local connection profile file.
If that fails, you can still set the API address and key manually.

## Install

```bash
npx skills add ifastcc/cherry-skills --skill cherry-chat-research -a codex -a claude-code
```

## Optional Manual Config

```bash
export CHERRY_API_BASE_URL=http://127.0.0.1:23333/v1
export CHERRY_API_KEY=your_api_key
```

You can also point directly at a connection profile file:

```bash
export CHERRY_API_CONNECTION_FILE=/absolute/path/to/api-server.json
```

## Repo Layout

The source of truth lives under:

```text
.agents/skills/cherry-chat-research/
```

## License

This repository is published under the AGPL-3.0 license.
