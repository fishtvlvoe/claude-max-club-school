---
layout: default
title: "驗證要跑出實際結果，build 通過不代表功能正確"
parent: fishtvlvoe
grand_parent: 貢獻者
tags: [驗證流程, 測試, 品質保證]
date: 2026-10-12
still_relevant: true
---

# 驗證要跑出實際結果，build 通過不代表功能正確

## 問題

Claude 完成功能實作後，只跑 `npm run build` 或 `tsc --noEmit` 通過就回報「完成」，沒有實際驗證功能行為是否正確。

常見案例：
- 實作了 PDF 生成功能，但只確認 build 成功，沒有實際生成一份 PDF 確認格式
- 修改了 API 端點，只確認 TypeScript 型別正確，沒有 curl 打一次確認回傳格式
- 寫了資料處理邏輯，只確認 lint 通過，沒有用假資料跑一次看輸出

用戶收到的是「build 成功」，但功能實際上是壞的。

## 原因

Build 成功代表程式碼語法正確，但不代表執行時的行為符合預期。Claude 混淆了「靜態檢查通過」和「行為驗證通過」。

## 解決方案

任何功能完成後，**必須**做行為驗證，至少選一種：
1. 寫腳本用假資料呼叫函式，確認輸出的實際內容（PDF/JSON/圖片存 `/tmp/`）
2. 啟動 dev server，用瀏覽器 MCP 操作 UI，截圖每個畫面
3. curl/fetch 打 API，確認回傳格式和內容

**build 通過 ≠ 功能正確**。只跑 build 就回報完成 = 把驗證工作丟給用戶。

## 可複用的 CLAUDE.md 規則

把這段貼進你的 CLAUDE.md：

```markdown
- build 通過 ≠ 功能正確。任何功能完成後 MUST 做行為驗證。
- 驗證方式（選一）：1) 腳本呼叫函式看實際輸出 2) dev server + 瀏覽器操作截圖 3) curl/fetch 確認 API 回傳。
- 禁止只跑 build/lint/typecheck 就回報「完成」。
- 禁止說「需要你自己跑一次確認」把驗證丟回用戶。
```
