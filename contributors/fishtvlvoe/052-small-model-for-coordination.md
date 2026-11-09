---
layout: default
title: "協調型子代理用小模型，讓大模型專注推理"
parent: fishtvlvoe
grand_parent: 貢獻者
permalink: /contributors/fishtvlvoe/052-small-model-for-coordination/
tags: [多代理架構, token效率, subagent]
date: 2026-08-20
still_relevant: true
---

# 協調型子代理用小模型，讓大模型專注推理

## 問題

在多代理工作流中，Claude 會為所有子代理繼承目前 session 的模型（例如 Sonnet 或 Opus）。這包含：

- 監看另一個代理是否完成的「監工」角色
- 輪詢任務狀態的「輪詢者」
- 讀 git diff 然後做簡單比對的「驗收者」

這些角色不需要深度推理，卻用了最貴的模型。這等於讓 Opus 去做 shell script 能做的事。

## 原因

Claude Code 的 Agent tool 預設繼承主 session 的模型。沒有明確指定時，所有子代理都是同一個模型。這對「只需要跑指令 + 讀輸出 + 比對結果」的協調類角色是嚴重浪費。

真正需要大模型的是「寫代碼、做推理、看設計文件做決策」的工作層，不是等待層。

## 解決方案

建立「模型分層」原則：

- **協調 / 監工 / 輪詢 / 驗收**（執行導向，無深度推理）→ 用 Haiku
- **實際開發 / 架構設計 / 複雜分析** → 用 Sonnet 或 Opus

在建立子代理時用 `model: haiku` 參數（或在 agent 定義檔的 frontmatter 裡指定），而不是每次呼叫時手動設定。寫進 agent 定義才是 SSOT，靠對話記憶會在下個 session 失效。

## 可複用的 CLAUDE.md 規則

```markdown
## 子代理模型分層

建立 subagent 時，按工作性質選模型：
- 監工 / 輪詢 / 讀狀態 / 驗收（執行導向，不需推理）→ model: haiku
- 程式開發 / 架構分析 / 複雜判斷 → model: sonnet 或 opus

把模型寫進 agent 定義檔（`.claude/agents/xxx.md` frontmatter），
不要靠對話記憶每次手動指定。
```
