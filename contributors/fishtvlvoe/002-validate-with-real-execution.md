---
layout: default
title: "Build 通過 ≠ 功能正確：驗證必須實際執行"
parent: fishtvlvoe
grand_parent: 貢獻者
permalink: /contributors/fishtvlvoe/002-validate-with-real-execution/
tags: [驗證流程, 完成定義]
date: 2026-05-17
still_relevant: true
---

# Build 通過 ≠ 功能正確：驗證必須實際執行

## 問題

Claude 完成一個功能後，跑一下 `npm run build` 或 `php -l` 沒有報錯，就回報「完成，請確認」。但 build 通過只代表語法正確，不代表功能邏輯正確。

常見的「假完成」狀況：
- PDF 匯出功能：build 通過，但實際產生的 PDF 是空白頁
- API 接口：build 通過，但打 curl 回傳的格式不對
- UI 元件：build 通過，但在瀏覽器打開是空白畫面

最後用戶要自己打開瀏覽器驗證，發現壞了，等於 Claude 把驗證工作丟回給用戶。

## 原因

Claude 傾向保守地定義「完成」——只要編譯通過就算交差，把實際功能驗證留給用戶。這是一種隱性的「把球丟回去」行為。

部分原因是 Claude 預設認為自己沒有執行環境，但事實上有 Bash 工具、可以啟動 dev server、可以用 curl 打 API。

## 解決方案

任何功能完成後，MUST 自己做行為驗證，至少選一種：

1. **API 驗證**：用 `curl` 或 `fetch` 打 API，確認回傳格式正確
2. **腳本驗證**：寫一個小測試腳本，用假資料呼叫函式，把結果輸出到 `/tmp/` 檢查
3. **UI 驗證**：啟動 dev server，用 Chrome MCP 操作畫面，截圖每個關鍵狀態

「build 通過」只能做為前置檢查，不能做為完成的證明。

## 可複用的 CLAUDE.md 規則

```markdown
## 驗證標準

- **build 通過 ≠ 完成**：任何功能完成後 MUST 自己驗證行為：
  - 後端/API：用 curl 打一次，貼出實際回傳結果
  - 腳本/函式：寫假資料跑一次，輸出到 /tmp/ 讓我看
  - UI：啟動 dev server + 用瀏覽器操作 + 截圖
- 禁止說「你可以自己跑一下看看」——驗證是你的工作，不是我的
```
