# 04 核心模块（Core Modules）

> 站点没有「业务模块」，只有「展示区块」。本文按面试可讲的粒度，把每个区块当「模块」讲：职责 → 结构 → 关键样式/行为 → 面试追问。

## 模块总览

| 模块 | 定位 | 代码位置 |
| --- | --- | --- |
| 导航（Nav） | 锚点式单页导航 | `index.html:14-27` / `styles.css:44-95` |
| 首屏（Hero） | 3 秒建立「我是谁 + 做了什么」 | `index.html:30-60` / `styles.css:97-135` |
| 技能（Skills） | 能力雷达 | `index.html:73-123` / `styles.css:262-295` |
| 项目展示（Projects） | 作品核心证据 | `index.html:125-201` / `styles.css:301-350` |
| 媒体播放（Media Panel） | 视频原生播放 + 报告入口 | `index.html:131-199` |
| 奖项（Awards） | 荣誉背书 | `index.html:203-255` / `styles.css:379-428` |
| 联系（Contact） | 转化入口 | `index.html:257-279` / `styles.css:430-461` |

---

## 1. 导航模块（Nav）

- **结构**：`<header class="site-header">` > `<nav aria-label="主导航">` > 品牌链接 + `.nav-links`（4 个锚点）。
- **行为**：锚点跳转是浏览器原生行为，配合 `html { scroll-behavior:smooth }`（`styles.css:21`）实现平滑滚动，**零 JS**。
- **粘性**：`.site-header` 用 `position:sticky; top:0; z-index:10` + `backdrop-filter:blur(18px)`（`styles.css:44-51`）悬浮。
- **响应式风险点**：`@media (max-width:760px)` 只把 `nav-links` 的 `gap` 收到 4px（`:468-470`），**没有汉堡菜单**；在 320px 极窄屏上 4 个链接可能拥挤（`body min-width:320px`，`:26`）。这是可讲到的「移动端导航未做折叠」的真实局限。
- **追问**：锚点导航的 `#` 会改变 URL，如何避免刷新？→ A：`#` 只改 hash，不触发 HTTP 请求；若要做「无 hash 的平滑滚动」需 JS 拦截 `preventDefault()`。

## 2. 首屏模块（Hero）

- **职责**：一句副标题 `Embedded Systems Portfolio`（`index.html:33`）+ 大字 `h1`「项目」+ 说明文案 + 两个 CTA + stats。
- **结构**：`.hero` 占满约一屏（`min-height:calc(100vh - 64px)`，`styles.css:97-104`），内部 `.hero-grid` 与 `.hero::before` 是纯装饰层。
- **CTA 策略**：主按钮 `#projects`（站内转化）+ 次按钮直接打开「代表报告」PDF（`index.html:40-44`）——即「给没耐心的 HR 一条最快看到证据的路径」。
- **追问**：为什么 `h1` 只有「项目」两个字？→ A：视觉上追求极简，但从 SEO/语义角度应改成「尤译庆 · 嵌入式项目集」之类；这是一个**已知可改进点**（诚实承认比狡辩加分）。

## 3. 技能模块（Skills）

- **结构**：6 张 `<article>`，卡头 `<span>` + 描述 `<p>`。
- **布局**：三栏 Grid（`styles.css:262-266`），`min-height:250px`。
- **内容要点**（面试需能对应上证据）：
  - 程序开发：C/C++、Python 脚本、FreeRTOS/RT-Thread、STM32/GD32/MSPM0/CH32、Keil/VSCode/CodeWarrior（`:80-85`）。
  - 协议调试：UART/I2C/SPI/One-Wire、LwIP、示波器/逻辑分析仪（`:88-92`）。
  - 电路设计：Altium Designer、嘉立创 EDA、Gerber、华秋 DFM/DFX（`:95-99`）。
  - 硬件调试：焊接、板级测试、PCBA 打样到量产导入（`:102-106`）。
  - 算法应用：PID/串级 PID、图像二值化/颜色阈值分割（`:109-113`）。
  - FPGA：Xilinx/高云、Vivado/Vitis/Vitis HLS、摄像头驱动/流水线/IP 核（`:116-120`）。
