# cherry-chat-research

[English](./README.en.md)

`cherry-chat-research` 是一个给 agent 用的 skill，用来研究 Cherry Studio 里的聊天记录。

工作原理：Cherry studio负责把聊天数据通过本地 API 暴露出来，agent 通过这个 skill 去读数据、自己做调研、再决定怎么写结果。

## 先说清楚：它只支持这个 fork

这个 skill 只支持 [`ifastcc/cherry-studio`](https://github.com/ifastcc/cherry-studio)。

因为依赖这个 cherry studio 的 fork 版本 提供的本地 `/v1/history` API 。官方 Cherry Studio 没有这套接口，所以直接装这个 skill 是用不起来的。

## 它能拿来做什么

适合这些场景：

- 回看最近一段时间自己反复在想什么
- 让 agent 研究你的长期关注点、矛盾、变化和情绪线索
- 基于聊天历史写人物侧写、洞察报告、长文总结
- 把聊天记录导出成一个可继续研究的 workspace

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

- “调用cherry-chat-research 针对我的聊天数据做一个深度研究报告“
- “研究一下我最近一个月在 Cherry 里的聊天记录，看看我反复在纠缠什么问题。”
- “基于我的聊天历史写一份人物侧写的报告”
- “看看我最近的兴趣、焦虑点和表达方式有没有变化。”


## License

AGPL-3.0
