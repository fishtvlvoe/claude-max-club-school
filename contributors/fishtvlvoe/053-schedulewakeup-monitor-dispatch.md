---
layout: default
title: "派工長時間任務後用 ScheduleWakeup 排定監看"
parent: fishtvlvoe
grand_parent: 貢獻者
permalink: /contributors/fishtvlvoe/053-schedulewakeup-monitor-dispatch/
tags: [多代理架構, 長時間任務, 自動化監控]
date: 2026-08-28
still_relevant: true
---

# 派工長時間任務後用 ScheduleWakeup 排定監看

## 問題

把長時間任務（例如 CI/CD、外部 API 呼叫、另一個代理的執行）丟出去之後，Claude 直接回報「已派工，等完成後告知」，然後就沉默了。

問題是：
- 任務卡住了，沒人知道
- 外部 API 回傳了錯誤，沒人注意
- 額度用完或網路斷線，一樣沉默
- 用戶回來問「做好了嗎」，Claude 只能說「不知道，我等你問」

這是「派完就不管」的行為模式，等於讓用戶去監工。

## 原因

Claude 在派工後沒有持續監控的機制。如果不主動排定喚醒，session 會一直等用戶開口，不會自動去確認任務狀態。

## 解決方案

在派工後立刻用 `ScheduleWakeup` 排定一個合理時間後的喚醒，讓 session 自己回來確認：

1. 估計任務需要多少時間（例如 CI 跑 5 分鐘，就排 300 秒後喚醒）
2. 喚醒時檢查任務狀態（看 log、讀輸出、確認是否完成）
3. 完成了 → 驗收 + 報告
4. 還在跑 → 再排下一次喚醒
5. 卡住了 → 診斷原因 + 通知用戶

監看間隔要匹配實際變化速度。CI 跑 8 分鐘，排一次 480 秒，不要每 60 秒輪詢一次浪費 prompt cache。

注意：`ScheduleWakeup` 是 session-bound，視窗關掉就消失。對於長達數小時的任務，應另外設系統層排程（cron）作為保障。

## 可複用的 CLAUDE.md 規則

```markdown
## 長時間派工後主動監看

派任何長時間任務後，MUST 用 ScheduleWakeup 排定喚醒：
1. 估計完成時間 → 排對應秒數（不要短間隔硬輪詢）
2. 喚醒時：確認狀態 → 完成則驗收、未完成則再排 → 失敗則診斷
3. 驗收只信工具輸出（log/git status/curl），不信代理的文字回報

禁止：派完就沉默，等用戶來問才發現任務早已卡住。
```
