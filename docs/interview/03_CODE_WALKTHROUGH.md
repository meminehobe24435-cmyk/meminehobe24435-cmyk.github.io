# 03 代码走读（Code Walkthrough）

> 逐区块讲解 `index.html`（282 行）与 `styles.css`（526 行）的真实实现。所有「代码在哪」均给到行号，可直接在仓库对照。

## 0. 文件总览

| 文件 | 行数 | 大小 | 职责 |
| --- | --- | --- | --- |
| `index.html` | 282 | 11,972 B | 页面结构、文案、媒体引用、SEO meta |
| `styles.css` | 526 | 9,671 B | 主题变量、布局、响应式、动效 |

---

## 1. `<head>`：SEO 与基础声明（index.html:3-12）

```html
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>尤译庆 | 嵌入式开发实习生项目集</title>
<meta name="description" content="尤译庆的嵌入式开发实习生项目作品集，包含项目演示视频与报告文档。" />
<link rel="stylesheet" href="styles.css" />
```

- `viewport`（`:5`）开启移动端响应式；`title` + `description`（`:6-10`）是唯一的 SEO 文本入口。
- 样式是**外部样式表**（`:11`），`index.html` 内无 `<style>`、无 `<script>`（可全文搜索确认）。
- **缺失项**（供 13_CODE_REVIEW 引用）：无 favicon、无 Open Graph/Twitter Card、无 canonical、无 JSON-LD。

## 2. `<header>` 粘性导航（index.html:14-27）

- `<nav aria-label="主导航">`（`:15`）提供无障碍导航标签。
- 品牌链接 `href="#top"` 带 `aria-label="返回首页"`（`:16`）。
- 4 个锚点：`#skills / #projects / #awards / #contact`（`:21-24`）。
- 对应样式：`position:sticky; top:0` + `backdrop-filter:blur(18px)`（`styles.css:44-51`），滚动时导航悬浮在顶部。

## 3. `section.hero` 首屏（index.html:30-60）

- 装饰网格 `hero-grid` 用 `aria-hidden="true"` 对读屏器隐藏（`:31`），对应 `styles.css:117-124` 的 `linear-gradient` 网格。
- `h1` 为「项目」（`:34`）——**语义偏弱**：主标题应更明确包含「姓名/定位」关键词（见 13）。
- CTA 两个按钮：`#projects` 跳转 + 直接打开代表报告 PDF（`target="_blank" rel="noreferrer"`，`:40-44`）。
- stats 概览：`4 个项目 / 2 个演示视频 / 3 份报告`（`:45-58`），带 `aria-label="作品集概览"`。

## 4. `section.intro` 求职方向（index.html:62-71）

- `h2`「嵌入式开发实习生」+ 一段说明：`关注 MCU/FPGA、传感器数据处理、运动控制、视觉算法落地与软硬件联调`（`:68`）。
- 这是「目标岗位」的自我定位，面试时应与这段话保持一致口径。

## 5. `section#skills` 技能卡（index.html:73-123）

6 张 `<article>` 卡：程序开发 / 协议调试 / 电路设计 / 硬件调试 / 算法应用 / FPGA 开发。
- 每卡用 `<span>` 做卡头 + 若干 `<p>` 描述（`:79-121`）。
- 布局 `grid-template-columns: repeat(3, minmax(0,1fr))`（`styles.css:262-266`），`min-height:250px`（`:274-277`）。
- 面试注意：技能描述里的「精通」「熟练掌握」措辞强度高（如 `:82` `精通 C/C++`），面试官可能针对「精通」追问，需准备能落地的证据。

## 6. `section#projects` 项目展示（index.html:125-201）—— 本站核心

4 个 `<article class="project">`，每个是「媒体面板 + 项目文案」两栏（`.project` 网格 `styles.css:301-309`）：

1. **双目视觉处理系统**（featured，`:131-149`）
   - `<video controls preload="metadata" poster="" aria-label="…">` + `<source src="assets/videos/binocular-vision.mp4" type="video/mp4">` + 回退文案（`:133-136`）。
   - 双按钮：观看视频（新标签打开 mp4）+ 查看报告（打开 66 页 PDF）（`:145-146`）。
   - `.project.featured` 加青色边框高亮（`styles.css:311-313`）。
2. **基于 CH585 的电竞级高性能超低延迟三模无线鼠标**（`:151-168`）：只有视频，无报告链接（`:165`）。
3. **简易自行瞄准装置**（`:170-184`）：无视频，用 `placeholder-panel` 占位「REPORT」，仅报告链接（`:181`）。
4. **运动目标控制与自动追踪系统**（`:186-200`）：占位「TRACKING」，仅报告链接（`:197`）。

