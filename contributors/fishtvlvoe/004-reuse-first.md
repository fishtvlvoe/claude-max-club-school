---
layout: default
title: "先考古再動手：reuse-first 原則"
parent: fishtvlvoe
grand_parent: 貢獻者
permalink: /contributors/fishtvlvoe/004-reuse-first/
date: 2026-05-01
still_relevant: true
---

# 先考古再動手：reuse-first 原則

## 問題

Claude 收到「實作 X 功能」的指令後，通常會直接開始寫新代碼，而不是先搜尋 codebase 裡是否已有類似的東西。

結果：
- 重複實作了現有的 helper 函式
- 繞過了已存在的 API procedure，直接用更底層的方式做
- 建立了只有靜態 UI 沒有接實際 API 的殼
- 和既有代碼的命名/架構不一致，之後難以維護

## 原因

Claude 傾向從頭建立清晰、完整的解法，而不是先花時間了解既有代碼。從頭寫感覺比讀懂舊代碼再接進去更可預期，Claude 會選阻力最小的路。

## 解決方案

在 CLAUDE.md 強制要求「動手前先考古」，把搜尋步驟變成前置條件，而不是可選的步驟。

## 可複用的 CLAUDE.md 規則

```markdown
## Reuse-First：動手前強制考古

寫任何代碼前，MUST 先做以下步驟：
1. grep 現有代碼找類似 pattern（函式名、API 路徑、型別名）
2. 確認是否已有對應的 API procedure 或 helper
3. 確認 UI 部分有沒有現成可複用的組件

考古完成後，MUST 主動告訴我：
- 「這些不用做（已有）」
- 「這些可以直接用（路徑/函式名）」
- 「只有這些需要新寫」

禁止：從頭寫現有的東西、只做靜態 UI 不接 API 就標「完成」。
```
