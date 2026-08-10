---
layout: default
title: "Chrome MCP 分多次 find→click，ref 失效 + 多餘 round-trip"
parent: fishtvlvoe
grand_parent: 貢獻者
tags: [Chrome-MCP, 效率]
date: 2026-08-10
still_relevant: true
---

# Chrome MCP 分多次 find→click，ref 失效 + 多餘 round-trip

## 問題

操作 Chrome MCP 時，每個動作都分開 call：
1. `find email-input` → 得到 ref_12
2. （新 call）`click ref_12` → 找不到了
3. `find password-input` → 得到 ref_35
4. （新 call）`type ref_35` → 失效

這樣做不只慢（多次 round-trip），ref 還可能在兩次 call 之間失效。

## 原因

每次獨立的 tool call 之間，頁面狀態可能改變，導致之前的 ref 失效。
而且分多次 call 需要等待每次的 latency，浪費時間。

正確的模式是：一次取得所有需要的 ref，然後一個 batch 完成所有操作。

## 解決方案

**步驟一**：先用 `read_page filter=interactive` 取得頁面上所有可互動元素的 ref。

```
read_page filter=interactive
# 輸出：
# ref_10: input[name="email"] (input)
# ref_11: input[name="password"] (input)  
# ref_12: button[type="submit"] (button)
```

**步驟二**：一個 `browser_batch` 完成填表 + 點擊 + 等待，不再分開 call。

```
browser_batch:
  - form_input ref=ref_10 value="test@example.com"
  - form_input ref=ref_11 value="password123"
  - click ref=ref_12
  - wait_for selector=".dashboard"
```

## 可複用的 CLAUDE.md 規則

把這段貼進你的 CLAUDE.md：

```markdown
- Chrome MCP 操作前先 `read_page filter=interactive` 取得所有 ref，再一個 `browser_batch` 完成所有動作。禁止分多次獨立 call（find→click→find→click），ref 會失效且浪費 round-trip。
```
