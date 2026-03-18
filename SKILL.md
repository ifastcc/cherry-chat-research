---
name: cherry-chat-research
description: Research Cherry Studio chat history through the local /v1/history API. Use when the user wants a deep, open-ended reading of one person's recent concerns, recurring themes, values, motives, personality texture, shifts over time, or an evidence-backed personal portrait.
---

# Cherry Chat Research

This skill turns the model into a chat-history researcher.
It is not a dashboard generator, and it is not a fixed analytics pipeline.

The job is to study Cherry Studio history deeply enough that the final output feels specific, alive, and personally meaningful.
The report can be reflective, psychological, narrative, philosophical, or portrait-like as long as it stays grounded in the actual chat record.

## Connection

Prefer Cherry Studio's local connection profile file.
The helper client will try to auto-discover it automatically.

If you need to override that behavior, use one of these:

- `CHERRY_API_CONNECTION_FILE=/absolute/path/to/api-server.json`
- `CHERRY_API_BASE_URL=http://127.0.0.1:<port>/v1`
- `CHERRY_API_KEY=<api key>`

## Data Semantics

- `clear` messages are context boundaries, not normal content.
- A `segment` is the continuous message span after a `clear` boundary.
- A `round` is one user message plus assistant messages whose `askId` matches that user message id.
- `message.annotations.segmentId` and `message.annotations.roundId` are structural anchors, not conclusions.
- `message.annotations.isPreferredResponse=true` means the assistant response was marked as preferred.

## What This Skill Is For

Use this skill when the user wants more than a factual summary.
The real job is to understand what kind of person appears through the history and what seems emotionally, psychologically, or existentially important in their recent conversations.

Good outputs often help answer questions like:

- What keeps returning in this person's inner life?
- What do they seem to want, fear, avoid, or long for?
- What pressures or contradictions shape their decisions?
- What kind of self-story do they keep telling?
- What has recently changed in tone, focus, or identity?
- What is psychologically meaningful here, even if it is not clinically precise?

## Research Style

Do not force the material into a rigid template.
You are allowed to think freely and to decide what structure best fits the history you find.

You may draw from lenses such as:

- personality texture
- narrative identity
- values and motives
- fear, shame, desire, and control
- attachment and relationships
- cognitive style and decision style
- self-image and self-criticism
- existential concerns, meaning, freedom, responsibility

These are interpretive lenses, not cages.
Use them when they help the user feel understood.

If the user asks for a particular angle, follow it.
If the user does not specify a frame, choose the lenses that seem most revealing for the material.

## Lens Library

These are optional ways to read the material. They are prompts for attention, not fixed output sections.

### Action and reflection

- daily encouragement
- action guidance
- blind spot questions
- theme extension
- inner resources
- counterintuitive insights
- default insight

### Self-awareness and relationships

- recurring questions
- key people
- blind spots
- friend view
- director view
- ACT
- CBT
- personality texture, including MBTI-style readings when useful and clearly tentative

### Thinking and decisions

- compounding loops
- primary contradiction
- value clarification
- inversion
- second-order thinking

### Classic lenses

- Charlie Munger
- Aristotle and first principles
- Seneca and the control dichotomy
- Tasha Eurich and self-awareness

Do not mechanically run through these.
Pick the few that actually sharpen the reading.

## Research Moves

There is no mandatory sequence.
Use the moves that fit the task.

- Start broad with the topic catalog.
- Search for motifs, repeated phrases, identities, worries, or names.
- Read full transcripts where the material feels alive or emotionally charged.
- Use context windows around important messages when a search hit is too thin by itself.
- Prefer `returnMode=round` when you need more context than a raw hit but do not want a whole-topic expansion yet.
- Pull several distant messages together when you need to compare evidence.
- Look for shifts over time, not just repeated themes.
- Revise the first impression when the record pushes back.

## Tone And Ambition

Aim for something that feels like a deep reading by an intelligent observer, not a safe internal memo.

The user should ideally feel:

