---
layout: default
title: "建 Repo 的正確順序：基礎設施先行"
parent: fishtvlvoe
grand_parent: 貢獻者
permalink: /contributors/fishtvlvoe/007-repo-init-order/
tags: [專案初始化, 最佳實踐]
date: 2026-05-15
still_relevant: true
---

# 建 Repo 的正確順序：基礎設施先行

## 問題

Claude 幫忙建新專案時，急著開始寫核心功能代碼，跳過了基礎設施的建立：
- 沒有 `.gitignore`：`node_modules/`、`.env`、`.DS_Store` 都被 commit 進去
- 沒有 `LICENSE`：或是用了不適合的授權（內容專案用 MIT，代碼專案用 CC）
- 沒有 `README`：別人（或三個月後的自己）不知道這個 repo 是做什麼的
- 依賴版本沒有固定：`package.json` 裡用 `^1.2.3`，三個月後 `npm install` 裝了不相容的版本

還有更常見的「先寫再補」問題：把所有功能代碼寫完，最後才說「你要建一個 README 嗎」——這時候補的 README 往往是敷衍的。

## 原因

Claude 的注意力集中在「用戶要求的功能」上，基礎設施看起來是次要的、之後再補也沒關係。但實際上基礎設施一旦跳過，日後很容易出問題（特別是 .gitignore 和版本固定）。

## 解決方案

建 repo 後，第一個 commit 的順序：

1. **.gitignore**：先建，避免把不該 commit 的東西塞進 git 歷史
2. **LICENSE**：選正確的授權類型（代碼用 MIT/Apache，文字內容用 CC）
3. **README**：簡短說明這個 repo 是什麼、怎麼跑
4. **依賴管理**：lock file（`package-lock.json`、`composer.lock`）且版本號固定（`1.2.3` 不是 `^1.2.3`）

這四件事做完，才開始寫核心功能。

## 可複用的 CLAUDE.md 規則

```markdown
## 新 Repo 初始化順序

建新 repo 後，第一個 commit 必須包含：
1. .gitignore（根據專案類型選對模板）
2. LICENSE（代碼用 MIT，內容用 CC BY-SA 4.0）
3. README（至少要有：這是什麼、怎麼安裝/跑）
4. 依賴管理：版本號固定（不用 ^ 或 ~）

禁止：先寫功能代碼再補基礎設施
```
