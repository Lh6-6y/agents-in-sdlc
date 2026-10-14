---
title: "第 12 课 - 使用 GitHub Pages 部署站点"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查拉取请求并验证在线的 Space Quiz。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

为 Space Quiz 提供公开的托管地址：使用 GitHub Pages 部署它。Copilot 可以准备工作流并指导部署，但你仍然需要负责审查变更并确认站点已上线。

在本课中，你将：

- 让 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署并验证已发布站点。

## 场景

Space Quiz 已经位于 GitHub 仓库中，但人们仍然需要一个简单的方法来使用它。通过 GitHub Pages 发布单文件站点，可以在不增加服务器或运行时依赖的情况下，通过公开 URL 提供访问。

## 让 Copilot 部署站点

在 `space-quiz` 的 Copilot app 会话中，发送以下提示：

```plaintext
为此仓库启用 GitHub Pages。创建 GitHub Actions 工作流，在需要时构建网站并将其部署到 GitHub Pages。如果仓库中还没有 README，请添加一个文件，介绍此应用以及如何在本地运行。根据这些更改创建拉取请求，并在必要的检查通过后合并。持续监控部署，直到成功，然后报告网站上线后的最终 URL。如果你没有启用 Pages 或合并的权限，请准确告知我需要批准或执行哪些操作；在网站上线前不要报告成功。
```

![三阶段示意图：已合并的 space-quiz 拉取请求；成功运行的 GitHub Actions Deploy website，其中包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 这个提示要求智能体创建并合并拉取请求，并修改仓库的 Pages 设置。在继续前，请审查差异、部署权限和任何 GitHub 审批提示。

## 审查工作流并验证部署

1. 确认只有在仓库中没有 README 时才新增它。
2. 审查拉取请求中的改动，包括工作流的构建和部署步骤。
3. 只有在所需的拉取请求检查通过后才合并。
4. 跟进 GitHub Actions 的部署运行，直到成功结束。
5. 打开给出的 URL，并确认 Space Quiz 能正常加载。

如果智能体无法启用 Pages 或合并拉取请求，请按照它提供的具体说明执行。直到部署成功并且线上站点可加载前，不要将任务视为完成。

## 总结与下一步

你的 Space Quiz 现在已作为在线站点可用，并且你已验证了部署，而不是仅依赖智能体的汇报。继续阅读 [第 13 课：回顾与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