**占位块实现**：`.placeholder-panel` 用网格 + 中心化文字（`styles.css:334-339`），`REPORT`/`TRACKING` 用不同描边色区分（`:341-350`）。——这说明两个「只有报告」的项目**缺少演示视频/截图**，是内容完整度的可见缺口。

## 7. `section#awards` 奖项（index.html:203-255）

- 8 张 `award-card`（国家一等奖、TI 杯省一、瑞萨杯二等奖等）+ 证书条 `C1 驾驶证 / 高压电工证`（`:250-254`）。
- `.award-board` 四栏 Grid（`styles.css:383-387`），`.highlight` 卡加青蓝渐变（`:394-399`）。
- 面试注意：奖项名称、年份、赛道是「可被追问」的硬信息，务必与实际证书一致（本仓库无证书原件，标注【待本人确认】）。

## 8. `section#contact` 联系（index.html:257-279）

- 邮箱 `mailto:`、电话 `tel:`、微信文本三栏（`:266-277`），`.contact-list` 三栏 Grid（`styles.css:430-435`）。
- 邮箱 `meminehobe24435@gmail.com` 与 GitHub 用户名 `meminehobe24435-cmyk` 同源，可作为「个人身份一致性」的小观察点。

---

## 9. `styles.css` 关键实现走读

| 机制 | 实现 | 行号 |
| --- | --- | --- |
| 暗色主题变量 | `:root { color-scheme:dark; --bg… }` | 1-14 |
| 全局盒模型 | `* { box-sizing:border-box }` | 16-18 |
| 平滑滚动 | `html { scroll-behavior:smooth }` | 20-22 |
| 网格背景 | `body` 双层 `linear-gradient` | 24-32 |
| 系统字体栈 | `Inter, system-ui, "Microsoft YaHei"…` | 33-35 |
| 粘性导航+毛玻璃 | `sticky` + `backdrop-filter` | 44-51 |
| 首屏高度 | `.hero { min-height:calc(100vh - 64px) }` | 97-104 |
| 首屏装饰渐变 | `.hero::before` 多层渐变 | 106-115 |
| 流式字号 | `clamp()` on h1/h2/h3 | 152-172 |
| 按钮 hover 上浮 | `.button:hover { translateY(-2px) }` + transition | 187-203 |
| stats 三栏 | `.stats { grid-template-columns:repeat(3,1fr) }` | 216-222 |
| 项目两栏布局 | `.project { grid-template-columns:…0.98fr…1.02fr }` | 301-309 |
| 视频自适应 | `.media-panel video { width:100%; object-fit:contain }` | 325-332 |
| 响应式塌缩 | `@media (max-width:760px)` 全部改 `1fr` | 463-512 |
| 极窄屏 | `@media (max-width:480px)` 隐藏品牌字、按钮全宽 | 514-526 |

---

## 10. 资源清单（含二进制解析结果）

| 资源 | 路径 | 大小 | 格式/编码 | 对应项目 | 引用处 |
| --- | --- | --- | --- | --- | --- |
| 视频 | `assets/videos/binocular-vision.mp4` | 27.0 MiB | MP4 H.264 (`avc1`)，≈3:25，≈960×544 | 双目视觉 | `index.html:134,145` |
| 视频 | `assets/videos/ch585-mouse.mp4` | 11.5 MiB | MP4 H.265 (`hvc1`)，≈2:36，960×544 | CH585 鼠标 | `index.html:154,165` |
| 报告 | `assets/reports/binocular-vision-report.pdf` | 10.1 MiB | PDF 1.7 / 66 页 | 双目视觉 | `index.html:41,146` |
| 报告 | `assets/reports/moving-target-tracking-report.pdf` | 1.64 MiB | PDF 1.7 / 9 页 | 目标追踪 | `index.html:197` |
| 报告 | `assets/reports/auto-aim-device-report.pdf` | 1.23 MiB | PDF 1.7 / 14 页 | 自瞄装置 | `index.html:181` |

## 11. GitHub 现场演示顺序（边点边讲）

1. 打开 `https://meminehobe24435-cmyk.github.io/` → 指首屏 stats「4/2/3」。
2. 点「查看项目」→ 滚到 `#projects`，展示 featured 双目视觉卡。
3. 点双目视觉「观看视频」→ 播放（说明 `preload=metadata` 未拖慢首屏）。
4. 点「查看报告」→ 新标签打开 66 页 PDF，说明「完整报告」。
5. 展示 CH585 鼠标视频（说明 H.265 编码省体积）。
6. 展示两个占位卡（REPORT/TRACKING），**如实说明这两项暂无演示视频**。
7. 滚到 `#skills`/`#awards` 收尾，说明站点是「可视化门面」，代码细节在对应 GitHub 仓库。
