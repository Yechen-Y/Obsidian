# 1.概述

EXTI可以监测指定GPIO口的电平信号，当其指定的GPIO口产生电平变化时，EXTI将立即向NVIC发出中断申请，经过NVIC裁决后即可中断CPU主程序，使CPU执行EXTI对应的中断程序。

EXTI有20/19个输入。其中16个来自不同的GPIO，其余4个来自其他外设。***[[Screenshot 2024-02-12 215538.png|图例]]***
# 2.工作原理

## 2.1 基本框架

![[EXTI基本框架.png]]

## 2.2 NVIC

NVIC属于内核，相当于cpu的小管家，用来处理来自外设的中断请求并给他们分配优先级。

![[Screenshot 2024-02-12 215815.png]]


# 3.相关寄存器

	- EXTI_IMR
	- EXTI_EMR
	- EXTI_RTSR
	- EXTI_FTSR
	- EXTI_SWIER
	- EXTI_PR

# 4.库函数配置

## 4.1 NVIC

```
void NVIC_PriorityGroupConfig(uint32_t NVIC_PriorityGroup);//设置优先级分组
void NVIC_Init(NVIC_InitTypeDef* NVIC_InitStruct);

typedef struct
{
uint8_t NVIC_IRQChannel;//中断通道的选择
uint8_t NVIC_IRQChannelPreemptionPriority;//抢占优先级
uint8_t NVIC_IRQChannelSubPriority;//响应优先级
FunctionalState NVIC_IRQChannelCmd;//中断使能
}
NVIC_InitTypeDef;
```

## 4.2 EXTI

### 4.2.1 初始化函数

```
void EXTI_Init(EXTI_InitTypeDef* EXTI_InitStruct);
void EXTI_StructInit(EXTI_InitTypeDef* EXTI_InitStruct);//初始化结构体 有默认值
```

### 4.2.2 标志位/状态查询和清楚

```
FlagStatus EXTI_GetFlagStatus(uint32_t EXTI_Line);
void EXTI_ClearFlag(uint32_t EXTI_Line);
ITStatus EXTI_GetITStatus(uint32_t EXTI_Line);
void EXTI_ClearITPendingBit(uint32_t EXTI_Line);
```

>EXTI_GetFlagStatus 只是纯粹读取中断标志位的状态，但是不一定会响应中断（EXT_IMR 寄存器对该中断进行屏蔽）；而 EXTI_GetITStatus 除了读取中断标志位，还查看 EXT_IMR 寄存器是否对该中断进行屏蔽，在中断挂起 & 没有屏蔽的情况下就会响应中断。
