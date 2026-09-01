# 09 优化（Optimization）

> 两栏对照：**已实现（以代码为准）** vs **后续可做（未实现）**。面试讲优化时，先说「已经做对的」，再说「如果继续会怎么做」，体现有判断、不空谈。

## 一、已实现的优化（代码证据）

| 优化项 | 实现 | 证据 | 收益 |
| --- | --- | --- | --- |
| 视频元数据预取 | `preload="metadata"`，首屏不全量下载视频 | `index.html:133,153` | 首屏网络成本大降 |
| 无 JS、无框架 | 零脚本、零依赖、无构建步骤 | 全文无 `<script>` | 无首屏 JS 阻塞、无 SPA 白屏 |
| 样式外置 + 极小体积 | 单份 `styles.css` 9.7 KB | 文件体积 | 一次加载、可缓存 |
| 系统字体栈 | 不加载 Web Font | `styles.css:33-35` | 无字体请求、无 FOUT |
| 无第三方请求 | 无统计/广告/外链资源 | 全文 | 隐私友好、无外链拖慢 |
| 流式字号 + 响应式断点 | `clamp()` + `@media` 双断点 | `styles.css:152-172, 463-526` | 一屏多用 |
| H.265 压缩长视频 | CH585 视频用 HEVC，≈0.62 Mbps | 二进制解析 `hvc1` | 比 H.264 省约 40% 码率 |
| 外链安全 | `target="_blank" rel="noreferrer"` | `index.html:41,145-197` | 防 reverse tabnabbing |
| 无障碍基础标注 | `lang`、`aria-label`、`aria-hidden` | `index.html:2,15,16,31,45,133,153,265` | 读屏可用 |
| Git 提交原子化 | 3 个 commit 分「骨架/联系/技能奖项」 | `git log` | 历史可读 |

## 二、后续可做的优化（未实现，按优先级）

### P0：把代码仓库链接进作品集（价值最高）
- 现状：4 个项目的 `<a>` 只指向视频/PDF，无 GitHub 链接（`index.html:125-201`）。
- 改法：每个项目卡加一个「查看代码」按钮，指向 `esp-recorder`/`uav-car-adhesion-platform` 等；并为站点上「有视频无代码」的项目标注「代码见 XXX」或「报告详见 PDF」。
- 理由：面试的核心是「可验证」，作品集连到代码，可信度翻倍（见 07）。

### P1：视频转码 + 多编码源 + poster
- 给 `ch585-mouse.mp4` 提供 H.264 备源（`<source>` 多源降级），消除 HEVC 兼容风险（`index.html:154`）。
- 两个视频补 `poster` 封面图（现在是 `poster=""`），消除黑块。
- 可选：长视频转更小分辨率/更激进压缩，或转成「首屏 GIF/短预告 + 全片按需」。
- 可选：为 10.1 MiB / 66 页 PDF 提供「压缩版预览 + 原件下载」。

### P2：懒加载与骨架屏
- `<video>` 无原生的 `loading="lazy"` 等价物；可改为「点击/滚动进入视口时才注入 `<source>`」的轻量 JS。
- 媒体面板在视频未就绪时显示骨架屏（CSS 动画占位）替代黑块。

### P3：CDN 与 HTTP 缓存
- 现状：GitHub Pages 自带 CDN + HTTPS（平台默认），但仓库内无自定义缓存头。
- 可做：对大媒体走 Git LFS 或独立对象存储/CDN，避免把 50 MB 二进制长期放在 git 历史里（当前 `.git` 已含这些大文件，`git clone` 会全量拉取）。

### P4：SEO / 可访问性增强
- 加 favicon、Open Graph/Twitter Card、`canonical`、JSON-LD（Person/CreativeWork）。
- 加 `:focus-visible` 样式、skip-link、视频字幕/文字描述（`<track>`）。
- 把 `h1` 从「项目」改为含姓名/定位的完整标题（`index.html:34`）。

### P5：交互增强（可选引入轻量 JS）
- 移动端汉堡菜单、`scroll-behavior` 的 `prefers-reduced-motion` 降级、导航滚动高亮（scrollspy）。

## 三、优先级矩阵（面试一张图说清）

| | 收益高 | 收益低 |
| --- | --- | --- |
| **成本低** | P0 加仓库链接、P1 补 poster/转码 | P4 favicon/OG/焦点样式 |
| **成本高** | P2 懒加载/骨架屏、P3 CDN/大文件治理 | P5 交互增强 |

## 四、面试总结句

> 「站点已经做对了零依赖、metadata 预取、响应式、H.265 压缩、外链安全这几件事；如果继续优化，我会优先把代码仓库链接进作品集（价值最高）、给 HEVC 视频加 H.264 备源并补封面，再做懒加载和 CDN。顺序是『先补可信度、再补体验、最后补工程化』。」
