# 00 项目总览（Project Overview）

> 本文档属于《嵌入式软件工程师技术面试 · 项目讲解材料》，全部结论以本地仓库真实代码为准。
> 仓库：`meminehobe24435-cmyk.github.io`，克隆位置：`interview_audit/portfolio-site`。

## 一句话定义

这是一个**纯静态、无构建步骤、无 JavaScript** 的 GitHub Pages 单页作品集站点，用 HTML5 语义化标签 + 一份外部 CSS 呈现作者（2027 届嵌入式软件工程师校招候选人尤译庆）的 4 个嵌入式项目：通过 2 个演示视频（MP4）与 3 份项目报告（PDF）向 HR/面试官传递「硬件控制、视觉处理、目标追踪、低延迟无线外设」方向的能力。

- 证据：全仓库仅 9 个受跟踪文件（不含 `.git`），其中代码文件只有 `index.html`（282 行 / 11,972 B）与 `styles.css`（526 行 / 9,671 B），其余为媒体资源。
- 证据：`index.html` 全文无 `<script>` 标签、无内嵌 `<style>`，样式全部来自第 11 行的 `<link rel="stylesheet" href="styles.css" />`。

## 背景（为什么是这样一个项目）

1. **作者身份**：2027 届校招候选人，实习于深圳维特智能（嵌入式开发）。站点 `index.html:62-71` 的求职方向自述为「嵌入式开发实习生」，关注 `MCU/FPGA、传感器数据处理、运动控制、视觉算法落地与软硬件联调`。
2. **求职诉求**：`index.html:35-37` 明确写到「这里集中展示硬件控制、视觉处理、目标追踪与低延迟无线外设相关项目，HR 可直接查看演示视频与项目报告」——即站点定位是**给 HR 快速看的可视化作品集**，而非技术博客或文档库。
3. **仓库性质**：这是 GitHub 的用户站点仓库（`<username>.github.io`），GitHub Pages 会直接把它当网站根目录发布；作者用 `.nojekyll` 关闭 Jekyll 处理，保证二进制媒体（MP4/PDF）按原样输出。
   - 证据：`remote.origin.url = https://github.com/meminehobe24435-cmyk/meminehobe24435-cmyk.github.io.git`；根目录存在 `.nojekyll`（2 字节，内容为 CRLF）。
4. **站点演进**：`git log` 显示 3 个 commit，全部在同一天（2026-06-12）约 47 分钟内完成（20:26:52 → 21:13:18 +0800），是一个「单次会话快速搭建」的站点，而非长期迭代产物。

## 为什么做这个作品集（面试要能讲清的一句话）

> 「嵌入式项目的实物很难在简历上被看到，所以我把 4 个项目的演示视频和完整报告做成一个零依赖静态站，让面试官点开链接就能『看』到我的项目，而不是只能读文字描述。」

这一点与站点文案一致（`index.html:36-37`：`HR 可直接查看演示视频与项目报告`）。

## 需求拆解

| 需求 | 站点如何满足 | 证据位置 |
| --- | --- | --- |
| 一眼看出求职方向 | Hero 副标题 + intro 区块写明「嵌入式开发实习生」 | `index.html:33, 62-71` |
| 快速量化作品规模 | Hero 的 stats 面板：4 项目 / 2 视频 / 3 报告 | `index.html:45-58` |
| 展示技能栈 | 6 张技能卡片（程序开发/协议调试/电路设计/硬件调试/算法应用/FPGA） | `index.html:73-123` |
| 展示项目 | 4 个 project 卡片，视频/PDF 可点开 | `index.html:125-201` |
| 展示获奖背书 | 8 张奖项卡片 + 技能证书条 | `index.html:203-255` |
| 提供联系方式 | 邮箱/电话/微信三栏 | `index.html:257-279` |
| 移动端可看 | 两个断点 `@media (max-width:760px / 480px)` | `styles.css:463-526` |

## 难点（真实的技术难点，按站点项目定位）

