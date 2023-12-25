## 嵌入式HW 6

> 秦君豪 10204804421

#### (1）一个基本的I/0接口，一般包含哪些寄存器？这些寄存器的作用分别是什么？

- **数据寄存器:**

**作用**: 用于暂存从I/O设备传送来的数据或者待发送到I/O设备的数据。

- **状态寄存器**：

**作用**: 用来表示I/O设备的状态。

- **控制寄存器：**

**作用**: 用于控制I/O设备的操作。

- **地址寄存器：**

**作用**: 如果I/O设备支持直接内存访问（DMA），则地址寄存器用于存储数据传输的目标内存地址。



#### (2)一个GPIO引脚作为输入、输出使用时，有哪几种常用具体配置？各种配置的特点和应用场合？

- **输入模式**:

  * **特点**: 在输入模式下，GPIO引脚用于读取外部信号，如按钮、传感器等的状态。
  * **应用场合**: 适用于需要从外部设备获取数据的场合，如按钮按下检测、读取传感器数据等。

- **输出模式**:

  * **特点**: 在输出模式下，GPIO引脚可以驱动外部设备，如LED灯、继电器等。
  * **应用场合**: 适用于控制外部设备，如点亮LED灯、驱动继电器开关等。

- **上拉（Pull-up）和下拉（Pull-down）电阻配置**:

  * **特点**: 这些配置用于稳定输入模式下的GPIO引脚的电平状态，防止悬空输入造成的不确定状态。

  * 应用场合

    :

    * **上拉电阻**: 当外部设备在活动状态下提供低电平时使用，如按钮按下为低电平。
    * **下拉电阻**: 当外部设备在活动状态下提供高电平时使用，如按钮按下为高电平。

- **开漏（Open Drain）/开源（Open Source）配置**:

  * 特点

    :

    * **开漏配置**: 在这种模式下，GPIO引脚可以被拉低，但无法主动驱动到高电平，需要外部上拉电阻。
    * **开源配置**: 相反，可以主动驱动到高电平，但无法被拉低，需要外部下拉电阻。

  * **应用场合**: 适用于多个设备共享同一数据线的场合，如I²C通讯。

- **模拟输入**:

  * **特点**: 在模拟输入模式下，GPIO引脚可以读取模拟信号（如温度传感器的变化）并将其转换为数字信号。
  * **应用场合**: 适用于需要读取模拟信号的场合。

- **PWM（脉冲宽度调制）输出**:

  * **特点**: 这种模式允许GPIO引脚产生PWM信号，用于控制设备的功率，如电机速度或LED亮度。
  * **应用场合**: 适用于需要精细控制电源输出的场合，如电机速控、LED调光等。





#### (3)设计编写一个程序，用中断方式来读取一个按键状态，并控制一个 LED 的亮和灭。即按一下灯亮、再按一下灯灭，可重复操作，要求每次操作执行可靠

```
#include <ti/devices/msp432p4xx/driverlib/driverlib.h>
#include <stdint.h>
#include <stdbool.h>
int main(void)
{
    MAP_WDT_A_holdTimer();
    MAP_GPIO_setAsOutputPin(GPIO_PORT_P1, GPIO_PIN0);
    MAP_GPIO_setAsInputPinWithPullUpResistor(GPIO_PORT_P1, GPIO_PIN1);
    MAP_GPIO_clearInterruptFlag(GPIO_PORT_P1, GPIO_PIN1);
    MAP_GPIO_enableInterrupt(GPIO_PORT_P1, GPIO_PIN1);
    MAP_Interrupt_enableInterrupt(INT_PORT1);
    MAP_SysCtl_enableSRAMBankRetention(SYSCTL_SRAM_BANK1);
    MAP_Interrupt_enableMaster();
    while (1)
    {
        MAP_PCM_gotoLPM3();
    }
}
void PORT1_IRQHandler(void)
{
    uint32_t status;
    status = MAP_GPIO_getEnabledInterruptStatus(GPIO_PORT_P1);
    MAP_GPIO_clearInterruptFlag(GPIO_PORT_P1, status);
    if(status & GPIO_PIN1)
    {
        MAP_GPIO_toggleOutputOnPin(GPIO_PORT_P1, GPIO_PIN0);
    }
}

```







#### (8)如何使用PWM来控制一个LED的亮度变化？请通过实验测试不同PWM占空比与LED亮度的基本对应关系，并解释其原因。

PWM可以通过调整信号在一个周期内高电平与低电平的持续时间比例（即占空比）来控制平均功率输出。对于LED来说，这相当于控制流过LED的平均电流，从而调节亮度。





#### (12)简述看门狗定时器的基本概念及其使用方法。

看门狗定时器是一个特殊的定时器，可以通过对系统进行复位来防止系统故障，主要功能是看门狗功能和普通定时器功能。

1、WDT 设置一定的计时时间，使能后，计数器开始计数。当计时时间到后，则触发系统复位
2、如果在定时时间到达之前，进行喂狗(计数器重装) 动作，则不会引起系统复位。
3、看门狗的定时时间可以由用户设定，可以根据需要在指定的时间内复位系统
