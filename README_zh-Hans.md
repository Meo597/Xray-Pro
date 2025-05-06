# Xray-Pro

[English](README.md) | 简体中文 | [繁体中文](README_zh-Hant.md)

基于上游最新稳定版本的 Xray 增强版本，主要包含被上游拒绝的功能增强，同时偶尔也包括我们认为需要向后移植的修复和功能。

## Fork 和贡献的使用说明

如果您有兴趣 fork 或为 `Xray-Pro` 做出贡献，以下说明概述了我们的自动化工作流程和分支策略，帮助您理解和适应此项目。

### 入门指南

1. **Fork 或克隆**：Fork 此仓库或将其克隆到您的本地环境。
2. **设置 PAT**：如果进行 fork，请按下方说明配置 PAT 以启用工作流程写权限。
3. **创建增强分支**：按照命名规范创建分支，用于您的增强或修复。
4. **贡献**：将分支推送到仓库，自动化工作流程将处理 rebase 和合并。
5. **监控发布**：检查 `RELEASE_BRANCH` 以获取最新的合并变更和发布版本。

### 自动化工作流程概述

我们的 GitHub Workflows 旨在简化追踪上游变更和整合本地增强的过程：

- **追踪上游稳定版本**：工作流程会自动追踪上游仓库（例如 `XTLS/Xray-core`）的最新稳定版本。本地镜像存储在定义为 `STABLE_BRANCH` 的分支中。
- **Rebase 增强分支**：本地增强分支，前缀为 `ENHANCE_BRANCH_PREFIX`（例如 `pro`），会被 rebase 到 `STABLE_BRANCH` 上，确保与上游基线保持同步。这些分支遵循严格的命名规范，使用年月序号前缀以强制合并顺序并防止潜在冲突（例如 `Xray-core/pro/250588/feat-xxxxxxxx` 或 `Xray-core/pro/250588/bpfix-xxxxxxxxxxx`）。
- **顺序合并**：rebase 后，增强分支将根据年月序号前缀按顺序合并到临时的 `RELEASE_BRANCH` 中。此分支会保留，直到有新的增强分支添加或下一次上游更新发生。
- **版本发布**：合并完成后，基于 `RELEASE_BRANCH` 的内容发布新的 `Xray-Pro` 版本。

### 处理多个上游仓库

此仓库支持同时管理多个上游仓库。每个上游仓库（例如 `XTLS/Xray-core`）由一个专用的 GitHub Workflow YAML 文件处理，允许在此单一仓库中进行统一管理。您可以根据需要复制和自定义工作流程文件，以针对不同的上游仓库，确保每个上游都有自己的追踪和合并流程。增强分支结构反映其对应的上游（例如 `Xray-core/pro/250588/feat-xxxxxxxx` 对应 `XTLS/Xray-core`）。

### 分支命名规范

要贡献或 fork 此项目，请遵循以下增强分支的命名规范：

- **格式**：`<upstream-name>/pro/<yymmnn>/<type>-<description>`
  - `<upstream-name>`：上游仓库的名称（例如 `Xray-core` 对应 `XTLS/Xray-core`）。
  - `pro`：定义为 `ENHANCE_BRANCH_PREFIX` 的前缀，表示本地增强分支。
  - `<yymmnn>`：一个六位数字前缀，格式为 `YYMMNN`，其中 `YY` 是年份后两位（如 2025 年为 `25`），`MM` 是两位月份（如 5 月为 `05`），`NN` 是当月内的两位序号（如第 88 个为 `88`），以确保合并顺序并避免冲突。
  - `<type>`：变更类型（例如 `feat` 表示功能，`fix` 表示修复）。
  - `<description>`：变更的简要描述。
- **示例**：
  - `Xray-core/pro/250588/feat-custom-routing`
  - `Xray-core/pro/250589/fix-memory-leak`

遵循此规范，工作流程将确保分支按正确顺序合并，最大限度减少整合过程中的潜在冲突。

### 为 Fork 版本设置个人访问令牌（PAT）

如果您 fork 此项目创建自己的版本，必须设置个人访问令牌（PAT），以便 GitHub Actions 写入 `workflow` 目录，因为默认令牌权限不足。在 [https://github.com/settings/personal-access-tokens/new](https://github.com/settings/personal-access-tokens/new) 创建 PAT，设置如下：

- 范围：仅限此仓库。
- 权限：`Contents`（读写）、`Issues`（读写）、`Workflows`（读写）。
- 过期：设为“永不过期”或按安全策略设置。

将 PAT 作为 `PAT_TOKEN` 添加到仓库的 `Settings -> Secrets and variables -> Actions -> Secrets` 中。
