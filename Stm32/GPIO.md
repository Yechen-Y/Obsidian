# 1.概述
GPIO是通用输入输出端口（General-purpose input/output）的英文简写，是所有的微控制器必不可少的外设之一，可以由STM32直接驱动从而实现与外部设备通信、控制以及采集和捕获的功能。STM32单片机的GPIO被分为很多组，每组有16个引脚，不同型号的MCU的GPIO个数是不同的。

# 2.GPIO工作原理

## 2.1 上拉电阻与下拉电阻

***上拉电阻***
1：输出模式下将浮空状态转化为高电平状态。
下图位上拉电阻不同取值的优缺点。
![[Pasted image 20240212161554.png|上拉电阻]]

2：输入模式下可以将电平钳位在高电平的同时也可以输入低电平

***下拉电阻***

在输入模式下将电平钳位在低电平同时也可以输出高电平
## 2.2 GPIO位结构

![[GPIO基本框架.png]]

## 2.3 工作模式

STM32的GPIO共有8种工作模式，分别是输入模式的模拟输入、上拉输入、下拉输入和浮空输入以及输出模式的推挽输出、开漏输出、推挽复用输出和开漏复用输出

通过配置GPIO的端口配置寄存器，端口可以配置成以下8种模式

![[GPIO模式.png]]
### 输入模式

略。见上图

### 输出模式

![[输出模式4种情况.png]]
推挽输出：高低电平组成（驱动能力强）
开漏输出：低电平和高阻态组成（能驱动低电压设备/多个设备控制一个设备）通常与上拉电阻结合使用  ***[[开漏输出与上拉电阻.png|图例]]***
复用输出：即将控制权交给外设而非MCU


# 3.GPIO相关寄存器
STM32的每组GPIO都包含7个寄存器，分别是：

    - GPIOx_CRL:端口配置低寄存器
    - GPIOx_CRH:端口配置高寄存器
    - GPIOx_IDR:端口输入寄存器
    - GPIOx_ODR:端口输出寄存器
    - GPIOx_BSRR:端口位设置/清除寄存器
    - GPIOx_BRR :端口位清除寄存器
    - GPIOx_LCKR:端口配置锁存寄存器
    - AFIO_EVCR ：事件控制寄存器
    - AFIO_MAPR ：复用重映射和调试I/O配置寄存器

相关细节看数据手册
# 4.库函数配置

## 4.1 标准库

### 4.1.1 初始函数
```
void GPIO_Init(GPIO_TypeDef* GPIOx, GPIO_InitTypeDef* GPIO_InitStruct);
void GPIO_StructInit(GPIO_InitTypeDef* GPIO_InitStruct);
```
### 4.1.2 读取函数
```
uint8_t GPIO_ReadInputDataBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
uint16_t GPIO_ReadInputData(GPIO_TypeDef* GPIOx);
uint8_t GPIO_ReadOutputDataBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
uint16_t GPIO_ReadOutputData(GPIO_TypeDef* GPIOx);
```
### 4.1.3 写入函数
```
void GPIO_SetBits(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
void GPIO_ResetBits(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
void GPIO_WriteBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin, BitAction BitVal);
void GPIO_Write(GPIO_TypeDef* GPIOx, uint16_t PortVal);
```

### 4.1.4 AFIO函数
```
void GPIO_PinRemapConfig(uint32_t GPIO_Remap, FunctionalState NewState);
void GPIO_EXTILineConfig(uint8_t GPIO_PortSource, uint8_t GPIO_PinSource);
```
重映射和中断引脚选择

## 4.2 HAL库









