# 07 硬件交互（作品集与嵌入式项目的对应关系）

> 站点本身不碰任何硬件。**本文是整套材料对「嵌入式面试」最有价值的一篇**：把站点上「展示的项目/技能」逐条映射到「作者 GitHub 上的代码证据」，明确哪些有代码、哪些只有展示——面试官最爱问「这个项目代码在哪」，答案全在这里。

## 一、先给结论（最重要的三句话）

1. **站点展示的 4 个项目，没有任何一个在作者公开仓库里找到对应代码**——它们只有视频/PDF 报告，属于「展示型材料」。
2. **作者真正的代码仓库（5 个）里，最有含金量的几个（无人机小车、ESP32 记录仪、蓝牙工具、飞控）反而完全没出现在站点上**。
3. 站点是 2026-06-12 搭建的「门面」，而代码仓库多为 2026-08 才创建——**时间上先有门面、后有代码**，所以门面没来得及链接代码。

> 证据：`git log` 3 个 commit 均为 2026-06-12；GitHub API 显示作者其余仓库 `created_at` 在 2026-08-07 ~ 2026-08-31。

## 二、站点展示项目 → 代码证据 映射表

| 站点项目 | 站点材料（证据） | 对应 GitHub 仓库 | 证据状态 |
| --- | --- | --- | --- |
| 双目视觉处理系统 | 视频 `binocular-vision.mp4` + 报告 `binocular-vision-report.pdf`（66 页） | 未找到 FPGA/双目视觉公开仓库 | 【待本人确认】 |
| 基于 CH585 的三模无线鼠标 | 视频 `ch585-mouse.mp4`（H.265） | 未找到 CH585（沁恒 WCH）仓库；作者有 `ac632n-dev-tools` 但是**杰理 AC632N**（另一厂商） | 【待本人确认】 |
| 简易自行瞄准装置 | 报告 `auto-aim-device-report.pdf`（14 页，元数据含 `E2025261001`） | 未找到 | 【待本人确认】 |
| 运动目标控制与自动追踪系统 | 报告 `moving-target-tracking-report.pdf`（9 页） | 未找到 | 【待本人确认】 |

> 「E2025261001」来自 PDF `/Title` 元数据前缀，疑似竞赛编号（电子设计竞赛 E 题？），属【根据代码推断】，需本人确认。

## 三、作者 GitHub 公开仓库全景（代码证据，未上站点）

| 仓库 | 描述（GitHub 原文） | 语言 | 对应技能/项目 | 证据状态 |
| --- | --- | --- | --- | --- |
| `uav-car-adhesion-platform` | 无人机正压攀附平台与 STM32 四舵轮小车控制代码、PX4 覆盖层及进度文档 | C | STM32F103 运动控制、FreeRTOS、UART、PX4 | ✅ 有代码（`car/`、`adhesion/px4-overlay/`、`docs/`） |
| `esp-recorder` | 维特智能 ESP32-S3 WiFi 数据记录仪（ESP-IDF + FreeRTOS + Flutter） | C | 实习项目、LwIP、双 UART 透传、SD、USB CDC/MSC | ✅ 有代码（`firmware/esp_recorder_v1.1~v1.3`、`app/`、`docs/`） |
| `esp32-wifi-recorder-v1.2` | ESP32-S3 WiFi dual-UART data recorder（SD、web UI、USB CDC+MSC） | C | 同上（v1.2 固件快照，含 `CMakeLists.txt`/`partitions.csv`/`sdkconfig`） | ✅ 有代码 |
| `ac632n-dev-tools` | 杰理 AC632N 蓝牙开发工具集（构建/烧录/UART 调试/日志 GUI/日报自动化） | C | 蓝牙、脚本化工程、串口 | ✅ 有代码（`sdk/`、`work/`、`python_tools/`、`build_uart_gui/`） |
| `-_MicoAir743v2_ArduPilot` | MicoAir743v2 ArduPilot Copter-4.7.0 飞控开发资料包（开发指南、环境验收、SITL、基线固件） | Python | 飞控、ArduPilot、SITL | ✅ 有代码/资料（README 描述，详见下方说明） |

