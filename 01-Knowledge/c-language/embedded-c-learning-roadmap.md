# 嵌入式 C 语言学习路线

## 一句话结论

嵌入式 C 的学习重点不是背语法，而是用内存、编译器和工程架构三个视角理解代码如何控制硬件、如何被构建、如何长期维护。

## 来源

- `03-References/embedded-c-learning-guide-source.md`
- 原始 PDF：`00-Inbox/嵌入式C语言学习指南-v2.pdf`

## 三个核心视角

## 内存视角

嵌入式软件直接面对寄存器、RAM、Flash、外设和总线。GPIO、UART、I2C、SPI、CAN 等外设在 C 代码中通常表现为内存映射寄存器访问。

应重点掌握：

- 地址、指针、数组、结构体布局。
- 代码段、只读数据段、全局数据段、堆、栈。
- 栈溢出、堆溢出、野指针、空指针。
- `volatile` 与外设寄存器访问。
- 对齐、打包、大小端、位操作。

## 编译器视角

C 代码不是直接运行，必须经过预处理、编译、汇编、链接和格式转换。很多嵌入式问题出现在构建链路或编译器优化之后。

应重点掌握：

- `gcc -E`、`gcc -S`、`gcc -c`、链接和 `objcopy`。
- 头文件找不到、语法错误、链接 undefined reference 等错误类型。
- 宏定义、条件编译、字符串化 `#`、连接符 `##`。
- `__attribute__((packed))`、`__attribute__((section))`、`weak`、`used`。
- `.map` 文件、链接脚本、启动文件。

## 函数与架构视角

C 是面向过程语言，但仍然可以写出模块化、可维护的工程结构。关键是用函数、结构体、函数指针、回调和接口分层表达边界。

应重点掌握：

- 函数参数传递的本质。
- 值传递和地址传递。
- 回调函数和 `void *` 上下文。
- 弱符号和可覆盖默认实现。
- 结构体封装状态和接口。
- 模块职责划分和依赖方向。

## 推荐学习顺序

1. 搭建 Linux 或类 Unix 编译环境，熟悉 Shell、Vim、GCC、GDB。
2. 理解 C 程序从 `.c` 到可执行文件的完整构建过程。
3. 系统学习预处理、关键字、运算符和类型系统。
4. 深入学习指针、数组、结构体和内存布局。
5. 结合 MCU 寄存器、CMSIS 头文件和外设驱动理解 `volatile`。
6. 学习链接脚本、启动文件、栈、堆、段和 `.map` 文件。
7. 通过真实工程拆解模块化设计、回调、弱符号、接口隔离和状态机。

## 后续拆分主题

- `gcc-build-pipeline.md`
- `c-preprocessor-macro-safety.md`
- `volatile-in-embedded-c.md`
- `static-extern-const-in-c.md`
- `pointer-array-memory-model.md`
- `c-callback-and-interface-design.md`
- `linker-script-and-map-file-basics.md`

## 待核查点

- 涉及 ISO C 标准行为的内容，应优先查 C 标准或权威编译器文档。
- 涉及 GCC 扩展的内容，应查 GCC 官方文档。
- 涉及 CMSIS、ARM、MCU 寄存器的内容，应查 ARM 或芯片厂商官方文档。