- seen
- understood
- slightly surprised in a truthful way
- invited into deeper reflection

It is fine to be psychologically rich.
It is fine to mention personality tendencies, relational style, or enduring traits when that adds meaning.
Do not flatten the person into a label.

## Evidence

Do not drown the user in citations, but do stay grounded.

- Important claims should come from real messages, recurring patterns, or repeated motifs.
- Use quoted evidence when it sharpens the report.
- If the evidence is weak or mixed, say so.

## Outputs

There is no fixed required structure.
Choose the shape that best fits the material.

Valid deliverables include:

- an insight report
- a personal portrait
- a psychological reading
- a recent-interest study
- a narrative essay
- a standalone HTML reading experience

If you make HTML, design it freely.
Do not assume a fixed dashboard layout or a fixed section list unless the material genuinely calls for it.

## API Surface

Primary endpoints and what they are for:

- `GET /history/topics`
  - Topic catalog. Use this to understand the terrain before reading deeply.
- `GET /history/topics/:topicId`
  - Topic metadata. Useful when one topic already looks promising and you want the stable summary.
- `GET /history/topics/:topicId/transcript`
  - Full topic transcript with pagination. This is the main body-reading endpoint.
- `GET /history/messages`
  - Cross-topic message stream. Use this when you want to study a time window across multiple topics instead of staying inside one thread.
- `GET /history/messages/:messageId`
  - Full detail for one message.
- `GET /history/messages/:messageId/context`
  - A local context window around one message. Good for validating a search hit without loading the whole transcript.
- `POST /history/messages/batch`
  - Batch fetch multiple messages by id. Good when you already have several candidate evidence points and want to compare them together.
- `GET /history/search/messages`
  - Message-level search.
  - Hits include `snippet`, full `mainText`, `createdAt`, and structural `annotations`.
  - Supports simple free text through `q`, and structured search through `phrase`, `allOf`, `anyOf`, `exclude`, `sort`, `order`, and optional `deduplicate`.
  - `returnMode=query` keeps the old hit list response.
  - `returnMode=round` returns grouped rounds with `matchedMessages` plus expanded `messages`.
  - `returnMode=topic` returns grouped topics with `matchedMessages` plus full topic `messages`.

Use these APIs as building blocks.
Do not assume any single endpoint is the "correct" research path.

When searching:

- Use `q` for the simplest substring search.
- Use `phrase` for an exact phrase.
- Use `allOf` when several terms must all appear.
- Use `anyOf` when any one of several terms can appear.
- Use `exclude` to cut obvious noise.
- Use `deduplicate=true` when repeated content across topics is wasting your result budget.
- Use `sort=createdAt&order=asc` when you want a timeline.
- Use `sort=relevance` when you want the strongest matches first.
- Use `returnMode=round` when the match itself is too thin and you want the containing round immediately.
- Use `returnMode=topic` only when you intentionally want whole-topic expansion.
- Keep `deduplicate=true` only with `returnMode=query`.

## Scripts

- `scripts/cherry_history_client.py`
  - Minimal local HTTP client for the history API.
  - Supports env overrides and local connection-profile auto-discovery.
- `scripts/analyze_chat_history.py`
  - Exports a raw local research workspace with topic catalogs, readable transcripts, and optional search results.
  - Search exports support `--search-return-mode query|round|topic`.
  - This script does not generate conclusions. It prepares source material for the model to study.

## Example

```bash
python scripts/analyze_chat_history.py \
  --topic-limit 20 \
  --search-return-mode round \
  --search "AI" \
  --search "焦虑" \
  --output-dir /tmp/cherry-chat-research
```

The command prints the workspace directory.
From there, read `catalog.md`, inspect `topics/*.md`, and write the final report yourself.

## Safety

- Avoid clinical diagnosis unless the user explicitly asks for that level of framing and the evidence is unusually strong.
- Avoid pretending certainty where there is only resonance or pattern.
- Stay close to the material.
- Prefer honest, vivid interpretation over empty caution language.
