# cherry-chat-research

[English](./README.en.md)

`cherry-chat-research` 是一个给 agent 用的 skill，用来研究 Cherry Studio 里的聊天记录。

工作原理很简单：Cherry Studio 负责把聊天数据通过本地 API 暴露出来，agent 通过这个 skill 去读数据、自己做调研、再决定怎么写结果。

## 先说清楚：它只支持这个 fork

这个 skill 只支持 [`ifastcc/cherry-studio`](https://github.com/ifastcc/cherry-studio)。

因为依赖这个 fork 提供的本地 `/v1/history` API 和连接配置文件自动发现。官方 Cherry Studio 没有这套接口，所以直接装这个 skill 是用不起来的。

## 它能拿来做什么

适合这些场景：

- 回看最近一段时间自己反复在想什么
- 让 agent 研究你的长期关注点、矛盾、变化和情绪线索
- 基于聊天历史写人物侧写、洞察报告、长文总结
- 把聊天记录导出成一个可继续研究的 workspace
- 跨 topic 看某一段时间整体在关注什么，而不只盯着单个话题

如果你只想看关键词排行、活跃时段这种统计页，这个 skill 不是为那个方向做的。

## 使用前准备

先在 `ifastcc/cherry-studio` 里打开本地 API：

- `设置 -> API 服务器`

确认这两件事：

- 服务已经启动
- 页面里能看到本地地址和 API 密钥

大多数情况下，不需要再手动配环境变量。这个 skill 会优先自动发现 Cherry 写出的本地连接文件。

## 安装

```bash
npx skills add ifastcc/cherry-chat-research -a codex -a claude-code
```

装完以后，重启对应的 agent。

## 怎么用

直接对 agent 说需求。

例如：

- “调用 cherry-chat-research，针对我的聊天数据做一个深度研究报告。”
- “研究一下我最近一个月在 Cherry 里的聊天记录，看看我反复在纠缠什么问题。”
- “基于我的聊天历史写一份人物侧写的报告。”
- “看看我最近的兴趣、焦虑点和表达方式有没有变化。”
- “从价值观、主要矛盾和关键人物这几个角度读一下我的最近聊天记录。”

默认情况下，不需要你自己先选一堆分析视角。agent 会自己决定从哪些角度读最合适。

如果你确实有偏好，也可以只给一个轻提示，例如：

- “偏行动建议一点”
- “偏自我觉察一点”
- “偏关系和人物影响”
- “偏哲学一点”

## 配置

默认情况下，skill 会自动读取 Cherry 的本地连接文件。

如果你想手动指定，最推荐直接给连接文件：

```bash
export CHERRY_API_CONNECTION_FILE=/absolute/path/to/api-server.json
```

如果自动发现失败，或者你就是想手动覆盖，再指定地址和密钥：

```bash
export CHERRY_API_BASE_URL=http://127.0.0.1:23333/v1
export CHERRY_API_KEY=your_api_key
```

## 这个 skill 现在能用到哪些接口

它会按需要调用这些数据接口：

- `GET /history/topics`
- `GET /history/topics/:topicId`
- `GET /history/topics/:topicId/transcript`
- `GET /history/messages`
- `GET /history/messages/:messageId`
- `GET /history/messages/:messageId/context`
- `POST /history/messages/batch`
- `GET /history/search/messages`

这些接口只是数据入口，不是固定研究流程。怎么查、先看哪里、最后写成什么样，都由 agent 根据任务自己决定。

## License

AGPL-3.0
