# 01 技术栈（Tech Stack）

> 面试口径：**先把「实际用的」讲清楚，再解释「为什么不用更重的方案」。** 本仓库技术栈极简，反而是可深挖的面试点——因为它证明作者懂「够用即可」的工程判断。

## 总览表

| 层 | 实际使用 | 证据 | 未使用（说明） |
| --- | --- | --- | --- |
| 结构 | HTML5（语义化标签） | `index.html` 全文 | 无框架（React/Vue 等） |
| 表现 | CSS3（自定义属性、Grid、`clamp()`、`@media`、`backdrop-filter`） | `styles.css` 全文 | 无预处理器（Sass/Less） |
| 行为 | **无 JavaScript** | `index.html` 无 `<script>` | 无 jQuery/框架 |
| 部署 | GitHub Pages（直接发布 `main`，无 CI/Actions） | 无 `.github/`；`remote` 为 `<username>.github.io` | 无自定义域名（无 `CNAME`） |
| 静态资源托管 | GitHub Pages 内置 CDN + HTTPS | 实测 `https://…/` 返回 200 | 无第三方 CDN |
| 媒体 | MP4（H.264/AVC 与 H.265/HEVC）+ PDF 1.7 | 二进制盒解析 | 无 WebM/AV1，无图片 |

---

## 一、HTML5（结构层）

**是什么**：W3C 制定的语义化标记语言；本站用其语义标签组织单页内容。

**为什么用**：
- 单页作品集不需要状态管理/路由，语义化 HTML 即可表达「导航→首屏→技能→项目→奖项→联系」的信息架构。
- 对 SEO 与可访问性友好，且零依赖、零构建。

**怎么用（代码在哪里）**：
- `<!doctype html>`（`index.html:1`）、`<html lang="zh-CN">`（`:2`）。
- 语义分区：`<header>`/`<nav>`（`:14-27`）、`<main>`（`:29`）、`<section>`×6（hero/intro/skills/projects/awards/contact）、`<article>` 用于技能卡与项目卡。
- 头部 meta：`charset=utf-8`（`:4`）、`viewport`（`:5`）、`<title>`（`:6`）、`meta description`（`:7-10`）。
- 视频标签带回退文案：`<video controls preload="metadata">` + `<source type="video/mp4">` + 「当前浏览器不支持视频播放…」`（:133-135, 153-155）。

**面试官怎么追问**：
- Q：为什么 `<video>` 里要写 `<source>` 之外的纯文本？→ A：作为 `video` 标签不支持时的 fallback，符合渐进增强（progressive enhancement）。
- Q：`lang="zh-CN"` 有什么用？→ A：告知屏幕阅读器/翻译器/搜索引擎文档语言，属于最基础的无障碍与 SEO 约定。

## 二、CSS3（表现层）

**是什么**：本站唯一的行为呈现载体，文件 `styles.css`（526 行，未压缩约 9.7 KB）。

**为什么用**：无需预处理器，CSS 变量 + Grid 已能覆盖暗色主题、响应式与卡片布局，避免为 9 KB 样式引入 Node 构建链。

**怎么用（关键点 + 行号）**：
- 主题变量（`:root` 定义 `--bg/--panel/--text/--cyan/--green/…`）：`styles.css:1-14`。
- 全局 `box-sizing: border-box`：`:16-18`；`scroll-behavior: smooth`：`:20-22`。
- 暗色网格背景（`linear-gradient` 叠加成网格）：`:24-32`。
- 流式字号 `clamp()`：`h1` `clamp(4rem,18vw,11rem)`（`:152-158`），`h2/h3` 同理（`:160-172`）。
- 布局：`display:grid` 用于 `.stats`/`.intro`/`.skill-grid`/`.project`/`.award-board`/`.contact-list`（`:216-222, 246-250, 262-266, 301-309, 383-387, 430-435`）。
- 粘性导航 + 毛玻璃：`.site-header` 的 `position:sticky; top:0` 与 `backdrop-filter: blur(18px)`（`:44-51`）。
- 响应式：`@media (max-width:760px)` 把多栏 Grid 塌缩为 `1fr`（`:463-512`）；`@media (max-width:480px)` 隐藏品牌文字、按钮全宽（`:514-526`）。
- 最小宽度保护：`body { min-width: 320px; }`（`:26`）。

**面试官怎么追问**：
- Q：`clamp()` 与 `@media` 的区别？→ A：`clamp()` 是「流式/连续」响应（字号随视口连续变化），`@media` 是「断点/离散」响应（布局整体切换）；本站两者并用：字号用 clamp、栅格用断点。
- Q：`backdrop-filter: blur(18px)` 兼容性？→ A：主流现代浏览器支持；在不支持的浏览器里只是失去毛玻璃，导航仍可用（渐进增强）。

## 三、JavaScript —— 明确「无」

**是什么**：本站没有一行 JS。所有交互依赖 HTML 原生能力：锚点导航（`<a href="#projects">`）、原生 `<video controls>`、`<a href="mailto:/tel:">`、`<a target="_blank">`。

**为什么（可讲的工程判断）**：
- 站点的交互需求（跳转、播放、发邮件）都能被浏览器原生元素覆盖，引入 JS 反而增加体积与复杂度。
- 面试加分表述：「这是一个**无 JS 也能完整可用**的站点，天然避免了首屏 JS 阻塞、SPA 白屏与 SEO 爬取问题。」

**面试官怎么追问**：
- Q：如果我要加「视频懒加载/骨架屏/移动端汉堡菜单」，你要不要引入 JS？→ A：承认这些需求下原生方案不够用（`loading="lazy"` 对 `<video>` 无直接对应，汉堡菜单需要状态切换），可以**局部、渐进地**引入原生 ES Module 或极轻量脚本，而不是为此引入框架。

## 四、GitHub Pages 部署

**是什么**：GitHub 提供的免费静态站托管，仓库即站点根目录。

**为什么用**：`<username>.github.io` 仓库是「免费、免运维、自带 HTTPS + CDN」的个人站点方案，适合作品集。

**怎么用（代码/事实在哪里）**：
- 远程仓库即用户站点仓库：`remote.origin.url = https://github.com/meminehobe24435-cmyk/meminehobe24435-cmyk.github.io.git`。
- **无 `.github/workflows`** → 说明未使用 GitHub Actions 自定义构建/部署，而是采用 GitHub Pages 的「Deploy from a branch」默认方式直接发布 `main` 分支根目录。证据：`Test-Path .github` = False。
- `.nojekyll` 存在（2 字节）→ 关闭 Jekyll 处理，让 `assets/` 下二进制与路径原样输出（否则 Jekyll 会忽略下划线目录并可能改写路径）。
- 无 `CNAME` → 使用默认域名 `https://meminehobe24435-cmyk.github.io/`（`README.md:9`）。
- HTTPS：GitHub Pages 默认提供并（在项目设置中）强制 HTTPS；实测 `GET https://meminehobe24435-cmyk.github.io/` 返回 200，最终 URL 为 `https://`。

