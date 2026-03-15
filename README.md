# cherry-chat-research

[English](./README.en.md)

`cherry-chat-research` 是一个用来研究 Cherry Studio 聊天数据的 skill。

它不是 Cherry Studio 里的一个页面，也不是内置报表。  
它是给 `Codex`、`Claude Code` 这类支持 skills 的 agent 用的。只要 agent 能调用 skills，并且能访问 Cherry 暴露出来的聊天数据接口，它就能工作。

你可以把它理解成一个“聊天历史研究员”：

- 它会自己看 topic
- 自己搜索线索
- 自己读 transcript
- 最后再决定怎么写报告

它不是那种固定输出几张图表、几段统计结论的工具。它更适合做深度研究、整理脉络、写长文、做画像、做洞察。

## 这东西能做什么

它适合拿来做这些事：

- 回看最近一段时间自己反复在想什么
- 研究某个阶段的关注点、矛盾、变化和长期母题
- 让 agent 基于聊天历史写用户画像、洞察报告、长文总结
- 把聊天记录整理成一个可继续研究的 workspace

如果你想要的只是“这个月聊了多少次、关键词前十是什么”，它也能配合做，但这不是它最擅长的方向。

## 它不是只能在 Cherry Studio 里用

这点很重要。

这个 skill 的研究对象是 **Cherry Studio 里的聊天数据**，但 skill 本身并不属于 Cherry Studio 应用内部。

它的工作方式是：

1. Cherry Studio 负责把聊天数据通过本地 `/v1/history` API 暴露出来
2. 外部 agent 通过这个 skill 去读这些数据
3. agent 自己决定怎么研究、怎么组织、怎么输出

所以更准确地说，它是：

- 一个面向 agent 的技能
- 一个以 Cherry 聊天数据为研究材料的技能

而不是“Cherry Studio 里的一个功能页”。

## 使用前准备

在开始之前，你需要先保证 Cherry Studio 的本地 API 是开着的。

操作路径：

- `设置 -> API 服务器`

确认两件事：

- 服务已经启动
- 你能看到本地地址和 API 密钥

如果你用的是 `ifastcc/cherry-studio` 这个 fork，通常不用手动填环境变量。  
这个 skill 会优先自动发现 Cherry 写出的本地连接配置文件。

## 安装

```bash
npx skills add ifastcc/cherry-chat-research --skill cherry-chat-research -a codex -a claude-code
```

装完之后，你就可以直接让 agent 去研究你的 Cherry 聊天历史。

## 怎么用

最简单的用法，不是手动跑脚本，而是直接对 agent 说需求。

比如：

- “研究一下我最近一个月在 Cherry Studio 里的聊天记录，看看我反复在纠缠什么问题。”
- “基于我的聊天历史，帮我写一份更像人物侧写的报告，不要做成统计报表。”
- “看看我最近的兴趣和焦虑点有没有明显变化。”

这个 skill 更适合开放式任务。  
你不用一开始就把报告结构定死，agent 会自己去看 topic、搜线索、读 transcript，然后再组织输出。

## 怎么配置

大多数情况下，不需要手动配置。

如果自动发现失败，再手动指定：

```bash
export CHERRY_API_BASE_URL=http://127.0.0.1:23333/v1
export CHERRY_API_KEY=your_api_key
```

也可以直接指定连接文件：

```bash
export CHERRY_API_CONNECTION_FILE=/absolute/path/to/api-server.json
```

## 如果你想自己导出研究材料

仓库里也带了一个脚本，可以把聊天数据导出成一个适合继续阅读和研究的 workspace。

示例：

```bash
python .agents/skills/cherry-chat-research/scripts/analyze_chat_history.py \
  --topic-limit 20 \
  --search "AI" \
  --search "焦虑" \
  --output-dir /tmp/cherry-chat-research
```

导出后你会得到：

- topic catalog
- 可读的 transcript
- search 结果
- 一个可以继续交给 agent 深挖的研究目录

## 仓库结构

真正的 skill 源码在这里：

```text
.agents/skills/cherry-chat-research/
```

主要文件：

- `SKILL.md`：skill 主说明
- `agents/openai.yaml`：UI 元数据
- `scripts/cherry_history_client.py`：本地 API 客户端
- `scripts/analyze_chat_history.py`：研究工作区导出脚本

## 适合谁

如果你只是想找一个通用提示词仓库，这个项目可能不适合你。  
如果你想让 agent 真正去研究 Cherry Studio 里的聊天数据，这个 skill 会更对路。

## License

AGPL-3.0
