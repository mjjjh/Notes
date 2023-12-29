# 系统架构

### F4

8+7

![image-20230515195230632](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230515195230632.png)

![image-20230515200516832](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230515200516832.png)





# 时钟树

![image-20230515164831965](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230515164831965.png)



![image-20230515165125584](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230515165125584.png)

### HAL时钟控制

时钟源、锁相环：HAL_RCC_OscConfig();

系统时钟、总线：HAL_RCC_ClockConfig();

使能外设时钟：__HAL_RCC_PWR_CLK_ENABLE();

扩展外设时钟（RTC/ADC/USB）:HAL_RCCEx_PeriphCLKConfig();





# 存储器映射

存储器指可以存储数据的设备，本身没有地址信息，对存储器分配地址的过程称为存储器映射

存储单元是按字节编制的
$$
2^{32} = 4G(字节)
$$

分块Block 





# 寄存器映射

给寄存器地址命名的过程

![image-20230515215109489](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230515215109489.png)

![image-20230515214721662](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230515214721662.png)

uint32_t 偏移量对应为4个字节





# 基于CMSIS应用程序文件描述

![image-20230516142252948](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230516142252948.png)



# HAL库文件

![image-20230516111709178](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230516111709178.png)



### 命名规则

##### 函数

![image-20230516112239619](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230516112239619.png)



##### 宏

![image-20230516112504019](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230516112504019.png)



##### 回调函数

![image-20230516112852395](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230516112852395.png)





# Map文件

![image-20230516150435039](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230516150435039.png)



# 启动

尝试：![image-20230516155041076](C:/Users/29557/AppData/Roaming/Typora/typora-user-images/image-20230516155041076.png)







# 通用外设驱动模型（四部法）

![image-20230517211706811](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230517211706811.png)







# GPIO（General Purpose Input Output）

寄存器组成

![image-20230517164553439](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230517164553439.png)

还有两个复用功  能寄存器

数据写入：可读写（ODR），只写（BSRR）

写入数据使用BSRR

![image-20230517193141269](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230517193141269.png)



### 使能

![image-20230517195125785](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230517195125785.png)





### 消抖

![image-20230517205724460](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230517205724460.png)



连按选择

![image-20230517211026829](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230517211026829.png)

需要判断按键状态







# 中断

意义：高效处理紧急程序，不会一直占用CPU资源



### 流程简图

![image-20230518145146684](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230518145146684.png)



### NVIC

Nested vectored interrupt controller：嵌套向量中断控制器，属于内核。

支持256个中断（16内核+240外部），支持：256个优先级 ，允许裁剪

##### 寄存器

![image-20230518150540167](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230518150540167.png)





##### NVIC工作原理

开启/关闭     优先选择

![image-20230518150930415](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230518150930415.png)





##### 优先级

抢占优先级（pre）

响应优先级（sub）

相同看自然优先级：NVIC表中的顺序



##### 使用

- 设置中断分组			HAL_NVIC_SetPriorityGrouping
- 设置中断优先级        HAL_NVIC_SetPriority
- 使能中断                    HAL_NVIC_EnableIRQ





### EXTI

External(Extended) interrupt/event Controller     外部（扩展）中断事件控制器

管理器件内部外部的唤醒事件

中断和事件的理解：

- 中断：要进入NVIC，有相应的中断服务函数，需要CPU处理
- 事件：不进入NVIC，仅用于内部硬件自动控制，如：TIM、DMA、ADC



##### 支持的外部中断/事件请求

![image-20230518191425192](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230518191425192.png)

​																								19					23						24

##### 特性

`F1/F4/F7`:

  - 每条EXTI线都可以`单独配置`:
    - 选择类型（中断或者事件）
    - 触发方式（上升沿，下降沿或者双边沿触发）
    - 支持软件触发
    - 开启/屏蔽
    - 有挂起状态位

`H7系列`：

   - 有其他外设对EXTI产生的事件分为可配置事件和直接事件。
     - 可配置事件：简单概括，基本和F1/F4/F7系列类似
     - 直接事件：固定上升沿触发、不支持软件触发、无挂起状态位（由其他外设触发）



##### EXTI工作原理

![image-20230518194542317](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230518194542317.png)



### 配置IO（映射关系）

**F1**：AFIO--------复用功能IO，主要用于重映射和外部中断映射配置

​		`AFIO_EXTICR1~4`,配置EXTI中断线0~15对应到哪个具体IO口

> 配置AFIO寄存器之前要使能AFIO时钟，方法如下：
>
> ​			__HAL_RCC_AFIO_CLK_ENABLE();     		对应RCC_APB2ENR寄存器的位0



**F4/F7/H7**:System configuration controller-----------SYSCFG	系统配置控制器，用于外部中断映射配置等

​			`SYSCFG_EXTICR1~4`配置EXTI中断线0~15对应到哪个具体IO口

> 配置SYSCFG寄存器之前要使能SYSCFG时钟，方法如下：
>
> ​			__HAL_RCC_SYSCFG_CLK_ENABLE();  



##### 配置原理

![image-20230518202242957](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230518202242957.png)

