# Xray-Pro

[English](README.md) | [简体中文](README_zh-Hans.md) | 繁体中文

基於上游最新穩定版本的 Xray 增強版本，主要包含被上游拒絕的功能增強，同時偶爾也包括我們認為需要向後移植的修復和功能。

## 最終用戶使用說明

如果您是最終用戶，希望下載或使用 `Xray 增強版`，以下是相關資源：

- **下載發佈版本**：訪問 [GitHub Releases](https://github.com/Meo597/Xray-Pro/releases) 獲取最新版本。
- **Docker 鏡像**：拉取鏡像 `ghcr.io/meo597/xray-pro`。
- **Linux 安裝腳本**：查看 [安裝指南](https://github.com/Meo597/Xray-Pro/blob/Xray-install/release/README_zh-Hant.md)。

如有問題，請在 GitHub Issues 中提交反饋。

## Fork 和貢獻的使用說明

如果您有興趣 fork 或為 `Xray-Pro` 做出貢獻，以下說明概述了我們的自動化工作流程和分支策略，幫助您理解和適應此項目。

### 入門指南

1. **Fork 或克隆**：Fork 此倉庫或將其克隆到您的本地環境。
2. **設置 PAT**：如果進行 fork，請按下方說明配置 PAT 以啟用工作流程寫權限。
3. **創建增強分支**：按照命名規範創建分支，用於您的增強或修復。
4. **貢獻**：將分支推送到倉庫，自動化工作流程將處理 rebase 和合併。
5. **監控發佈**：檢查 `RELEASE_BRANCH` 以獲取最新的合併變更和發佈版本。

### 自動化工作流程概述

我們的 GitHub Workflows 旨在簡化追蹤上游變更和整合本地增強的過程：

- **追蹤上游穩定版本**：工作流程會自動追蹤上游倉庫（例如 `XTLS/Xray-core`）的最新穩定版本。本地鏡像儲存在定義為 `STABLE_BRANCH` 的分支中。同時，支持透過倉庫變數鎖定特定上游版本以確保穩定性。
- **Rebase 增強分支**：本地增強分支，前綴為 `ENHANCE_BRANCH_PREFIX`（例如 `pro`），會被 rebase 到 `STABLE_BRANCH` 上，確保與上游基線保持同步。這些分支遵循嚴格的命名規範，使用年月序號前綴以強制合併順序並防止潛在衝突（例如 `Xray-core/pro/250588/feat-xxxxxxxx` 或 `Xray-core/pro/250588/bpfix-xxxxxxxxxxx`）。
- **順序合併**：rebase 後，增強分支將根據年月序號前綴按順序合併到臨時的 `RELEASE_BRANCH` 中。此分支會保留，直到有新的增強分支添加或下一次上游更新發生。
- **版本發佈**：合併完成後，基於 `RELEASE_BRANCH` 的內容發佈新的 `Xray-Pro` 版本。

### 處理多個上游倉庫

此倉庫支持同時管理多個上游倉庫。每個上游倉庫（例如 `XTLS/Xray-core`）由一個專用的 GitHub Workflow YAML 文件處理，允許在此單一倉庫中進行統一管理。您可以根據需要複製和自定義工作流程文件，以針對不同的上游倉庫，確保每個上游都有自己的追蹤和合併流程。增強分支結構反映其對應的上游（例如 `Xray-core/pro/250588/feat-xxxxxxxx` 對應 `XTLS/Xray-core`）。

### 分支命名規範

要貢獻或 fork 此項目，請遵循以下增強分支的命名規範：

- **格式**：`<upstream-name>/pro/<yymmnn>/<type>-<description>`
  - `<upstream-name>`：上游倉庫的名稱（例如 `Xray-core` 對應 `XTLS/Xray-core`）。
  - `pro`：定義為 `ENHANCE_BRANCH_PREFIX` 的前綴，表示本地增強分支。
  - `<yymmnn>`：一個六位數字前綴，格式為 `YYMMNN`，其中 `YY` 是年份後兩位（如 2025 年為 `25`），`MM` 是兩位月份（如 5 月為 `05`），`NN` 是當月內的兩位序號（如第 88 個為 `88`），以確保合併順序並避免衝突。
  - `<type>`：變更類型（例如 `feat` 表示功能，`fix` 表示修復）。
  - `<description>`：變更的簡要描述。
- **示例**：
  - `Xray-core/pro/250588/feat-custom-routing`
  - `Xray-core/pro/250589/fix-memory-leak`

遵循此規範，工作流程將確保分支按正確順序合併，最大限度減少整合過程中的潛在衝突。

### 為 Fork 版本設置個人訪問令牌（PAT）

如果您 fork 此項目創建自己的版本，必須設置個人訪問令牌（PAT），以便 GitHub Actions 寫入 `workflow` 目錄，因為預設令牌權限不足。在 [https://github.com/settings/personal-access-tokens/new](https://github.com/settings/personal-access-tokens/new) 創建 PAT，設置如下：

- 範圍：僅限此倉庫。
- 權限：`Contents`（讀寫）、`Issues`（讀寫）、`Workflows`（讀寫）。
- 過期：設為“永不過期”或按安全策略設置。

將 PAT 作為 `PAT_TOKEN` 添加到倉庫的 `Settings -> Secrets and variables -> Actions -> Secrets` 中。
