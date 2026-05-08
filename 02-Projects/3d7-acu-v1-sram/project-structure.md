# 项目结构初判

## 顶层结构

原始工程顶层包含：

- `.vs/`：Visual Studio / Atmel Studio IDE 缓存，不应进入知识库或源码仓库。
- `3D7_ACU_v1/`：主要工程目录。
- `3D7_ACU_v1.atsln`：Atmel Studio solution 文件。
- `UpgradeLog.htm`：工程升级日志，可作为参考资料。

## 主要工程目录

- `app/`：应用初始化、硬件初始化、协议对接等上层入口。
- `board/`：板级配置和板级初始化相关代码。
- `data/`：轴、通信、常量、Flash、规划、系统、传输等数据结构或数据管理。
- `driver/`：CAN、Flash、SPI、SRAM、UART、USART 等底层驱动。
- `function/`：beacon、CAN、Flash、limit、math、plan、SRAM、trans、UART 等功能模块。
- `lib/FreeRTOS_TCP/`：FreeRTOS TCP 网络栈相关代码。
- `math_unit/`：数学计算模块。
- `modul/`：轴模块、命令解析模块、周期任务、安全模块、系统模块、上传模块等业务模块。
- `src/ASF/`：Atmel Software Framework、CMSIS、FreeRTOS 及芯片支持代码。
- `src/config/`：工程配置文件，例如 `FreeRTOSConfig.h`、时钟、串口、SPI 等配置。
- `use/`：保留参数、motor 等偏应用侧使用模块。
- `Debug/`：构建输出目录，不应进入知识库。

## 初步架构判断

项目大体可分为四层：

1. 芯片和框架层：ASF、CMSIS、链接脚本、启动文件、芯片外设定义。
2. 驱动层：CAN、UART、USART、SPI、Flash、SRAM 等硬件接口。
3. 中间功能层：FreeRTOS、FreeRTOS TCP、数学、规划、传输、Flash 管理。
4. 业务模块层：轴控制、系统状态、命令解析、安全、周期任务、上传、人机或监控协议。

## 需要优先梳理的结构问题

- `app_initialize` 内部创建了哪些任务。
- `periodic_tasks` 与 FreeRTOS tick、软件定时器或周期调度的关系。
- `cmd_decode_modul` 中 TCP、UDP、命令解析与业务模块之间的数据流。
- `axis_modul`、`system_modul`、`safe_modul` 的状态机关系。
- `driver/sram` 与 `function/sram` 是否对应外部 SRAM 初始化和封装访问。

