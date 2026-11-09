---
layout: default
title: "主 AI 工具額度用完時，立刻切換備援工具，不要乾等"
parent: fishtvlvoe
grand_parent: 貢獻者
tags: [多模型協作, 工具備援, 效率, CLAUDE.md規則]
date: 2026-08-28
still_relevant: true
permalink: /contributors/fishtvlvoe/054-backup-ai-tool-on-quota-exhausted/
---

# 主 AI 工具額度用完時，立刻切換備援工具，不要乾等

## 問題

長時間跑任務時，Claude 或其他 AI coding tool 突然跳出「你的用量上限已達到」（You've hit your usage limit）。Claude 的反應是：「額度用完了，請等 X 小時後再試」。

然後整個工作停下來，等待重置時間，幾個小時的計算資源白白浪費。

## 原因

Claude 沒有被告知「有備援工具可以切換」，所以預設行為是回報瓶頸並等待用戶決策。實際上，AI 工具市場上有多個可選（Claude Code、Cursor、GitHub Copilot、各種 AI CLI），遇到額度限制可以立刻切換接手。

## 解決方案

提前規劃備援 AI 工具，並知道如何做任務交接：

**1. 建立備援工具清單**

確認開發機上已安裝且已登入的 AI coding tools：
```bash
which claude      # Claude Code CLI
which cursor      # Cursor AI
which copilot     # GitHub Copilot CLI  
which aider       # Aider
```

**2. 切換前先記錄當前狀態**

```bash
# 確認未提交的工作
git status --short
git diff --stat

# 建立 WIP 存檔點，不要讓中斷的工作丟失
git add -A && git commit -m "wip: pre-handoff checkpoint"
```

**3. 用完整 context 交接給備援工具**

切換到備援工具時，交接 prompt 要包含：
- 整體任務目標
- 已完成的部分
- 卡住的地方（如果有）
- 未提交的檔案清單
- 驗收標準

**注意**：在同一個工作目錄繼續，不要開新的，避免上下文脫節。

## 可複用的 CLAUDE.md 規則

```markdown
- 如果我的用量額度用完（出現 "You've hit your usage limit"）：
  1. 立刻用 `git add -A && git commit -m "wip: pre-handoff"` 存檔
  2. 告知我可以切換到哪個備援工具繼續
  3. 提供完整的交接 context（已完成/卡住/下一步/驗收標準）
  不要說「請等 X 小時」，要說「可以切換到 [備援工具] 繼續，context 如下」。
```
