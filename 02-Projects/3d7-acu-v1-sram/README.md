# 3D7 ACU v1 SRAM 项目入库索引

## 基本信息

- 项目名称：3D7_ACU_v1_sram
- 原始路径：`D:\AICodeProject\3m7\3m7acu\no.1\3D7_ACU_v1.21_251117_first\3D7_ACU_v1.21_251117_first\3D7_ACU_v1_sram`
- 工程目录：`3D7_ACU_v1`
- 工程文件：`3D7_ACU_v1.atsln`、`3D7_ACU_v1\3D7_ACU_v1.cproj`
- 当前状态：第一轮索引型入库，未复制源码。
- 入库日期：2026-05-08

## 入库结论

该项目是完整嵌入式工程，不适合整体放入知识库或 `00-Inbox/`。知识库只记录项目结构、关键路径、分析结论和可复用经验；原始工程继续保留在独立项目目录。

## 已确认事实

- 工程未检测到 Git 仓库。
- 文件总数约 699 个，总体积约 110 MB。
- 源码规模约 135 个 `.c` 文件、269 个 `.h` 文件。
- 编译产物包括 `.o`、`.d`、`.elf`、`.hex`、`.bin`、`.srec`、`.eep`、`.map`、`.lss`、`.a`。
- 工程配置显示工具链为 `com.Atmel.ARMGCC.C`。
- 目标器件为 `ATSAM4E16E`，器件系列为 `sam4e`。
- 工程使用 ASF、CMSIS、FreeRTOS、FreeRTOS TCP 相关代码。
- `main.c` 启动流程为：`sysclk_init()`、`board_init()`、`delay_init()`、`AppInitialize()`、`vTaskStartScheduler()`。
- 链接脚本 `flash.ld` 定义内部 Flash 1 MB、内部 SRAM 128 KB，默认栈大小 `0x3000`。

## 相关文档

- [项目结构初判](project-structure.md)
- [源码与配置索引](source-index.md)
- [构建产物处理规则](build-and-output.md)
- [后续分析计划](next-analysis-plan.md)

## 当前不确定点

- 任务创建位置和任务职责尚未完整梳理。
- SRAM 相关代码是否使用外部 SRAM，仍需进一步确认。
- CAN、TCP、UDP、串口协议的数据流尚未分析。
- `.map` 文件尚未做内存占用汇总。
- 原始工程是否包含敏感协议、客户信息或硬件私密参数，尚未人工确认。

