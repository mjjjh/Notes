# 基本汇编指令

![image-20231222143201437](C:/Users/Hduer/AppData/Roaming/Typora/typora-user-images/image-20231222143201437.png)



### 反汇编

![image-20231222144457098](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/RTthread/Rt202312221444212.png)

```shell
fromelf --text -a -c -output=xxx.dis xxx.axf
```





局部变量保存在栈里







# 堆、栈

堆：一块内存空间，可以从中分配出一个小Buffer，用完后再把它放回去

栈：也是一块内存空间，CPU的sp的寄存器指向它，可以用于函数调用、局部变量、多任务系统里保存现场





### 原理

![image-20231222170206641](C:/Users/Hduer/AppData/Roaming/Typora/typora-user-images/image-20231222170206641.png)

调用函数本质是汇编使用BL指令，把函数地址给PC，函数嵌套时LR被覆盖，**因此会先把LR的值push到栈中**，退出函数后再把LR值给PC。



Rtos每次在A，B间切换，需要**保存A/B的现场，恢复B/A的现场**

![image-20231225152047898](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/RTthread/Rt202312251520003.png)

保存时把所有寄存器信息存到栈里，在任务A的结构体里记录sp；恢复时找到A的结构体，找到sp，把所有的寄存器信息恢复到CPU里

> 多任务系统中每个任务都有自己的栈，因为每个任务都有自己的调用关系，都有自己的局部变量，都有自己的现场。

不同任务调用的相同函数是在不同栈里





# 任务

### 三要素

- 函数
- 栈（和TCB）
- 优先级





### 栈的大小

![image-20231225163957350](C:/Users/Hduer/AppData/Roaming/Typora/typora-user-images/image-20231225163957350.png)

调用深度考虑：![image-20231225163811516](C:/Users/Hduer/AppData/Roaming/Typora/typora-user-images/image-20231225163811516.png)







# 任务状态与调度理论

### 状态

- ready 《==》 running
- Blocked（等待某些event）  ，  Suspend（暂停）

![image-20231226105107797](C:/Users/Hduer/AppData/Roaming/Typora/typora-user-images/image-20231226105107797.png)

### 优先级

- 相同优先级的任务轮流运行
- 最高优先级的任务先运行



高优先级的任务未执行完，更低优先级的任务无法运行

一旦高优先级任务就绪，马上运行

最高优先级的任务有多个，他们轮流运行



### 链表管理

创建同优先级新任务TCB指针变化，**执行时最后指向先运行**

![image-20231226143855194](C:/Users/Hduer/AppData/Roaming/Typora/typora-user-images/image-20231226143855194.png)

任务若处于阻塞态，会在readlist删除，放入到其他数组中，tick中断会去遍历

任务若处于暂停态，会放入到suspend数组中，不可通过tick中断唤醒

vTaskDelay

vTaskSuspend

![image-20231226145543815](C:/Users/Hduer/AppData/Roaming/Typora/typora-user-images/image-20231226145543815.png)









### 任务循环

![image-20231226152334470](C:/Users/Hduer/AppData/Roaming/Typora/typora-user-images/image-20231226152334470.png)



### 任务退出

收尸：释放TCB、栈

空闲任务优先级最低为0，用来释放被删除任务的内存；它要么处于就绪态，要么处于运行态，永远不会阻塞。

- 自己delete：vTaskDelete(NULL);              **由空闲任务收尸**，由于优先级可能会导致无法执行，使得内存无法释放
- 其他任务delete: vTaskDelete(handle);      **由执行删除命令的任务收尸**



> 良好的编程习惯：
>
> - 事件驱动
> - 延时函数，不要使用死循环，使用阻塞形式的，让出CPU给空闲任务做某些事情





# 同步与互斥

阻塞代替循环不执行

关于通信协议的执行，需要互斥使用，不然协议格式会出错

![image-20231227165430044](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/RTthread/Rt202312271654461.png)



原因：修改变量的过程会被打断

解决方案：

- 修改变量时，执行之前关闭中断，执行完后再开启中断

  > 多任务时会导致CPU利用率低，另一个任务也会进入调度，不是阻塞

  ```C
  disable_irq();
  
  enable_irq();
  ```

