---
layout: default
title: "「完成」不等於真的跑出結果"
parent: fishtvlvoe
grand_parent: 貢獻者
permalink: /contributors/fishtvlvoe/003-verify-means-run/
date: 2026-05-01
still_relevant: true
---

# 「完成」不等於真的跑出結果

## 問題

Claude 寫完代碼，跑了 `npm run build` 或 `tsc --noEmit` 通過，就回報「功能完成」。

但 build 通過只代表語法沒錯，不代表功能正確。常見的情況：
- API 打出去有 200 但回傳格式不對
- 資料庫查詢跑起來但結果是空的
- PDF 生成函式可以呼叫但輸出的檔案是壞的
- UI 可以點擊但行為跟預期不符

Claude 沒有真的走過一遍功能，只是確認代碼「沒有語法錯誤」。

## 原因

Verifying = 跑 build 是最省力的驗證方式，而且有明確的 pass/fail 訊號。Claude 容易把「沒有報錯」等同於「功能正確」，因為做真正的行為驗證（呼叫函式取得輸出、打 API 看回傳、操作 UI 截圖）需要更多步驟，也需要主動去設計驗證情境。

## 解決方案

在 CLAUDE.md 明確定義「驗證」的標準：MUST 有實際的輸出或觀察，不能只靠 build 通過。

## 可複用的 CLAUDE.md 規則

```markdown
## 驗證標準（build 通過不等於完成）

任何功能完成後，MUST 做以下其中一種行為驗證才能回報「完成」：
1. 寫腳本用假資料呼叫函式 → 取得實際輸出（檔案/JSON/截圖）存 /tmp/
2. 啟動 dev server → 操作 UI → 截圖每個畫面確認行為
3. curl / fetch 打 API → 確認回傳格式和內容正確

禁止：
- 只跑 build / typecheck 就說「完成」
- 說「應該可以正常運作」沒有實際驗證
- 要求使用者「你去試試看」把驗證責任丟回去
```