- **关键审计结论**：这些技能在**本站只有文字声明，无代码证据**；作者的部分技能（如 FreeRTOS/STM32/UART）能在**其他仓库**找到代码证据（`uav-car-adhesion-platform`、`esp-recorder`），但 FPGA、双目视觉、CH585 蓝牙没有公开仓库佐证 → 面试可能被要求「举例证明」（见 07）。

## 4. 项目展示模块（Projects）

- **结构**：4 个 `<article class="project">`，`featured` 修饰第一个（`styles.css:311-313` 青色边框）。
- **卡片 = 媒体面板 + 文案**：`.project` 两栏 Grid（`styles.css:301-309`）。
- **文案结构**：`<span class="tag">`（技术标签）→ `h3`（项目名）→ `<p>`（一句话简介）→ `.project-actions`（按钮）。
- **tag 语义**：`border-left:3px solid var(--red)` + 绿色等宽字（`styles.css:365-373`），视觉上像「日志标签」。

| 项目 | 标签 | 材料 | 说明 |
| --- | --- | --- | --- |
| 双目视觉处理系统 | FPGA · 双目视觉 | 视频 + 66 页报告 | featured |
| CH585 三模无线鼠标 | CH585 · 三模无线 · 低延迟 | 仅视频 | 无报告链接 |
| 简易自行瞄准装置 | 控制系统 · 自瞄装置 | 仅报告（占位 REPORT） | 无视频 |
| 运动目标控制与自动追踪系统 | 运动控制 · 目标追踪 | 仅报告（占位 TRACKING） | 无视频 |

## 5. 媒体播放模块（Media Panel）

- **视频实现**：`<video controls preload="metadata" poster="" aria-label="…">`（`index.html:133-136, 153-156`）。
  - `controls`：原生控制条，零 JS。
  - `preload="metadata"`：只预取 moov 元数据，不全量下载（首屏优化核心）。
  - `poster=""`：**空**，未设置封面 → 播放前是黑底（`.media-panel video { background:#050607 }`，`styles.css:331`）。这是体验缺口。
  - `aria-label`：给视频的无障碍名称。
  - fallback 文本：`当前浏览器不支持视频播放，请下载视频文件查看。`（`:135,155`）。
- **报告入口**：`<a target="_blank" rel="noreferrer" href="…pdf">`（`:41,146,181,197`）——`rel="noreferrer"` 防 reverse tabnabbing，是安全意识细节。
- **占位块**：`.placeholder-panel`（`styles.css:334-339`）中心化显示 `REPORT`/`TRACKING` 大字，替代缺位的缩略图。
- **追问**：`preload="metadata"` vs `preload="none"` vs `preload="auto"`？→ A：`auto` 允许浏览器按需预载（可能全量），`metadata` 只取时长/尺寸/首帧，`none` 完全不下。本站选 `metadata` 兼顾「首屏不重 + 播放前能显示时长」。

## 6. 奖项模块（Awards）

- **结构**：8 张 `award-card`（`<span>` 等级 + `h3` 赛事名 + `<p>` 赛道）+ `.cert-strip` 证书条。
- **高亮**：前两项（国家一等奖、TI 杯省一）加 `.highlight` 渐变（`styles.css:394-399`）。
- **面试注意**：奖项是「可核验」信息，名称/年份/赛道需与证书一致；本仓库无证书扫描件，仅文字 → 面试官可能要求出示佐证，标注【待本人确认】。

## 7. 联系模块（Contact）

- **结构**：`.contact-list` 三栏——`mailto:` 邮箱、`tel:` 电话、微信文本（`:266-277`）。
- **细节**：`.contact-list strong { overflow-wrap:anywhere }`（`styles.css:452-456`）防止长邮箱撑破卡片。
- **语义**：`mailto:`/`tel:` 触发系统默认邮件/拨号，是「零 JS 的转化闭环」。

---

## 模块间关系（面试总结句）

> 全站由 7 个纯展示模块线性堆叠，交互全部依赖 HTML 原生能力（锚点/视频控件/mailto/tel），样式集中在单份 `styles.css`，用 CSS 变量统一主题、Grid + 断点统一布局。这种「无 JS 单页」架构的**优点**是零依赖、首屏轻、不易出错；**代价**是缺少懒加载、骨架屏、折叠菜单等增强体验，也无法把「项目卡片」动态挂到 GitHub 仓库 API 上自动同步。