1. **大体积媒体与首屏体验的矛盾**：两个视频合计约 38.5 MB，PDF 报告合计约 13.4 MB。站点用 `preload="metadata"` 只预取元数据（`index.html:133,153`），把「下载成本」推迟到用户真正点播放，避免首屏拖垮。
2. **无构建工具下的工程组织**：不引入框架、不打包，靠语义化标签 + CSS 变量 + Grid 直接交付，且要保证 320px 最小宽度可读（`styles.css:26`）。
3. **二进制资源在 GitHub Pages 上的可用性**：靠 `.nojekyll` 关闭 Jekyll（否则 `assets/` 下的文件可能被 Jekyll 忽略/改写路径），并保证资源路径大小写与文件名一致（`assets/videos/…`、`assets/reports/…`）。
4. **可访问性与 SEO 的基础功课**：`lang="zh-CN"`、`aria-label`、`viewport`、`meta description` 均已就位（见 13_CODE_REVIEW），但 favicon/OG/结构化数据等更高级项缺失。

## 「我的工作内容」三档（面试自我介绍分档口径）

> 注意：站点是「作品集站点」本身，不是固件工程。以下三档是**围绕这个站点项目**的工作内容划分，供面试时按岗位 JD 深浅切换。

- **第一档（1 分钟）**：独立设计并实现了这个 GitHub Pages 单页作品集——HTML5 语义化结构 + 一份响应式 CSS，整理并挂载了 4 个项目的演示视频与 PDF 报告，通过 `.nojekyll` 让二进制媒体在 GitHub Pages 正常发布，最终在 `https://meminehobe24435-cmyk.github.io/` 以 HTTPS 上线。
- **第二档（3 分钟）**：补充「怎么做」——用 CSS 自定义属性做暗色主题、`clamp()` 做流式字号、Grid 做响应式布局，在两个断点下把多栏卡片塌缩为单栏；视频用 `preload="metadata"` 控制首屏带宽；外部资源链接统一 `target="_blank" rel="noreferrer"` 防 reverse tabnabbing；并为导航/视频/联系方式补充了 `aria-label` 无障碍标注。
- **第三档（8 分钟 + 可追问）**：补充「权衡与改进」——指出当前站点的局限：无 JS 交互、视频无 poster 封面、HEVC 视频存在浏览器兼容风险、4 个展示项目未链接到任何 GitHub 代码仓库（而作者实际有 5 个公开代码仓库未被站点引用）。能据此主动讲优化路线（CDN、视频转码、懒加载、骨架屏、把代码仓库链接补进项目卡片）。

## 最终成果（可量化）

- 一个 9 文件、约 50 MB 的静态站，全部由 `git` 跟踪、单分支 `main`。
- 3 次 commit 交付（`e575bc6 → 81f14a6 → 3e82fa0`），HEAD = `3e82fa084854840fb737284451bf0f4aa5f1080f`。
- 线上可访问：`https://meminehobe24435-cmyk.github.io/`（实测 HTTP 200，标题与仓库 `index.html` 一致，页面含 `<video>`）。
- 内容侧：4 个项目、2 段视频（约 38.5 MB）、3 份 PDF（约 13.4 MB）、6 项技能、8 项奖项。

## 关键事实速查（贯穿全系列文档的「证据基线」）

| 项 | 值 | 证据 |
| --- | --- | --- |
| HEAD | `3e82fa084854840fb737284451bf0f4aa5f1080f` | `git rev-parse HEAD` |
| commit 数 | 3 | `git log --all` |
| 首次/末次 commit | 2026-06-12 20:26:52 / 21:13:18 (+0800) | `git log --date=iso` |
| 作者 | 尤译庆（唯一提交者） | `git log --format=%an` |
| 分支 | `main`（唯一） | `git branch -a` |
| 部署 | GitHub Pages，直接发布 `main` 分支，无 Actions | 无 `.github/`、无 `CNAME`，`.nojekyll` 存在 |
| 域名 | `https://meminehobe24435-cmyk.github.io/`（HTTPS 200） | `README.md:9` + 实测 HTTP 200 |
| 技术栈 | HTML5 + CSS（无 JS、无框架、无构建工具） | `index.html` / `styles.css` |
| 媒体 | MP4（H.264 + H.265）+ PDF 1.7 | 二进制解析（见 01） |
