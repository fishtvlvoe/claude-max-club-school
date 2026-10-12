---
layout: default
title: "碰到程式碼風格衝突必須明確選邊"
parent: fishtvlvoe
grand_parent: 貢獻者
tags: [程式碼風格, 一致性, CLAUDE.md規則]
date: 2026-10-12
still_relevant: true
---

# 碰到程式碼風格衝突必須明確選邊

## 問題

當 codebase 裡存在兩種不一致的風格（例如 camelCase 和 snake_case 混用、tab 和 space 混用），Claude 會「兩邊都保留」或「這個檔案用 A、那個檔案用 B」，悄悄延續混亂狀態。

常見案例：
- 看到有些地方用 `userName` 有些地方用 `user_name`，就混著寫
- 看到有些函式用 early-return，有些用 nested-if，就「為了兼容」兩種都維持
- 明知有 lint 規則衝突，但「先跑通再說」不處理

## 原因

Claude 傾向「不破壞現有代碼」，對於風格不一致的情況，選擇最小改動路徑——但這讓問題越積越深。

## 解決方案

遇到風格衝突時必須明確處理，按優先順序：
1. 查 `.editorconfig`、`prettier.config.js`、`.eslintrc` 等設定檔
2. 統計哪種風格佔多數，跟著佔多數的走
3. 兩者差不多時，直接問用戶「A 還是 B？」

**禁止**：「為了相容性兩個都保留」、混用兩種風格。

## 可複用的 CLAUDE.md 規則

把這段貼進你的 CLAUDE.md：

```markdown
- 碰到 codebase 風格衝突（命名、縮排、結構）時，必須明確選一種，不能悄悄混用。
- 選擇順序：1) 查 .editorconfig/prettier 設定 2) 跟著佔多數的走 3) 無法決定就問用戶「A 還是 B？」
- 禁止：「為了相容性兩個都保留」、不同檔案用不同風格。
```