- freertos



### Freertos解决方案

- 正确性
- 效率：等待者要进入阻塞
- 多种解决方案



![image1](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/RTthread/Rt202312280954604.png)





# 数据传输

**不要让多个任务同时修改同一个全局变量，会出现错误**

![image-20231228102722332](C:/Users/Hduer/AppData/Roaming/Typora/typora-user-images/image-20231228102722332.png)



### 环形缓冲区

![image-20231228104812064](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/RTthread/Rt202312281048182.png)

 ![image-20231228110014804](C:/Users/Hduer/AppData/Roaming/Typora/typora-user-images/image-20231228110014804.png)



### 队列

本质：环形缓冲区，加上互斥措施、阻塞-唤醒机制（超时阻塞、被唤醒）；两个链表（senderList、receiverList）。

如果这个队列不传输数据，只调整“数据个数”，它就是信号量（semaphore）。

如果信号量中，限定“数据个数”最大值为1，他就是互斥量（mutex）。



##### 创建

队列的创建有两种方法：动态分配内存、静态分配内存，

- 动态分配内存：xQueueCreate，队列的内存在函数内部动态分配

函数原型如下：

```c
QueueHandle_t xQueueCreate( UBaseType_t uxQueueLength, UBaseType_t uxItemSize );
```

| **参数**      | **说明**                                                     |
| ------------- | ------------------------------------------------------------ |
| uxQueueLength | 队列长度，最多能存放多少个数据(item)                         |
| uxItemSize    | 每个数据(item)的大小：以字节为单位                           |
| 返回值        | 非0：成功，返回句柄，以后使用句柄来操作队列 NULL：失败，因为内存不足 |

- 静态分配内存：xQueueCreateStatic，队列的内存要事先分配好

函数原型如下：

```c
QueueHandle_t xQueueCreateStatic(*
              		UBaseType_t uxQueueLength,*
              		UBaseType_t uxItemSize,*
              		uint8_t *pucQueueStorageBuffer,*
              		StaticQueue_t *pxQueueBuffer*
           		 );
```

| **参数**              | **说明**                                                     |
| --------------------- | ------------------------------------------------------------ |
| uxQueueLength         | 队列长度，最多能存放多少个数据(item)                         |
| uxItemSize            | 每个数据(item)的大小：以字节为单位                           |
| pucQueueStorageBuffer | 如果uxItemSize非0，pucQueueStorageBuffer必须指向一个uint8_t数组， 此数组大小至少为"uxQueueLength * uxItemSize" |
| pxQueueBuffer         | 必须执行一个StaticQueue_t结构体，用来保存队列的数据结构      |
| 返回值                | 非0：成功，返回句柄，以后使用句柄来操作队列 NULL：失败，因为pxQueueBuffer为NULL |

示例代码：

```c
// 示例代码
 #define QUEUE_LENGTH 10
 #define ITEM_SIZE sizeof( uint32_t )
 
 // xQueueBuffer用来保存队列结构体
 StaticQueue_t xQueueBuffer;

// ucQueueStorage 用来保存队列的数据

// 大小为：队列长度 * 数据大小
 uint8_t ucQueueStorage[ QUEUE_LENGTH * ITEM_SIZE ];

 void vATask( void *pvParameters )
 {
	QueueHandle_t xQueue1;

	// 创建队列: 可以容纳QUEUE_LENGTH个数据，每个数据大小是ITEM_SIZE
	xQueue1 = xQueueCreateStatic( QUEUE_LENGTH,
							ITEM_SIZE,
                            ucQueueStorage,
                            &xQueueBuffer ); 
  }
```



##### 复位

队列刚被创建时，里面没有数据；使用过程中可以调用 **xQueueReset()** 把队列恢复为初始状态，此函数原型为：

```c
/*  pxQueue : 复位哪个队列;
 * 返回值: pdPASS(必定成功)
*/
BaseType_t xQueueReset( QueueHandle_t pxQueue);
```



##### 删除

删除队列的函数为 **vQueueDelete()** ，只能删除使用动态方法创建的队列，它会释放内存。原型如下：

```c
void vQueueDelete( QueueHandle_t xQueue );
```



##### 写入

