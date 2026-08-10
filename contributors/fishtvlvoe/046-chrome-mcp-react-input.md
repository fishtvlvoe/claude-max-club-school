---
layout: default
title: "用 Chrome MCP 填 React 表單，值寫進去但 state 沒更新"
parent: fishtvlvoe
grand_parent: 貢獻者
tags: [Chrome-MCP, React, 測試驗證]
date: 2026-08-10
still_relevant: true
---

# 用 Chrome MCP 填 React 表單，值寫進去但 state 沒更新

## 問題

用 Chrome MCP 操作 React 受控表單（controlled input）時，
用 `computer.type` 或直接設 `element.value` 輸入文字，
看起來 input 有值了，但送出後什麼都沒發生，或者 UI 的 state 完全沒更新。

常見症狀：
- `input.value` 查到有值，但 React state 還是空的
- 按送出按鈕後 API 收到的是空字串
- 表單看起來填了，但驗證還是報「必填」

## 原因

React 的受控元件是透過 `onChange` 事件驅動 state 更新的。
`computer.type` 只模擬鍵盤按鍵事件（keydown/keyup/keypress），
而直接設 `element.value` 完全繞過事件系統。

React 18+ 的合成事件不響應 fake 鍵盤事件，
需要透過原生 DOM property descriptor 的 setter 才能觸發 onChange。

## 解決方案

使用 `nativeInputValueSetter` 方式設值：

```javascript
// Chrome MCP javascript_tool 執行
const el = document.querySelector('input[name="email"]')
const nativeSetter = Object.getOwnPropertyDescriptor(
  HTMLInputElement.prototype,
  'value'
).set

nativeSetter.call(el, 'test@example.com')
el.dispatchEvent(new Event('input', { bubbles: true }))
```

或者用 Chrome MCP 的 `form_input` + ref（MCP 內部自動用 nativeInputValueSetter）——
先用 `read_page filter=interactive` 拿到 input 的 ref，
再用 `form_input ref=<ref> value=<值>` 填值，React state 會正確更新。

## 可複用的 CLAUDE.md 規則

把這段貼進你的 CLAUDE.md：

```markdown
- Chrome MCP 填 React controlled input MUST 用 `form_input` + ref（MCP 自動用 nativeInputValueSetter）；或 JS：`const s=Object.getOwnPropertyDescriptor(HTMLInputElement.prototype,'value').set; s.call(el,val); el.dispatchEvent(new Event('input',{bubbles:true}))`。禁止 `computer.type` 或直接設 `.value`。
```
