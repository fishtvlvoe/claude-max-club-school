---
layout: default
title: "看到兩種風格要明說選哪個"
parent: fishtvlvoe
grand_parent: 貢獻者
permalink: /contributors/fishtvlvoe/002-explicit-code-style/
date: 2026-04-01
still_relevant: true
---

# 看到兩種風格要明說選哪個

## 問題

同一個 codebase 裡如果存在兩種命名或格式風格（例如 camelCase 和 snake_case 混用），Claude 不會主動選一個，而是兩邊都保留，甚至在同一個 PR 裡繼續新增兩種風格的代碼。

常見情境：
- 有些函式用 camelCase、有些用 snake_case，Claude 新增時也跟著混
- 有些地方用 tab 縮排、有些用 space，Claude 新增時也混
- 有些 if 用 early return、有些用 nested if，Claude 跟著混

結果是風格越來越亂，也更難用自動化工具（prettier、eslint）整理。

## 原因

Claude 觀察到「兩種都在用」，認為這是「這個專案的風格」，所以跟著兩邊都用。Claude 的預設傾向是「相容」、不打破現狀，而不是「統一」。

## 解決方案

在 CLAUDE.md 要求 Claude 遇到風格衝突時，MUST 明確選一種並說明理由，而不是悄悄混用。選擇原則：先看 `.editorconfig` / `prettier.config` / `.eslintrc`，沒有就看哪種佔多數，都無法判斷才問你。

## 可複用的 CLAUDE.md 規則

```markdown
## 風格衝突處理規則

遇到 codebase 有兩種風格衝突（命名/縮排/if 寫法等）時，MUST：
1. 先查 .editorconfig / prettier.config / .eslintrc / .stylelintrc
2. 沒有設定 → 看哪種在現有代碼中佔多數
3. 都無法判斷 → 直接問我「A 還是 B？」

選定後，在這次工作中全部統一用那一種。
禁止：「兩個都保留」、「這檔案用 A 那檔案用 B」的悄悄混用。
如有選擇，必須在回應中說明「選了 X，因為 Y」。
```
