---
layout: default
title: "Chrome MCP find() 的 ref 硬編碼號碼，跨 batch 就失效"
parent: fishtvlvoe
grand_parent: 貢獻者
tags: [Chrome-MCP, 效率, 測試驗證]
date: 2026-08-10
still_relevant: true
---

# Chrome MCP find() 的 ref 硬編碼號碼，跨 batch 就失效

## 問題

用 Chrome MCP 的 `find()` 找到元素後，下一步 action 用 "ref_61"、"ref_79" 這樣猜測的 ref ID 號碼，
執行後報錯「element not found」或操作到錯誤元素。

## 原因

`find()` 每次執行回傳的 ref 號碼是不固定的，不是從 1 開始遞增的固定值。
頁面狀態改變、navigation、甚至只是時間過去，ref 號碼就可能不同。

硬編碼 ref 號碼（猜測 "ref_61"）必定出錯。

## 解決方案

**同一個 batch 內**：`find` 後直接從輸出取得 ref，在同 batch 後續 action 使用。

```
# 正確做法：find → 從輸出取 ref → 同 batch 用該 ref
browser_batch:
  - find selector=".submit-btn"    # 輸出: ref_42
  - click ref=ref_42               # 用剛剛得到的 ref，不猜
```

**跨 batch 保存 ref**：先用 `javascript_tool` 查元素的固定屬性（id、data attribute），
再用 `find` 搭配這些屬性，不要試圖記住 ref 號碼。

```javascript
// 查元素的固定 id
document.querySelector('.submit-btn').id  // "form-submit-btn"
// 下次用: find selector="#form-submit-btn"
```

## 可複用的 CLAUDE.md 規則

把這段貼進你的 CLAUDE.md：

```markdown
- Chrome MCP find() 返回 ref 後，MUST 在同 batch 直接用該 ref；禁止猜測或硬編碼 ref_N 號碼。跨 batch 需要 ref 時，先用 javascript_tool 取元素的固定 id/attribute，再重新 find。
```
