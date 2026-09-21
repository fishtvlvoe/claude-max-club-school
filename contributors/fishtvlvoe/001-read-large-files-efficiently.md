---
layout: default
title: "大檔案讀取：先 Grep 定位再用 offset/limit"
parent: fishtvlvoe
grand_parent: 貢獻者
permalink: /contributors/fishtvlvoe/001-read-large-files-efficiently/
tags: [token效率, 讀檔策略]
date: 2026-05-15
still_relevant: true
---

# 大檔案讀取：先 Grep 定位再用 offset/limit

## 問題

Claude 遇到大型檔案（超過 100 行）時，習慣把整個檔案讀進來，即使只需要修改其中幾行。一個 500 行的設定檔只需要改第 320 行，卻把全部 500 行都讀入 context，白白消耗 token。

更糟的是，若檔案超過 context 視窗，Claude 可能讀到截斷的版本，之後用 Write 覆寫時反而把後半段內容抹掉。

## 原因

Claude 的預設本能是「先把資料讀完整再操作」，就像人類習慣先看完整份文件再找要改的地方。對小檔案這沒問題，但對大檔案這是資源浪費。Read 工具沒有強制限制，不加參數就會從頭讀到尾。

## 解決方案

**兩步走策略**：
1. 先用 **Grep** 搜尋關鍵字，取得目標內容的行號
2. 再用 **Read** 的 `offset`（起始行）和 `limit`（讀取行數）只讀需要的段落

例如要修改某個函式：
```bash
# Step 1: 找行號
Grep("function processOrder", "src/orders.php")
# 結果：第 287 行

# Step 2: 只讀那個段落
Read("src/orders.php", offset=285, limit=30)
```

這樣 token 消耗從 500 行降到 30 行，快 16 倍。

## 可複用的 CLAUDE.md 規則

```markdown
## 大檔案讀取策略

- 任何超過 100 行的檔案，禁止整個讀入：
  1. 先用 Grep 搜關鍵字找行號
  2. 再用 Read 加 offset/limit 只讀需要的段落
- 唯一例外：需要理解整體架構時，且必須在開口前說明「需要讀整個檔案，因為 X」
```
