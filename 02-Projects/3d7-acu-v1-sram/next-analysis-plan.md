# 后续分析计划

## 第一优先级：启动与任务

目标：搞清楚系统从 `main()` 到 FreeRTOS 多任务运行的完整路径。

入口：

- `src/main.c`
- `AppInitialize()`
- `modul/periodic_tasks`
- `FreeRTOSConfig.h`

输出：

- 启动流程图。
- 任务列表、优先级、栈大小、周期。
- 中断和任务之间的交互关系。
- 栈溢出、堆不足、优先级反转等风险点。

## 第二优先级：内存与 SRAM

目标：确认内部 RAM、外部 SRAM、堆、栈、全局变量和 FreeRTOS heap 的实际关系。

入口：

- `flash.ld`
- `Debug/3D7_ACU_v1.map`
- `driver/sram`
- `function/sram`
- `FreeRTOSConfig.h`

输出：

- Flash / RAM 使用汇总。
- 堆和栈配置说明。
- 外部 SRAM 初始化和访问方式。
- 大对象和长期占用内存清单。

## 第三优先级：通信链路

目标：明确外部通信如何进入系统，命令如何解析，结果如何回传。

入口：

- `driver/can`
- `driver/uart*`
- `driver/usart*`
- `lib/FreeRTOS_TCP`
- `modul/cmd_decode_modul`
- `app/protocol`

输出：

- CAN、UART/USART、TCP、UDP 通信路径。
- 协议帧格式和状态机。
- 接收缓冲、发送缓冲和并发访问风险。
- 常见通信故障定位方法。

## 第四优先级：轴与运动控制

目标：理解轴模块、规划模块、状态机和安全逻辑。

入口：

- `modul/axis_modul`
- `function/plan`
- `math_unit`
- `modul/safe_modul`
- `data/axis_data`

输出：

- 轴状态机。
- 规划与执行链路。
- 限位、安全、报警处理。
- 参数与运行状态数据结构。