```c
/* 等同于xQueueSendToBack
 * 往队列尾部写入数据，如果没有空间，阻塞时间为xTicksToWait
 */
BaseType_t xQueueSend(
                                QueueHandle_t    xQueue,
                                const void       *pvItemToQueue,
                                TickType_t       xTicksToWait
                            );

/* 
 * 往队列尾部写入数据，如果没有空间，阻塞时间为xTicksToWait
 */
BaseType_t xQueueSendToBack(
                                QueueHandle_t    xQueue,
                                const void       *pvItemToQueue,
                                TickType_t       xTicksToWait
                            );


/* 
 * 往队列尾部写入数据，此函数可以在中断函数中使用，不可阻塞
 */
BaseType_t xQueueSendToBackFromISR(
                                      QueueHandle_t xQueue,
                                      const void *pvItemToQueue,
                                      BaseType_t *pxHigherPriorityTaskWoken
                                   );

/* 
 * 往队列头部写入数据，如果没有空间，阻塞时间为xTicksToWait
 */
BaseType_t xQueueSendToFront(
                                QueueHandle_t    xQueue,
                                const void       *pvItemToQueue,
                                TickType_t       xTicksToWait
                            );

/* 
 * 往队列头部写入数据，此函数可以在中断函数中使用，不可阻塞
 */
BaseType_t xQueueSendToFrontFromISR(
                                      QueueHandle_t xQueue,
                                      const void *pvItemToQueue,
                                      BaseType_t *pxHigherPriorityTaskWoken
                                   );
```

这些函数用到的参数是类似的，统一说明如下：

| 参数          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| xQueue        | 队列句柄，要写哪个队列                                       |
| pvItemToQueue | 数据指针，这个数据的值会被复制进队列， 复制多大的数据？在创建队列时已经指定了数据大小 |
| xTicksToWait  | 如果队列满则无法写入新数据，可以让任务进入阻塞状态， xTicksToWait表示阻塞的最大时间(Tick Count)。 如果被设为0，无法写入数据时函数会立刻返回； 如果被设为portMAX_DELAY，则会一直阻塞直到有空间可写 |
| 返回值        | pdPASS：数据成功写入了队列 errQUEUE_FULL：写入失败，因为队列满了。 |



##### 读取

使用 **xQueueReceive()** 函数读队列，读到一个数据后，队列中该数据会被移除。这个函数有两个版本：在任务中使用、在ISR中使用。函数原型如下：

```c
BaseType_t xQueueReceive( QueueHandle_t xQueue,
                          void * const pvBuffer,
                          TickType_t xTicksToWait );

BaseType_t xQueueReceiveFromISR(
                                    QueueHandle_t    xQueue,
                                    void             *pvBuffer,
                                    BaseType_t       *pxTaskWoken
                                );
```

参数说明如下：

| **参数**     | **说明**                                                     |
| ------------ | ------------------------------------------------------------ |
| xQueue       | 队列句柄，要读哪个队列                                       |
| pvBuffer     | bufer指针，队列的数据会被复制到这个buffer 复制多大的数据？在创建队列时已经指定了数据大小 |
| xTicksToWait | 果队列空则无法读出数据，可以让任务进入阻塞状态， xTicksToWait表示阻塞的最大时间(Tick Count)。 如果被设为0，无法读出数据时函数会立刻返回； 如果被设为portMAX_DELAY，则会一直阻塞直到有数据可写 |
| 返回值       | pdPASS：从队列读出数据入 errQUEUE_EMPTY：读取失败，因为队列空了。 |





##### 查询

可以查询队列中有多少个数据、有多少空余空间。函数原型如下：

```c
/*
 * 返回队列中可用数据的个数
 */
UBaseType_t uxQueueMessagesWaiting( const QueueHandle_t xQueue );

/*
 * 返回队列中可用空间的个数
 */
UBaseType_t uxQueueSpacesAvailable( const QueueHandle_t xQueue );
```



##### 覆盖/偷看

当队列长度为1时，可以使用 **xQueueOverwrite()** 或 **xQueueOverwriteFromISR()** 来覆盖数据。

注意，队列长度必须为1。当队列满时，这些函数会覆盖里面的数据，这也以为着这些函数不会被阻塞。

