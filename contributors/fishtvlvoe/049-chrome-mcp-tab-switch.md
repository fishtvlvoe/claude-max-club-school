---
layout: default
title: "Chrome MCP 截圖報 extension URL 權限錯誤，越 debug 越卡"
parent: fishtvlvoe
grand_parent: 貢獻者
tags: [Chrome-MCP, 工具使用]
date: 2026-08-10
still_relevant: true
---

# Chrome MCP 截圖報 extension URL 權限錯誤，越 debug 越卡

## 問題

Chrome MCP 截圖時偶發報錯：

```
Cannot access a chrome-extension:// URL of different extension
```

這個錯誤出現後，繼續在同一個 tab 操作，錯誤反覆出現，越試越卡，
最後整個操作流程就卡死了。

## 原因

這是 Chrome Extension 安全策略的限制，是 Chrome 本身的行為，不是代碼問題。

同一個 tab 持續操作一段時間後（或 tab 狀態變複雜），
截圖操作可能觸碰到 extension 的跨 URL 存取限制。
這個問題本身無法用 debug 解決——越試越壞。

## 解決方案

**遇到這個錯誤，直接換新 tab，不要 debug**：

1. 用 `tabs_create_mcp` 開新分頁
2. 重新 navigate 到目標 URL
3. 在新分頁繼續操作

```
# 開新分頁
tabs_create_mcp url="about:blank"

# 重新導航到目標頁面
navigate url="http://localhost:3000/dashboard"

# 繼續原本的操作
read_page filter=interactive
```

這個方法 99% 能繞過這個錯誤。不需要重啟 Chrome、不需要清快取、不需要找原因。

## 可複用的 CLAUDE.md 規則

把這段貼進你的 CLAUDE.md：

```markdown
- Chrome MCP 截圖報「Cannot access a chrome-extension:// URL of different extension」→ 立刻換新 tab（tabs_create_mcp + navigate），不要 debug。同一 tab 持續操作後偶發，換 tab 即解，無需其他處理。
```
