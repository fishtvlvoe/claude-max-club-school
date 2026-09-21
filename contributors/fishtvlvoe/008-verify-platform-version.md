---
layout: default
title: "加平台設定前必須確認版本限制"
parent: fishtvlvoe
grand_parent: 貢獻者
permalink: /contributors/fishtvlvoe/008-verify-platform-version/
tags: [平台整合, 版本管理]
date: 2026-05-15
still_relevant: true
---

# 加平台設定前必須確認版本限制

## 問題

Claude 幫忙設定 GitHub Pages + Jekyll 時，按照自己訓練資料裡「最新版本」的文件操作，加了 Jekyll 4.x 才有的設定。但 GitHub Pages 鎖定在 Jekyll 3.10（官方平台限制），結果部署失敗，要花時間回頭拆掉那些不相容的設定。

同樣的問題也發生在：
- Vercel 的 Node.js 版本（預設不是最新版）
- Supabase Edge Functions 的 Deno 版本
- WordPress 支援的 PHP 最低版本
- GitHub Actions 的 runner 環境

## 原因

Claude 的訓練資料傾向最新版本，在不確認目標環境的情況下，會用「最新文件」的寫法。但生產環境往往是老一點的版本，兩者不一致就會出問題。

## 解決方案

任何平台相關設定（CI/CD、雲端部署、靜態網站）開始前，先確認：

1. **目標平台的版本**：查官方文件或設定介面，確認實際運行的版本
2. **功能限制**：該版本支援哪些語法、不支援哪些
3. **不確定就查**：用 Bash 跑 `<platform> --version`，或查官方 changelog

確認完才寫設定。如果確認過程發現版本比想像的舊，主動告知用戶「目標環境是 X 版本，有 Y 功能不能用」。

## 可複用的 CLAUDE.md 規則

```markdown
## 平台版本確認

加任何平台設定前必須先確認目標版本：
- GitHub Pages：Jekyll 3.10（不是 4.x）
- 其他平台：查官方文件的「supported versions」頁面

不確定版本 → 先查，不要猜
確認版本有限制 → 主動告訴我有哪些功能不能用
禁止：直接按最新文件操作，不確認目標環境版本
```
