# Xray-Pro

English | [简体中文](README_zh-Hans.md) | [繁体中文](README_zh-Hant.md)

An enhanced version of Xray based on the latest stable upstream release, primarily featuring enhancements rejected by upstream, while occasionally including forward-ported fixes and features as needed.

## Usage Instructions for Forking and Contributing

If you are interested in forking or contributing to `Xray-Pro`, the following instructions outline our automated workflow and branching strategy to help you understand and adapt the project.

### Getting Started

1. **Fork or Clone**: Fork this repository or clone it to your local environment.
2. **Set Up PAT**: If forking, configure a PAT as described below to enable workflow write permissions.
3. **Create Enhancement Branches**: Create branches following the naming convention for your enhancements or fixes.
4. **Contribute**: Push your branches to the repository, and the automated workflow will handle rebasing and merging.
5. **Monitor Releases**: Check the `RELEASE_BRANCH` for the latest merged changes and released versions.

### Automated Workflow Overview

Our GitHub Workflows are designed to streamline the process of tracking upstream changes and integrating local enhancements:

- **Tracking Upstream Stable Releases**: The workflows automatically track the latest stable release from the upstream repository (e.g., `XTLS/Xray-core`). A local mirror of the upstream stable release is maintained in the branch defined as `STABLE_BRANCH`.
- **Rebasing Enhancement Branches**: Local enhancement branches, prefixed with `ENHANCE_BRANCH_PREFIX` (e.g., `pro`), are rebased onto the `STABLE_BRANCH` to ensure they are up-to-date with the upstream baseline. These branches follow a strict naming convention with a numeric prefix to enforce merge order and prevent potential conflicts (e.g., `Xray-core/pro/0001/feat-xxxxxxxx` or `Xray-core/pro/0001/fix-xxxxxxxxxxx`).
- **Sequential Merging**: After rebasing, enhancement branches are merged into a temporary `RELEASE_BRANCH` in sequential order based on their numeric prefix. This branch is retained until a new enhancement branch is added or the next upstream update occurs.
- **Version Release**: Once merging is complete, a new version of `Xray-Pro` is released based on the contents of `RELEASE_BRANCH`.

### Handling Multiple Upstream Repositories

This repository supports managing multiple upstream repositories simultaneously. Each upstream repository (e.g., `XTLS/Xray-core`) is handled by a dedicated GitHub Workflow YAML file, allowing unified management within this single repository. You can duplicate and customize workflow files to target different upstream repositories as needed, ensuring each upstream has its own tracking and merging process. Enhancement branches are structured to reflect their corresponding upstream (e.g., `Xray-core/pro/0001/feat-xxxxxxxx` for `XTLS/Xray-core`).

### Branch Naming Convention

To contribute or fork this project, adhere to the following branch naming convention for enhancement branches:

- **Format**: `<upstream-name>/pro/<number>/<type>-<description>`
  - `<upstream-name>`: The name of the upstream repository (e.g., `Xray-core` for `XTLS/Xray-core`).
  - `pro`: The prefix defined as `ENHANCE_BRANCH_PREFIX`, indicating a local enhancement branch.
  - `<number>`: A zero-padded numeric prefix (e.g., `0001`) to ensure merge order and avoid conflicts.
  - `<type>`: The type of change (e.g., `feat` for feature, `fix` for fix).
  - `<description>`: A brief description of the change.
- **Examples**:
  - `Xray-core/pro/0001/feat-custom-routing`
  - `Xray-core/pro/0002/fix-memory-leak`

By following this convention, the workflow ensures that branches are merged in the correct order, minimizing potential conflicts during integration.

### Setting Up Personal Access Token (PAT) for Forked Repositories

If you fork this project, you must set up a Personal Access Token (PAT) to allow GitHub Actions to write to the `workflow` directory, as the default token lacks sufficient permissions. Create a PAT at [https://github.com/settings/personal-access-tokens/new](https://github.com/settings/personal-access-tokens/new) with the following settings:

- Scope: Specific to this repository.
- Permissions: `Contents` (Read and write), `Issues` (Read and write), `Workflows` (Read and write).
- Expiration: Set to "Never expire" or as per your security policy.

Add the PAT as `PAT_TOKEN` in your repository's `Settings -> Secrets and variables -> Actions -> Secrets`.
