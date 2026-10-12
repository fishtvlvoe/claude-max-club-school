---
layout: default
title: fishtvlvoe
parent: 貢獻者
has_children: true
---

# Fish（老魚）

## 關於我

- **身份**：一人公司經營者 / 獨立開發者
- **工作內容**：經營 BuyGo 電商管理系統（WordPress 外掛），台灣在地電商 SaaS
- **技術棧**：WordPress + PHP、Next.js + Supabase、多模型 AI 協作框架（GSD / Spectra）
- **用 Claude Code 做什麼**：外掛開發、多模型 Agent 協作流程設計、SDD（Spec-Driven Development）
- **使用的 Claude 方案**：Claude MAX
- **用了多久**：超過 6 個月，日均使用

## 我的經驗文章

### 工具使用與效率
- [工具失敗時靜默試備案，別打斷用戶]({{ site.baseurl }}{% link contributors/fishtvlvoe/001-silent-retry-before-interrupt.md %})：遇阻先試 B/C/D，全部失敗才回報
- [需要 API key 先查 .env，不問用戶]({{ site.baseurl }}{% link contributors/fishtvlvoe/009-check-env-before-asking.md %})：環境變數在那裡，自己找

### 診斷與驗證
- [不確定的事自己查，禁止叫用戶代勞驗證]({{ site.baseurl }}{% link contributors/fishtvlvoe/002-research-before-asking-user.md %})：有工具能找到答案就不要問
- [驗證要跑出實際結果，build 通過不代表功能正確]({{ site.baseurl }}{% link contributors/fishtvlvoe/006-verification-means-actual-run.md %})：build pass ≠ feature correct

### 自動化原則
- [需要重複做的操作必須自動化]({{ site.baseurl }}{% link contributors/fishtvlvoe/003-automate-repetitive-operations.md %})：相同流程第二次就要自動化

### 協作與溝通
- [完成每個階段後主動告知下一步]({{ site.baseurl }}{% link contributors/fishtvlvoe/004-proactively-tell-next-steps.md %})：不要做完就停在那裡等問
- [討論中一題一題等確認，不要擅自跳下一步]({{ site.baseurl }}{% link contributors/fishtvlvoe/010-one-question-at-a-time.md %})：用戶沒說好就不要繼續

### 規則遵守與品質
- [碰到程式碼風格衝突必須明確選邊]({{ site.baseurl }}{% link contributors/fishtvlvoe/005-code-style-must-choose-one.md %})：禁止悄悄混用兩種風格
- [先考古再動手：reuse-first 原則]({{ site.baseurl }}{% link contributors/fishtvlvoe/007-reuse-first-grep-before-coding.md %})：寫代碼前先 grep 現有 pattern
- [派工後自己驗收，不把球丟回給用戶]({{ site.baseurl }}{% link contributors/fishtvlvoe/008-complete-delegation-self-verify.md %})：你派的工你自己收尾
