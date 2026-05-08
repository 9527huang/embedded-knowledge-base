# 源码与配置索引

## 原始工程路径

```text
D:\AICodeProject\3m7\3m7acu\no.1\3D7_ACU_v1.21_251117_first\3D7_ACU_v1.21_251117_first\3D7_ACU_v1_sram
```

## 关键入口

- `3D7_ACU_v1\src\main.c`
  - 初始化系统时钟。
  - 初始化板级硬件。
  - 初始化 delay。
  - 调用 `AppInitialize()`。
  - 调用 `vTaskStartScheduler()` 进入 FreeRTOS 调度。
  - 包含 `HardFault_Handler()`，但当前处理逻辑主要是读取故障寄存器后死循环。

- `3D7_ACU_v1\app\initialize\...`
  - 需要继续追踪 `AppInitialize()` 的实现。
  - 这是任务创建、模块初始化和系统启动顺序的优先分析入口。

## 关键配置

- `3D7_ACU_v1\3D7_ACU_v1.cproj`
  - Atmel Studio / ARM GCC 工程配置。
  - 目标器件：`ATSAM4E16E`。
  - 器件系列：`sam4e`。
  - FPU 配置显示为启用。

- `3D7_ACU_v1\src\config\FreeRTOSConfig.h`
  - `configUSE_PREEMPTION = 1`
  - `configTICK_RATE_HZ = 1000`
  - `configMAX_PRIORITIES = 8`
  - `configMINIMAL_STACK_SIZE = 70`
  - `configTOTAL_HEAP_SIZE = 32 * 2016`
  - `configUSE_TIMERS = 1`
  - `configTIMER_TASK_PRIORITY = configMAX_PRIORITIES - 1`
  - `configCHECK_FOR_STACK_OVERFLOW = 2`
  - `configUSE_MUTEXES = 1`
  - `configUSE_COUNTING_SEMAPHORES = 1`

- `3D7_ACU_v1\src\ASF\sam\utils\linker_scripts\sam4e\sam4e16\gcc\flash.ld`
  - Flash：`0x00400000` 起始，长度 `0x00100000`。
  - RAM：`0x20000000` 起始，长度 `0x00020000`。
  - 默认栈大小：`0x3000`。

## 值得后续重点阅读的文件

- `AppInitialize()` 所在源文件。
- `hardware_sram_initialize.*`
- `hardware_can_initialize.*`
- `hardware_gmac_initialize.*`
- `FreeRTOSConfig.h`
- `flash.ld`
- `3D7_ACU_v1.map`
- `modul/periodic_tasks`
- `modul/cmd_decode_modul`
- `modul/axis_modul`
- `modul/safe_modul`

## 记录规则

不要把完整源码复制到知识库。后续分析源码时，只摘录必要的短代码片段，并优先记录：

- 调用链。
- 初始化顺序。
- 数据流。
- 任务和中断协作。
- 内存占用。
- 关键参数和适用条件。
- 风险点和调试方法。

