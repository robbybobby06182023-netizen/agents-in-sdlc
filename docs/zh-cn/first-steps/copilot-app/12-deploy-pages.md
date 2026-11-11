---
title: "第 12 课 - 使用 GitHub Pages 部署站点"
description: "使用 GitHub Copilot app 配置 Pages 工作流，审查其拉取请求，并验证在线 Space Quiz。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

给 Space Quiz 一个公开的家：使用 GitHub Pages 部署它。Copilot 可以准备工作流并指导部署，但你仍然负责审查变更并确认站点已上线。

在本课中，你将：

- 要求 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署并验证已发布站点。

## 场景

Space Quiz 已经在 GitHub 仓库中，但人们仍然需要一种简单的方式来试用它。使用 GitHub Pages 发布这个单文件网站，可以让它从公开 URL 访问，而无需额外服务器或运行时依赖。

## 要求 Copilot 部署站点

在 `space-quiz` 的 Copilot app 会话中，发送以下提示：

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段示意图：已合并的 space-quiz 拉取请求；成功运行的 GitHub Actions Deploy website 工作流，其中包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 这个提示要求智能体创建并合并拉取请求，并更改仓库的 Pages 设置。在继续之前，请审查差异、部署权限，以及任何需要你批准的 GitHub 提示。

## 审查工作流并验证部署

1. 确认仅在仓库中还没有 README 时才新增它。
2. 审查拉取请求中的更改，包括工作流中的构建和部署步骤。
3. 仅在所需的拉取请求检查全部通过后再合并。
4. 跟踪 GitHub Actions 的部署运行，直到它成功完成。
5. 打开报告的 URL，并确认 Space Quiz 能正常加载。

如果智能体无法启用 Pages 或合并拉取请求，请遵循它给出的具体说明。不要在部署成功且在线站点能够加载之前就将任务视为完成。

## 总结与后续步骤

你的 Space Quiz 现在已经作为在线站点可用，并且你已验证了其部署，而不是仅依赖智能体的报告。继续阅读 [第 13 课：回顾与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
