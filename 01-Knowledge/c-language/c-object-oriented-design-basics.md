# 嵌入式 C 的对象化设计基础

## 一句话结论

C 语言没有 `class` 和 `this`，但可以用 `struct` 表达对象状态，用函数接收对象指针表达方法，用头文件接口隐藏实现细节，从而写出更适合扩展和维护的嵌入式代码。

## 适用场景

- 同类硬件存在多个实例，例如多个 LED、多个 UART、多个电机轴、多个 CAN 通道。
- 上层业务逻辑希望复用，底层硬件端口、引脚、寄存器或驱动实现可能变化。
- 模块内部状态较多，不希望全局变量散落在多个文件中。
- 需要把驱动、协议、业务状态机拆成边界清晰的模块。

## 核心思路

对象化设计的第一步不是模拟 C++ 语法，而是回答三个问题：

- 哪些状态属于同一个对象。
- 外部应该通过哪些接口操作对象。
- 对象从初始化到销毁的生命周期由谁负责。

对于嵌入式 C，最常见的表达方式是：

```c
typedef struct {
    GPIO_TypeDef *port;
    uint16_t pin;
    GPIO_PinState on_level;
} Led;

void led_init(Led *self, GPIO_TypeDef *port, uint16_t pin, GPIO_PinState on_level);
void led_on(Led *self);
void led_off(Led *self);
```

这里的 `Led` 描述一类对象的状态，`led_run`、`led_err` 这类变量才是具体实例。`led_on(&led_run)` 和 `led_on(&led_err)` 调用同一套函数，但操作不同对象。

## `self` 指针

C 语言没有隐式 `this`，因此对象化函数通常显式传入当前对象指针：

```c
void led_on(Led *self)
{
    if (self == NULL) {
        return;
    }

    HAL_GPIO_WritePin(self->port, self->pin, self->on_level);
}
```

`self` 的价值是让同一套行为逻辑适配多个实例。它避免为每个对象复制一组 `led1_on()`、`led2_on()`、`led3_on()` 之类的函数。

## 封装边界

弱封装做法是把结构体定义放在头文件里，外部可以看到并修改所有成员。这种方式简单、开销低，适合小型驱动或性能敏感场景，但边界较弱。

强封装可以使用不透明指针：

```c
/* key.h */
typedef struct Key Key;

void key_init(Key *self);
void key_scan(Key *self, uint8_t raw_level, uint32_t now_ms);
uint8_t key_get_state(const Key *self);
```

```c
/* key.c */
struct Key {
    uint8_t level;
    uint8_t last_level;
    uint32_t tick_ms;
    uint8_t stable_state;
};
```

外部只知道 `Key` 是一种类型，并通过接口使用它，不依赖内部字段。这能减少模块外部对内部实现的耦合。

## 生命周期

对象不是声明出来就一定可用。嵌入式 C 里要明确对象生命周期：

1. 声明或创建对象。
2. 初始化对象，使其进入合法状态。
3. 通过接口使用对象。
4. 不再使用时反初始化、释放资源或复位状态。

典型命名：

- `xxx_init()`
- `xxx_deinit()`
- `xxx_create()`
- `xxx_destroy()`
- `xxx_start()`
- `xxx_stop()`

在 MCU 工程中，很多对象更适合静态分配，再用 `init` 建立状态；除非有明确内存管理策略，不应随意在驱动层大量使用动态分配。

## 工程注意事项

- 对象内部状态如果会被 ISR 和任务同时访问，要设计临界区、锁、消息队列或单写者模型。
- `self == NULL` 检查可以提高接口鲁棒性，但不能替代系统级错误处理策略。
- 头文件暴露越少，模块替换越容易；但过度隐藏会增加调试成本。
- 对象化不是目标，降低重复、收拢状态、稳定接口才是目标。
- 对于寄存器驱动、协议栈、运动控制轴、通信通道，对象化通常比大量全局变量更可维护。

## 和嵌入式工程的关系

STM32 HAL 中的 `UART_HandleTypeDef`、`SPI_HandleTypeDef`、`TIM_HandleTypeDef` 就是典型对象化风格：句柄保存实例、配置、状态，HAL 函数围绕句柄工作。

同样思想可以迁移到项目代码中：

- `MotorAxis`
- `CanChannel`
- `ProtocolParser`
- `SoftTimer`
- `FlashStorage`
- `SafetyMonitor`

关键是让“状态”和“操作状态的函数”保持一致的归属边界。

## 待深入主题

- 用函数指针实现多态。
- 用接口表适配不同硬件驱动。
- 静态对象池替代 `malloc`。
- RTOS 下对象状态的并发保护。
- 面向对象风格和状态机的结合。

## 相关资料

- `03-References/embedded-c-oop-design-wechat-source.md`
- `01-Knowledge/c-language/embedded-c-learning-roadmap.md`

