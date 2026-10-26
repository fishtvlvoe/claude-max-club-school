---
layout: default
title: "開口問之前先問自己能不能自己查"
parent: fishtvlvoe
grand_parent: 貢獻者
permalink: /contributors/fishtvlvoe/006-research-before-asking/
date: 2026-05-01
still_relevant: true
---

# 開口問之前先問自己能不能自己查

## 問題

Claude 在遇到不確定的事時，習慣直接問使用者，而不是先試著用手上的工具自己查清楚。

常見的多餘詢問：
- 「請問這個服務的 API Key 是什麼？」（.env 裡有）
- 「請截圖給我看看頁面長什麼樣」（有瀏覽器工具能自己看）
- 「你那邊報什麼錯？」（log 可以自己讀）
- 「這個功能你想要怎麼設計？」（spec 文件裡有寫）

每次這樣問，都是要使用者切換情境去找資料，再切回來回答。如果 Claude 能自己查，就不應該問。

## 原因

問使用者是最省力的方式：不需要搜尋，不需要判斷，把不確定性直接交出去。Claude 也傾向謹慎，不想猜錯，所以寧可問確認也不自己查。但這個「謹慎」的成本由使用者承擔。

## 解決方案

在 CLAUDE.md 設定前置反思：想問任何問題之前，先問自己「我有工具能找到這個答案嗎？」。如果有工具，就去用，不開口。如果真的查不到，說明「已查了什麼、找不到什麼」，再問。

## 可複用的 CLAUDE.md 規則

```markdown
## 開口問之前的反思規則

想問我任何問題前，MUST 先自問：「我有工具能找到這個答案嗎？」

- API Key / Token → 先 `cat .env` / `cat .env.local` / `cat .env.production`
- 頁面長什麼樣 → 先用瀏覽器工具（Playwright/Chrome MCP）自己看
- Log 報什麼錯 → 先讀 log 檔或執行取得輸出
- Spec 要求什麼 → 先 grep / Read spec 文件

有工具能查 → 自己查，不開口。
查不到才問，且問的時候說明「已查 [工具]，找不到 [原因]」。

禁止說法：「請截圖給我」「請告訴我看到什麼」「請你手動試試看」
```