函数原型如下：

```c
/* 覆盖队列
 * xQueue: 写哪个队列
 * pvItemToQueue: 数据地址
 * 返回值: pdTRUE表示成功, pdFALSE表示失败
 */
BaseType_t xQueueOverwrite(
                           QueueHandle_t xQueue,
                           const void * pvItemToQueue
                      );

BaseType_t xQueueOverwriteFromISR(
                           QueueHandle_t xQueue,
                           const void * pvItemToQueue,
                           BaseType_t *pxHigherPriorityTaskWoken
                      );
```

如果想让队列中的数据供多方读取，也就是说读取时不要移除数据，要留给后来人。那么可以使用"窥视"，也就是**xQueuePeek()\**或\**xQueuePeekFromISR()**。这些函数会从队列中复制出数据，但是不移除数据。这也意味着，如果队列中没有数据，那么"偷看"时会导致阻塞；一旦队列中有数据，以后每次"偷看"都会成功。

函数原型如下：

```c
/* 偷看队列
 * xQueue: 偷看哪个队列
 * pvItemToQueue: 数据地址, 用来保存复制出来的数据
 * xTicksToWait: 没有数据的话阻塞一会
 * 返回值: pdTRUE表示成功, pdFALSE表示失败
 */
BaseType_t xQueuePeek(
                          QueueHandle_t xQueue,
                          void * const pvBuffer,
                          TickType_t xTicksToWait
                      );

BaseType_t xQueuePeekFromISR(
                                 QueueHandle_t xQueue,
                                 void *pvBuffer,
                             );
```



### 队列集

规范，硬件传递数据，软件处理数据

![image-20231228160100978](C:/Users/Hduer/AppData/Roaming/Typora/typora-user-images/image-20231228160100978.png)

**抽离**

![image-20231228160523310](C:/Users/Hduer/AppData/Roaming/Typora/typora-user-images/image-20231228160523310.png)

读取队列方式

- 轮询——不推荐，不能进入阻塞状态，极大浪费CPU
- 队列集——保存队列句柄，记录下了队列写入的顺序![image-20231228161326001](C:/Users/Hduer/AppData/Roaming/Typora/typora-user-images/image-20231228161326001.png)

![image-20231228161641950](C:/Users/Hduer/AppData/Roaming/Typora/typora-user-images/image-20231228161641950.png)



##### 创建

函数原型如下：

```c
QueueSetHandle_t xQueueCreateSet( const UBaseType_t uxEventQueueLength )
```

| **参数**      | **说明**                                                     |
| ------------- | ------------------------------------------------------------ |
| uxQueueLength | 队列集长度，最多能存放多少个数据(队列句柄)                   |
| 返回值        | 非0：成功，返回句柄，以后使用句柄来操作队列 NULL：失败，因为内存不足 |

##### 加入

函数原型如下：

```c
BaseType_t xQueueAddToSet( QueueSetMemberHandle_t xQueueOrSemaphore,
                               QueueSetHandle_t xQueueSet );
```

| **参数**          | **说明**                       |
| ----------------- | ------------------------------ |
| xQueueOrSemaphore | 队列句柄，这个队列要加入队列集 |
| xQueueSet         | 队列集句柄                     |
| 返回值            | pdTRUE：成功 pdFALSE：失败     |

##### 读取

函数原型如下：

```c
QueueSetMemberHandle_t xQueueSelectFromSet( QueueSetHandle_t xQueueSet,
                                                TickType_t const xTicksToWait );
```

| **参数**     | **说明**                                                     |
| ------------ | ------------------------------------------------------------ |
| xQueueSet    | 队列集句柄                                                   |
| xTicksToWait | 如果队列集空则无法读出数据，可以让任务进入阻塞状态，xTicksToWait表示阻塞的最大时间(Tick Count)。如果被设为0，无法读出数据时函数会立刻返回；如果被设为portMAX_DELAY，则会一直阻塞直到有数据可写 |
| 返回值       | NULL：失败， 队列句柄：成功                                  |



> 创建任务要在队列集放入后







# 规范

- 队列集使用
  - 初始化
  - 创建队列
  - 读队列
  - 处理数据
  - 写队列
- static静态变量外部使用，通过函数return