**面试官怎么追问**：
- Q：`.nojekyll` 不写会怎样？→ A：GitHub Pages 默认用 Jekyll 构建；若站点无 `_config.yml` 通常也能构建，但 `assets/` 等非 Jekyll 结构可能在处理时被忽略或报错；放 `.nojekyll` 是显式声明「不要 Jekyll，原样发布」的标准做法。
- Q：为什么不用 GitHub Actions 部署？→ A：本仓库无构建步骤（无压缩/打包产物），直接发布源文件即可；Actions 的价值在于「先构建再发布」，此处不需要，属于合理取舍。

## 五、媒体格式（视频编码 / PDF）

> 编码与时长/分辨率来自对 MP4 盒（box）的二进制解析（无 ffprobe 环境），方法可靠但精度以「约」标注；如需精确值，用 ffprobe 复核。

| 文件 | 大小 | 封装/编码 | 时长(约) | 分辨率(约) | 估算码率(约) |
| --- | --- | --- | --- | --- | --- |
| `assets/videos/binocular-vision.mp4` | 28,294,909 B（27.0 MiB） | MP4/`isom` + **H.264 (`avc1`)** | 205.3 s（3 分 25 秒） | 约 960×544 | ≈1.10 Mbps |
| `assets/videos/ch585-mouse.mp4` | 12,059,523 B（11.5 MiB） | MP4/`mp42` + **H.265/HEVC (`hvc1`)** | 155.6 s（2 分 36 秒） | 960×544 | ≈0.62 Mbps |
| `assets/reports/binocular-vision-report.pdf` | 10,605,375 B（10.1 MiB） | PDF 1.7，66 页 | — | — | — |
| `assets/reports/moving-target-tracking-report.pdf` | 1,716,834 B（1.64 MiB） | PDF 1.7，9 页 | — | — | — |
| `assets/reports/auto-aim-device-report.pdf` | 1,286,951 B（1.23 MiB） | PDF 1.7，14 页 | — | — | — |

**编码细节与面试价值**：
- 两个视频**用了不同编码**：双目视觉用 H.264（`avc1`），CH585 鼠标用 H.265（`hvc1`）。在几乎相同的分辨率下（约 960×544），H.265 用约 0.62 Mbps 达到与 H.264 约 1.10 Mbps 相近的画质——**约 40% 的码率节省**，这是可讲的「视频压缩」知识点。
- 但代价是**兼容性**：H.264 全浏览器可用；H.265/HEVC 在 Safari 原生支持良好，Chrome 需硬件解码器支持，Firefox 一般不原生支持 `<video>` 播放 HEVC（详见 06/08）。
- `preload="metadata"`（`index.html:133,153`）只预取 moov 元数据，不全量下载视频——这是首屏性能的关键手段。

**面试官怎么追问**：
- Q：为什么一个用 H.264 一个用 H.265？→ A：若来自不同采集/剪辑工具链（手机/录屏/OBS 默认导出不同），或为省体积对长视频用了 H.265；**这一点我无法从仓库证明导出意图，需如实说「以导出源为准」**（标注【待本人确认】）。
- Q：10 MB 的 66 页 PDF 对移动端太重吗？→ A：PDF 不会在首屏加载（点击才打开），10 MB 主要影响「点击后」的下载体验；后续可做「按需导出压缩 PDF / 图片化预览 + 下载原件」优化。

## 六、「是什么 → 为什么 → 怎么用 → 在哪 → 追问」速查

| 技术 | 是什么 | 为什么用 | 代码在哪 | 典型追问 |
| --- | --- | --- | --- | --- |
| HTML5 语义化 | 语义标签结构 | SEO/无障碍/零依赖 | `index.html` | `section` vs `div` 区别 |
| CSS 变量 + Grid | 主题与布局 | 免预处理、可维护 | `styles.css:1-14, 301` | 为什么不用 Tailwind |
| `clamp()`/`@media` | 流式与断点响应 | 一屏多用 | `styles.css:152, 463` | 两者区别 |
| 无 JS | 零脚本 | 原生能力已够、避免首屏阻塞 | 全文无 `<script>` | 什么需求会让你引入 JS |
| GitHub Pages | 免运维托管 | 免费 + HTTPS + CDN | `remote`/`.nojekyll` | `.nojekyll` 作用、为何无 Actions |
| H.264/H.265 | 视频编码 | 画质/体积权衡 | `assets/videos/*.mp4` | 两编码的兼容与码率差异 |
