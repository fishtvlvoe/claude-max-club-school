---
layout: default
title: "寫代碼前先考古：禁止重複造輪子"
parent: fishtvlvoe
grand_parent: 貢獻者
permalink: /contributors/fishtvlvoe/006-reuse-first/
tags: [代碼品質, 效率, DRY原則]
date: 2026-05-18
still_relevant: true
---

# 寫代碼前先考古：禁止重複造輪子

## 問題

用戶說「幫我加一個匯出 CSV 的功能」，Claude 立刻開始從頭寫新代碼，但其實這個 codebase 三個月前就已經有一個 CSV 匯出的 helper function，只是命名不太顯眼所以沒被找到。

結果：
- 同樣的邏輯在 codebase 裡存在兩份（DRY 違反）
- 新寫的版本可能遺漏舊版本已經處理過的邊界案例
- 維護成本加倍

## 原因

Claude 傾向「從頭寫新代碼」，因為這比搜尋現有代碼更快（對 Claude 來說）。搜尋現有實作需要多幾個 Grep 呼叫，而直接寫感覺更有效率。但這種「效率」是假的，對整體 codebase 是傷害。

## 解決方案

任何寫代碼任務開始前，強制前置搜尋步驟：

1. **Grep 相關關鍵字**：搜尋現有 codebase 有沒有類似 pattern
2. **查同類功能**：同個模組或相鄰模組有沒有做過類似的事
3. **確認 API/函式是否存在**：禁止只做 UI 外殼卻沒接現有 API

完成搜尋後，主動告訴用戶：
- 「這些部分已經有了，不用重做：X」
- 「這些可以直接用現有的：Y」
- 「只有這些是真正需要新寫的：Z」

## 可複用的 CLAUDE.md 規則

```markdown
## Reuse First（先考古再動手）

寫任何代碼前強制前置步驟：
1. Grep 現有 codebase 找類似 pattern
2. 確認同類功能是否已存在
3. 確認 API/函式是否已有（禁止只做 UI 不接 API）
4. 主動告訴我：哪些不用做、哪些直接用、哪些要新寫

禁止：直接從頭寫已存在的功能、只做靜態 UI 沒接 API 就標完成
```
