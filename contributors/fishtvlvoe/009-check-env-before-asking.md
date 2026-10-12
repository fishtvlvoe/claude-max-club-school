---
layout: default
title: "需要 API key 先查 .env，不問用戶"
parent: fishtvlvoe
grand_parent: 貢獻者
tags: [環境變數, API key, 自主解決]
date: 2026-10-12
still_relevant: true
---

# 需要 API key 先查 .env，不問用戶

## 問題

Claude 需要某個外部服務的 API key 或 token 時，直接問用戶：「這個服務的 API key 是什麼？」或「你有這個服務的 token 嗎？」

但其實 `.env` 檔案裡通常已經有了。

## 原因

Claude 把「我不知道這個值」等同於「需要問用戶」，沒有先做「查本地設定檔」這個步驟。

## 解決方案

需要任何 API key / token / endpoint 時，按順序查：
1. 先查 `.env`
2. 再查 `.env.local`
3. 再查 `.env.production`（或其他環境對應的 env 檔）

**找到就直接用，找不到才問用戶**——且問的時候要說明「已查 .env、.env.local，沒有找到 X」。

這是「不確定的事自己查」原則的具體化：環境變數在檔案裡，不在用戶腦子裡。

## 可複用的 CLAUDE.md 規則

把這段貼進你的 CLAUDE.md：

```markdown
- API key / token / endpoint 先查 .env，禁止直接問用戶。
- 查詢順序：.env → .env.local → .env.production。
- 找不到才問，且問時說明「已查 .env，沒有 X」。
```
