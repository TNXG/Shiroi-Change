此内容由Gemini 2.5 Flash 基于 diff 原文件生成。

如果出现错误，请联系我。

---

# Shiroi - 天翔TNXGの空间站 定制版本

## ⚠️ 重要声明：关于本项目与上游 Shiro / Shiroi

本项目是基于 Innei 的闭源仓库 `innei-dev/Shiroi` 进行的二次开发和**部分开源**。

**上游原版 Shiro (开源版) 和 Shiroi (闭源版) 均为 [Innei](https://github.com/Innei) 所有。**

* **Shiro:** 这是 Innei 的开源博客框架，可以在 [Innei/Shiro](https://github.com/Innei/Shiro) 找到。它是一个功能强大且美观的博客解决方案，足以满足大多数个人博客的需求。
* **Shiroi:** 这是 Innei 的闭源版本，提供了一些额外的定制功能，主要针对特定的赞助者。

**本项目的性质：**

本仓库包含了我在使用 `innei-dev/Shiroi` 的过程中进行的一些**个性化修改和功能增强**。这些改动基于我对自身需求和用户体验的理解。

**请注意：**

* **我无权分发上游的闭源代码。** 因此，本仓库只包含了我对上游仓库所进行的 `diff` （即代码差异）。这意味着，你无法直接从本仓库获取一个完整的、可运行的 Shiroi 实例。
* **如果你想使用 Shiroi 的完整功能，我强烈建议你通过 [Innei 的赞助渠道](https://github.com/sponsors/Innei) 来获取。** 这样做不仅能获得官方支持，也能尊重原作者的知识产权和付出。
* 本仓库旨在展示我对 Shiroi 的定制化工作，并为可能对某些特定功能修改感兴趣的开发者提供参考。

---

## 本项目的改动列表

以下是本仓库中包含的、针对上游 `innei-dev/Shiroi` 所做的主要改动概述：

### 1. GitHub Actions CI/CD 配置 (`.github/workflows/build.yml`) 🌟

*   **新增了完整的 CI Build 工作流：** 用于自动化构建、测试和打包项目。
*   **自动生成 `release.zip` Artifact：** 方便下载和部署最新的构建产物。
*   **集成了 Secret 环境变量：** 确保敏感配置（如 API Key）在构建过程中安全注入。
*   **优化了 `ci-release-build.sh` 脚本：** 精简了构建步骤，更适合自动化流程。

### 2. 自动化合并上游改动脚本 (`merge.py`) 自动化上游更新 🛠️

*   **新增 `merge.py` 脚本：** 一个用 Python 编写的自动化工具，用于从指定的上游 Git 仓库（例如 `innei-dev/Shiroi`）获取最新代码并合并到当前仓库。
*   **智能远程管理：** 脚本会根据上游仓库名动态创建或更新 Git 远程，并尝试合并 `main` 分支。
*   **平台兼容性：** 尝试根据操作系统选择 HTTPS 或 SSH URL。
*   **错误处理与日志：** 提供详细的命令输出和错误信息，方便调试。

### 3. Service Worker (PWA) 支持 (`public/serviceworker.js`, `public/sw.js`) 🚀

*   **新增 Service Worker 文件：** 实现渐进式 Web 应用 (PWA) 的离线缓存和网络请求拦截。
*   **资源缓存策略：** 针对常用资源进行缓存，包括图片资源（带过期时间），提升加载速度和离线可用性。
*   **图片代理绕过：** 自动识别并绕过 Shiro 内置的图片代理，直接访问原始图片链接，提高图片加载效率。
*   **多镜像源并发请求：** 针对特定 CDN (如 `hdslb.com`, `mx.tnxg.top` 等) 启用并发请求，自动选择最快可用的镜像源，优化用户体验。
*   **SW-Req 接口：** 提供自定义的 Service Worker 内部 API，用于获取 IP 信息和版本号。

### 4. 捐赠页面及赞助者鸣谢功能 (`src/app/(app)/donate/*`, `src/app/api/sponsors/route.ts`) ❤️

*   **新增 `src/app/(app)/donate` 目录：** 包含了全新的捐赠页面，提供微信支付、支付宝和爱发电等多种支持方式。
*   **赞助者鸣谢列表：** 从自定义 API (例如 `mx.tnxg.top/api/v2/snippets/data/sponsors`) 获取捐赠者数据并展示，感谢他们的支持。
*   **动态加载与分页：** 支持分页加载赞助者数据，优化页面性能。
*   **用户界面优化：** 包含动效和友好的提示信息。

### 5. UI/UX 及细节优化 🎨

*   **字体定制：** 引入并应用了 `MiSans_VF` 等自定义字体，提升视觉效果。
*   **鼠标光标定制：** 提供了独特的鼠标光标样式。
*   **评论区功能增强 (`src/components/modules/comment/Comment.tsx`)：**
    *   评论时间显示精确到秒。
    *   移除了 `location` 和 `key` 的公开展示，以保护隐私。
    *   优化了头像和第三方登录图标的尺寸及位置。
    *   调整了评论气泡样式和间距。
*   **文章内容展示优化：**
    *   **`ActivityCard.tsx`：** 在活动卡片中增加了时间戳，提供更精确的活动发生时间。
    *   **`ActivityPostList.tsx`：** 将“最近更新的文稿”改为“最近更新的文章”，用词更规范。
    *   **`PostCopyright.tsx`：** 优化了版权声明的排版和内容，添加了 CC BY-NC-SA 4.0 许可协议的图片链接，并调整了字体大小和间距。
    *   **`PostOutdate.tsx`：** 修改了文章过时提醒的判断周期（从 60 天增加到 120 天），并增加了更详细的提醒文案。
    *   **`SummarySwitcher.tsx`、`XLogSummary.tsx`、`XLogSummaryAsync.tsx`：** 将摘要内容的字体大小从 `text-sm` 调整为 `text-lg`，提升可读性。
    *   **`NoteFontFab.tsx`：** 调整了手记默认字体为 `youzai`，并提供了 `MiSans_VF` 字体选项。
*   **友链申请页面优化 (`src/app/(app)/friends/page.tsx`)：** 更新了友链申请的各项注意事项，使其更清晰、规范。
*   **页脚信息增强 (`src/components/layout/footer/FooterInfo.tsx`, `src/components/layout/footer/Footer.tsx`)：**
    *   更新了 `Powered by 白い` 的浮动提示，明确指出了闭源版本和开源版本的区别，并引导用户通过赞助获取闭源版本。
    *   更新了页脚的座右铭 "Live in the present more than the future or the past."。
    *   优化了页脚的整体布局，将主题切换器移至右下角。
*   **Gateway 信息优化 (`src/components/layout/footer/GatewayInfo.tsx`)：**
    *   调整了人数统计的文案，使其更生动有趣。
    *   优化了正在阅览内容列表的文案。
*   **导航菜单调整 (`src/components/layout/header/config.ts`)：**
    *   调整了菜单项的顺序和名称，将“文稿”改为“文章”，并新增了“捐赠”入口。
*   **文章排版及打印样式调整 (`src/styles/tailwindcss.css`)：**
    *   调整了打印时字体的默认大小。
    *   修复了 Prose 样式下图片上下的空白问题。
*   **Git Submodule 更新：** 更新了 `reporter-assets` 子模块的 URL。
*   **VSCode 配置更新：** 添加了 `Codegeex.RepoIndex` 配置。
*   **依赖项更新 (`pnpm-lock.yaml`)：** 升级了部分依赖包版本，确保项目依赖的健康性。

### 6. 代码高亮优化 (`src/components/ui/code-highlighter/*`) ✨

*   **更新 Shiki 主题：** 将代码高亮主题从 `github-dark`/`github-light` 切换到 `light-plus`，提供更一致的视觉体验。
*   **语言图标增强：**
    *   引入 `@iconify-json/catppuccin`，为代码语言提供了更丰富的图标。
    *   实现了基于语言的动态图标和颜色获取逻辑，提升了代码块的视觉辨识度。
    *   调整了代码块中语言图标的尺寸。
*   **字体大小调整：** 优化了代码块内容的字体大小，提升代码的可读性。

---

## 如何使用这些改动？

由于本仓库不包含完整的 Shiroi 源代码，因此你无法直接克隆并运行。

如果你是 Shiroi 的赞助者并拥有其完整代码，你可以将本仓库的 `diff` 应用到你的本地 Shiroi 仓库中。这需要一些 Git 操作知识：

1.  **下载 Patch 文件：**
    你可以通过 GitHub 的 UI 或使用 `git diff` 命令，生成一个包含所有这些改动的 `.patch` 文件。
    **示例：** 如果你是在 GitHub 上看到这个 README，你可以通过在提交历史中找到一个包含所有这些改动的提交，然后点击 `...` -> `View file` -> `Raw`，将其内容保存为 `my_changes.patch` 文件。

2.  **应用 Patch：**
    在你的本地 `innei-dev/Shiroi` 仓库的根目录下，运行：
    ```bash
    git apply /path/to/my_changes.patch
    ```
    **注意：** 应用补丁可能会遇到冲突。你需要手动解决这些冲突。

**温馨提示：** 除非你对 Git 和项目结构非常熟悉，否则直接应用补丁可能会比较复杂。

---

## 贡献与支持

*   **对本项目的贡献：** 如果你发现我的这些改动有任何 Bug 或优化建议，欢迎通过 GitHub Issues 提出。由于这不是一个独立的应用程序，PR 可能会比较复杂，但任何反馈都非常宝贵。
*   **支持上游项目：** 我强烈建议您通过 [Innei 的 GitHub Sponsors](https://github.com/sponsors/Innei) 支持原作者。他们的工作是 Shiro 和 Shiroi 的基石。
*   **支持本项目：** 如果你喜欢我所做的这些个性化定制和优化，或者觉得它们对你有所帮助，你也可以通过 [我的捐赠页面](https://tnxgmoe.com/donate) 来支持我。你的支持是我持续维护和分享的动力。# Shiroi-Change