> 注：`-_MicoAir743v2_ArduPilot` 的 README 抓取因仓库名含 `-_` 前缀未直接命中，但其 `description` 字段（GitHub API 返回）已明确内容为「飞控开发资料包」，此处以 API 描述为证据。

## 四、技能声明 → 代码证据 映射（交叉核对）

把站点 6 张技能卡（`index.html:79-121`）逐条对照作者仓库：

| 站点技能声明 | 能在作者仓库找到的代码证据 | 状态 |
| --- | --- | --- |
| C/C++ 嵌入式编程 | `uav-car-adhesion-platform/car/firmware`（STM32 Keil 工程）、`esp-recorder/firmware`（ESP-IDF C） | ✅ |
| FreeRTOS | `esp-recorder`（ESP-IDF 即基于 FreeRTOS）、小车 README 提到 FreeRTOS | ✅（esp-recorder 直接证据） |
| STM32 / MSPM0 / CH32 | `uav-car-adhesion-platform`（STM32F103） | 部分✅（MSPM0/CH32 未找到） |
| UART / SPI / I2C | `esp-recorder`（dual-UART 透传）、小车（PS2 链路/UART） | ✅ |
| LwIP / 网络协议栈 | `esp-recorder`（WiFi AP+Station、web 配置） | ✅ |
| Python 调试脚本 | `ac632n-dev-tools/python_tools/jieli_uart_logger.py`、`esp-recorder/scripts/build_report_*.py` | ✅ |
| PID / 串级 PID | 站点文字声明；小车 README 明确「未启动编码器 PID」（见下） | ⚠️【待本人确认】 |
| 图像二值化/颜色阈值分割 | 未找到代码 | 【待本人确认】 |
| FPGA（Vivado/Vitis HLS） | 未找到公开代码 | 【待本人确认】 |
| Altium / 嘉立创 EDA 电路设计 | `uav-car-adhesion-platform/car/hardware`（CAR-MOTOR-V1.2S 原理图） | 部分✅ |
| CH585 蓝牙 | 无（`ac632n-dev-tools` 是杰理 AC632N） | 【待本人确认】 |

**一个必须诚实面对的点**：`uav-car-adhesion-platform/README.md` 明确写到「当前不是『整机已交付验证』的声明」「没有启动 FreeRTOS、编码器 PID、ROS 或飞控通信」——即小车仓库**主动划清了「已实测/待实测」边界**。这是作者工程诚实的表现，面试时可直接引用，反而是加分项。

## 五、站点未展示、但值得在面试主动讲的项目（关键建议）

站点遗漏了作者最强的代码证据，面试时应主动补上：

1. **`esp-recorder`（维特智能实习项目）**：ESP32-S3 + ESP-IDF + FreeRTOS，AP+Station 混合模式、双 UART 透传、SD 记录、USB CDC/MSC 复合设备、Flutter App——这是「实习期间真实交付」的强证据，且与简历「深圳维特智能实习」直接对应。
2. **`uav-car-adhesion-platform`（无人机+四轮小车）**：STM32F103 四舵机四电机小车 + PX4 飞控覆盖层，含完整 `TECHNICAL_DEVELOPMENT_DOCUMENT.md`/`CODE_MAP.md`/`PROJECT_STATUS.md`，能体现「文档化 + 证据边界」的工程素养。
3. **`ac632n-dev-tools`（杰理蓝牙工具链）**：把蓝牙 SDK 构建/烧录/串口日志/日报自动化脚本化，体现「工程效率意识」。

**面试话术模板**：
> 「这个作品集站是我 6 月搭的『可视化门面』，把 4 个项目的视频和报告放上去给 HR 看。但我的代码主要在另外几个仓库：实习做的 ESP32-S3 WiFi 记录仪在 `esp-recorder`，无人机+STM32 四轮小车在 `uav-car-adhesion-platform`，杰理蓝牙工具集在 `ac632n-dev-tools`。站点本身还没把这些仓库链接进去，这是我下一步要补的。」

## 六、结论（一句话）

> 站点是「展示层」，代码在「仓库层」，二者目前**没有链接**；面试时要以「站点讲门面、仓库讲代码、报告讲深度」的三层结构来组织，把缺链接这件事讲成「已知待办」而非「藏拙」。
