---
layout: default
title: "程式碼風格衝突：必須明說選哪一種，禁止混用"
parent: fishtvlvoe
grand_parent: 貢獻者
permalink: /contributors/fishtvlvoe/003-pick-one-code-style/
tags: [程式碼品質, 風格一致性]
date: 2026-05-15
still_relevant: true
---

# 程式碼風格衝突：必須明說選哪一種，禁止混用

## 問題

同一個 codebase 裡有兩種風格（例如：有些地方用 `camelCase`，有些用 `snake_case`），Claude 會說「為了相容性，兩種都保留」，結果新寫的代碼繼續混用，問題越來越嚴重。

更隱性的版本：Claude 不說話，直接在新檔案用 A 風格、舊檔案用 B 風格，讓整個 codebase 越來越亂。

常見的衝突類型：
- 命名：`camelCase` vs `snake_case` vs `PascalCase`
- 縮排：tab vs 2 spaces vs 4 spaces
- 條件結構：early return vs 巢狀 if
- 字串：單引號 vs 雙引號

## 原因

Claude 不想「擅自做決定」，所以用「兩邊都保留」當藉口迴避選擇。這是一種偽中立——實際上是把決策問題留著讓 codebase 腐爛。

## 解決方案

遇到風格衝突時，強制走以下流程：

1. **先查設定檔**：看 `.editorconfig`、`.prettierrc`、`phpcs.xml` 等，有設定就跟著走
2. **看佔多數的**：用 `grep` 或 `ripgrep` 統計兩種風格各出現幾次，跟多數走
3. **無法決定就直接問**：「我看到 A 和 B 兩種風格，你要用哪一種？」——只問這一個問題，不要問其他的

決定後，新代碼全部統一，不接受「這個檔案用 A，那個檔案用 B」。

## 可複用的 CLAUDE.md 規則

```markdown
## 程式碼風格

- 看到兩種風格衝突時，禁止「兩邊都保留」：
  1. 先查 .editorconfig / .prettierrc / linter 設定
  2. 沒有設定 → 統計哪種佔多數，跟著走
  3. 無法判斷 → 問我「A 還是 B？」，等我答才動手
- 禁止：「為了相容性保留兩種」、不同檔案用不同風格
```
