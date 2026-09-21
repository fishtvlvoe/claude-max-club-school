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

### Token 效率與工具使用
- [大檔案讀取：先 Grep 定位再用 offset/limit]({{ site.baseurl }}{% link contributors/fishtvlvoe/001-read-large-files-efficiently.md %})：超過 100 行的檔案不要整個讀入

### 驗證與品質
- [Build 通過 ≠ 功能正確：驗證必須實際執行]({{ site.baseurl }}{% link contributors/fishtvlvoe/002-validate-with-real-execution.md %})：build 成功不代表行為正確
- [Bug 修復有正確流程：蒐集→列原因→排除→確定根因→修復→自驗]({{ site.baseurl }}{% link contributors/fishtvlvoe/005-bug-diagnosis-flow.md %})：不要猜，要診斷
- [自動化腳本失敗：先記錄實際狀態再決定是否重試]({{ site.baseurl }}{% link contributors/fishtvlvoe/010-automation-debug-log-first.md %})：盲目 retry 沒有意義

### 程式碼品質
- [程式碼風格衝突：必須明說選哪一種，禁止混用]({{ site.baseurl }}{% link contributors/fishtvlvoe/003-pick-one-code-style.md %})：兩邊都保留等於沒解決
- [寫代碼前先考古：禁止重複造輪子]({{ site.baseurl }}{% link contributors/fishtvlvoe/006-reuse-first.md %})：先 grep 現有 pattern 再動手

### 協作與溝通
- [任務完成後必須主動告知下一步]({{ site.baseurl }}{% link contributors/fishtvlvoe/004-proactive-next-steps.md %})：不要讓用戶一直問「然後呢」
- [不確定的事自己查完再說，禁止叫用戶試試看]({{ site.baseurl }}{% link contributors/fishtvlvoe/009-investigate-before-asking.md %})：有工具能做就自己做

### 專案初始化與平台整合
- [建 Repo 的正確順序：基礎設施先行]({{ site.baseurl }}{% link contributors/fishtvlvoe/007-repo-init-order.md %})：.gitignore、LICENSE、README 先，功能後
- [加平台設定前必須確認版本限制]({{ site.baseurl }}{% link contributors/fishtvlvoe/008-verify-platform-version.md %})：GitHub Pages 是 Jekyll 3.10，不是 4.x