一次只能选择一个





### 使用

![image-20230518204258255](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230518204258255.png)

##### 具体配置

![image-20230518204707776](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230518204707776.png)



### 处理机制

![image-20230518205832495](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230518205832495.png)



### 不同功能中断回调函数在相应功能模块中







# 通信

![image-20230519143539238](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230519143539238.png)



### 常见串行通信接口

| 通信接口               | 接口引脚                   | 数据同步方式 | 数据传输方向 |
| ---------------------- | :------------------------- | ------------ | ------------ |
| UART（通用异步收发器） | TXD：发送端<br>RXD:接收端<br>GND：公共地 | 异步通信 | 全双工 |
| 1-wire | DQ：发送/接收端 | 异步通信 | 半双工 |
| IIC | SCL：同步时钟<br>SDA:数据输入/输出端 | 同步通信 | 半双工 |
| SPI | SCK：同步时钟<br>MISO：主机输入，从机输出<br>MPSI：主机输出，从机输入<br>CS：片选信号 | 同步通信 | 全双工 |



### 发送/接收简图

![image-20230519151702530](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230519151702530.png)



### 波特率计算

##### F1

![image-20230519153126989](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230519153126989.png)

简化：

![image-20230519153640248](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230519153640248.png)

再简化：

![image-20230519153722424](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230519153722424.png)



**例程写法**

![image-20230519153749165](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230519153749165.png)



##### F4

![image-20230519154933332](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230519154933332.png)



### IO引脚复用

![image-20230519165433966](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230519165433966.png)

![image-20230519165456469](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230519165456469.png)



##### 寄存器配置

![image-20230519170150799](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230519170150799.png)







# WDG

### IWDG

主要用于检查外界电磁干扰，或硬件异常导致的程序跑飞

用于高稳定性，对时间精度要求低的场合

![image-20230522102407281](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230522102407281.png)

![image-20230522103236690](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230522103236690.png)



##### 溢出公式计算

![image-20230522112706652](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230522112706652.png)



##### 配置

![image-20230522111909820](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230522111909820.png)





### WWDG

窗口看门狗

本质：能产生系统复位信号和提前唤醒中断的计数器

特性：![image-20230522135105993](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230522135105993.png)

喂狗：窗口期内重装载计数器的值，防止复位

作用：监测运行是否精准，主要检测软件异 常，用于精准监测程序运行时间的场合



##### 原理

![image-20230522135706386](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230522135706386.png)

W[6:0] ≥ 窗口期 >0x3F



##### 框图

![image-20230522140725984](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230522140725984.png)



##### 配置

![image-20230522204641512](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230522204641512.png)





### 两者区别

![image-20230522204222570](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230522204222570.png)









# 定时器

### 分类

![image-20230523141612527](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230523141612527.png)



### 特性

![image-20230523142623283](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/嵌入式image-20230523142623283.png)









### 功能区别

![image-20230523142847743](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230523142847743.png)





### 基本定时器

![image-20230523143428719](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230523143428719.png)

 

##### 计算公式

![image-20230523151006779](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230523151006779.png)



![image-20230525200802497](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230525200802497.png)



##### 配置步骤

![image-20230523151256486](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230523151256486.png)



##### 相关函数

![image-20230523151529822](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230523151529822.png)

> 记得开启HAL_TIM_Base_Start_IT()





### 通用定时器

![image-20230523202715881](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230523202715881.png)





![image-20230523203611246](C:/Users/29557/AppData/Roaming/Typora/typora-user-images/image-20230523203611246.png)



##### 输出PWM

![image-20230523205320007](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230523205320007.png)

模式：

![image-20230523210008397](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230523210008397.png)



##### 配置

![image-20230523210116563](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230523210116563.png)



##### 相关函数

![image-20230523210149714](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230523210149714.png)



##### 关键结构体

![image-20230523210733060](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230523210733060.png)





### 参数配置函数

![image-20230528140058116](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230528140058116.png)





# A/D

采样、保持、量化、编码 



![image-20230526142146185](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230526142146185.png)

![image-20230526142408240](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230526142408240.png)





### 转换过程

![image-20230526143006417](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230526143006417.png)



### 数据获取方式

##### 查询方式

![image-20230526161435939](C:/Users/29557/AppData/Roaming/Typora/typora-user-images/image-20230526161435939.png)



##### 中断方式

![image-20230526161649892](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/%E5%B5%8C%E5%85%A5%E5%BC%8Fimage-20230526161649892.png)



### 计算

电压：3.3v

位数：（可调）12位









# 中断API

> 开启要在初始化结束后



### 定时器

##### 开启

```c
HAL_TIM_Base_Start_IT(&htim6);
```

##### 回调

```c
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim);
```



### 串口接收

##### 开启

```c
HAL_UART_Receive_IT(&huart1,data,3);
```

##### 回调

```c
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart);
```



### ADC

##### 开启

```c
 HAL_ADC_Start_IT(&hadc1);
```

##### 回调

```c
void HAL_ADC_ConvCpltCallback(ADC_HandleTypeDef* hadc);
```

