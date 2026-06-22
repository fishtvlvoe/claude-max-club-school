---
layout: default
title: "localhost 換專案前先清 Service Worker 和 Cache"
parent: fishtvlvoe
grand_parent: 貢獻者
tags: [診斷不足, CLAUDE.md規則]
date: 2026-05-19
still_relevant: true
permalink: /contributors/fishtvlvoe/045-clear-service-worker-before-test/
---

# localhost 換專案前先清 Service Worker 和 Cache

## 問題

在同一個 localhost port（例如 `:3000`）先後跑不同專案的 dev server 時，瀏覽器可能還在用上一個專案的 Service Worker 和快取。結果出現詭異的行為：`curl` 打 localhost 回傳正確內容，但瀏覽器卻顯示舊頁面或錯誤畫面。

這個 bug 很難發現，因為一切看起來「server 正常運行」，錯誤只出現在瀏覽器端，而且沒有 console error——因為 Service Worker 攔截了請求，靜默返回舊的快取內容。

## 原因

Service Worker 註冊在 origin（protocol + host + port）上，不是綁定在特定專案。當你在 `localhost:3000` 跑專案 A 註冊了 SW，之後在同個 port 換跑專案 B，瀏覽器的 SW 仍然活著，會攔截專案 B 的請求並返回專案 A 的快取內容。

Claude 在 debug 這類問題時，通常會去查 server 端的設定和代碼，但根本原因在瀏覽器端的 SW 快取，完全不在 server 端的管轄範圍。

## 解決方案

每次在同一個 port 切換不同專案的 dev server 時，先清除 Service Worker 和 Cache：

**方法一：DevTools 手動清除**
1. DevTools → Application → Service Workers → Unregister
2. Application → Storage → Clear site data

**方法二：Console 一行清除**
```javascript
(async()=>{
  const r=await navigator.serviceWorker.getRegistrations();
  for(const x of r) await x.unregister();
  const k=await caches.keys();
  for(const c of k) await caches.delete(c);
})()
```

**診斷判斷**：如果 `curl localhost:3000` 正確但瀏覽器不對，十之八九是 Service Worker 攔截。

## 可複用的 CLAUDE.md 規則

```markdown
### localhost dev server 換專案前
- 同個 port 換不同專案的 dev server 前 MUST 清 Service Worker + Cache
- 症狀：curl 對但瀏覽器錯 = Service Worker 攔截
- 清除方式：DevTools → Application → Clear site data
  或 console 執行一行清除腳本
- 每次 `localhost:<port>` 換專案都要先清，不例外
```
