# ARM 栈指针与堆栈初始化

## 一句话结论

ARM 程序启动时要确保栈指针指向有效 RAM；在有多个异常模式的 ARM 架构中，还要为不同异常模式准备独立栈，避免异常发生时破坏主程序运行现场。

## 适用场景

- ARM7、ARM9、ARM A/R 等存在多种处理器模式和 banked SP 的场景。
- 裸机启动代码、Bootloader、异常向量表初始化。
- 需要理解 `STMFD`、`LDMFD`、`PUSH`、`POP` 等栈操作的场景。

对于 Cortex-M，要额外区分 MSP/PSP 机制，不能简单套用传统 ARM 多异常模式栈初始化模型。

## 核心概念

R13 通常作为栈指针，也就是 SP。传统 ARM 架构下，不同异常模式可能拥有独立的 banked R13，例如：

- `R13_usr`
- `R13_fiq`
- `R13_irq`
- `R13_svc`
- `R13_abt`
- `R13_und`

这样做的目的，是让 IRQ、FIQ、SVC、Abort、Undefined 等异常进入时，有自己的栈空间保存现场，降低不同执行上下文互相踩内存的风险。

## 四类栈模型

栈可以按两个维度分类：

- 满栈 / 空栈：SP 指向最后一个有效元素，还是下一个空位置。
- 递增 / 递减：栈向高地址增长，还是向低地址增长。

因此组合出四种模型：

- 满递减栈：Full descending
- 满递增栈：Full ascending
- 空递减栈：Empty descending
- 空递增栈：Empty ascending

ARM/Thumb C/C++ 调用约定通常使用满递减栈。也就是说，栈从高地址向低地址增长，SP 通常指向当前栈顶有效数据。

## 常见汇编别名

常见栈操作和 LDM/STM 地址模式的对应关系：

| 栈模型 | 入栈 | 出栈 |
|---|---|---|
| Full descending | `STMFD` / `STMDB` | `LDMFD` / `LDMIA` |
| Full ascending | `STMFA` / `STMIB` | `LDMFA` / `LDMDA` |
| Empty descending | `STMED` / `STMDA` | `LDMED` / `LDMIB` |
| Empty ascending | `STMEA` / `STMIA` | `LDMEA` / `LDMDB` |

示例：

```asm
STMFD r13!, {r0-r5}
LDMFD r13!, {r0-r5}
```

`!` 表示写回更新基址寄存器，也就是更新 SP。

## 工程注意事项

- 启动代码必须在调用 C 函数前初始化好栈，否则函数调用、局部变量、异常保存现场都可能出错。
- 异常模式栈空间不能过小，尤其是 IRQ/FIQ 中有嵌套调用、保存浮点上下文或调用复杂函数时。
- 不建议在中断或异常处理中随意调用深层函数链、格式化输出或大栈对象。
- Cortex-M 项目要重点检查向量表初始 MSP、`MSP/PSP` 切换、RTOS 任务栈和异常栈关系。

## 待确认点

- 该来源资料没有展开 Cortex-M 的 MSP/PSP 差异，后续应单独整理一篇 `Cortex-M 栈模型与 FreeRTOS 任务栈`。
- Empty descending 的文字解释需要与 ARM 官方手册再次核对。

## 相关资料

- `03-References/arm-stack-initialization-source.md`

