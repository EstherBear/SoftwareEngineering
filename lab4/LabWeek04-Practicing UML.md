# LabWeek04-Practicing UML

https://github.com/EstherBear/SoftwareEngineering

## 0 人员介绍

| 姓名                 | 学号     |     | 姓名   | 学号     |
| -------------------- | -------- | --- | ------ | -------- |
| 劳倩茹（版本管理员） | 18340086 |     | 吴天   | 19334019 |
| 刘思然               | 19335141 |     | 赖奕恺 | 19335093 |

## 1 Use Case Diagram

### 1.1 需求分析

ATM 自动取款机是银行在银行营业大厅、超市、闹市区等设置的一种小型机器，利用一张信用卡大小的胶卡记录客户的基本户口资料让客户可以透过机器进行提款、存款、转帐等银行服务。

1. 客户将银行卡插入读卡器，读卡器识别卡的真伪，并在显示器上提示输入密码。

2. 客户通过键盘输入密码，取款机验证密码是否有效。如果密码错误提示错误信息，如果正确，提示客户进行选择操作的业务。

3. 客户根据自己的需要可进行存款、取款、查询账户、转账、修改密码的操作。

4. 在客户选择后，显示器进行交互提示和操作确认等信息。

5. 客户可在事务结束后选择打印交易凭证

6. 银行职员可进行对 ATM 自动取款机的硬件维护和添加现金的操作。

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image002.gif)

### 1.2 例图描述

用例图是软件需求到最终实现的第一步。用例图是使系统的用户更好的理解系统元素的用途，同时也便于软件开发人员最终的实现。

通过需求分析可以确定，ATM 自动取款机系统主要有两个参与者：银行管理员和客户。

- 银行管理员会启动或参与的业务是“资金调度”，如查询 ATM 机剩余金额、放入现金、取出现金，还能够“开启 ATM 系统”，“关闭 ATM 系统”。

- 客户可以登陆启动一个会话，会话包括了“取款”、“存款”、“转账”、“查询余额”、“修改密码”五个事务。

## 2 Class Diagram

### 2.1 类图综述

类图用于说明 ATM 机系统的概念模型，表现了 ATM 机该有的责任和目的，例如：用户登录系统后可进行存款、取款、转账、查询、打印凭条等操作。以下用主要类图、界面类图两个类图分别描述了 ATM 系统中实体类间的联系和界面之间的交互情况。

（1）主要类图，包含了用户、银行职员、客户、事务、ATM 机、凭条共 6 个实体类。其中，客户、银行职员由用户类派生而来。用户，即客户，银行职员都可以在 ATM 系统进行登录，客户还可以进行存款、取款、转账、查询余额、修改密码的操作，银行职员可以进行资金调度操作。

（2）界面类图，包含了主界面、存款界面、取款界面、转账界面、查询余额界面、修改密码界面、打印凭条界面、资金调度界面共 8 个界面类。其中主界面根据所选择的不同操作（除打印凭条操作）跳转到各个对应的操作界面。在取款界面、存款界面、转账界面进行打印凭条操作可跳转到打印凭条界面。

#### 2.1.1 主要类图

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image004.gif)

#### 2.1.2 界面类类图

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image006.gif)

### 2.2 关系描述

#### 2.2.1 主要类图

- 关联关系

  - 客户类与事务类关联，是一对多的关系，因为一个客户可以发起多个事务。
  - 事务类与 ATM 机类关联，是一对多的关系，因为一台 ATM 机可以完成多个事务。
  - 打印凭条接口与凭条类关联，是一对一的关系，因为一次打印凭条的任务对应一个凭条。

- 实现关系

  - 用户类实现了登录接口。
  - 银行职员类实现了资金调度接口。
  - 事务类实现了修改密码接口、存款接口、取款接口、转账接口、查询余额接口、打印凭条接口共 6 个接口。

- 继承关系

客户类、银行职员类这两类是由用户类继承而来的。

#### 2.2.2 界面类图

- 主界面与修改密码界面、存款界面、取款界面、转账界面、查询余额界面、资金调度界面交互关联。

- 打印凭条界面与取款界面、存款界面、转账界面交互关联。

## 3 Object Diagram

对象图描述的是参与交互的各个对象在交互过程中某一时刻的状态。和类图一样，对象图是对系统的静态设计或静态进程视图建模，对象图更注重现实或原型实例，这种视图主要支持系统的功能需求，对象图描述了静态的数据结构。对象图可以被看作是类图在某一时刻的实例。

我们以顾客、银行职员、ATM 机等多个对象相互交互的某一时刻为例，画出对应的对象图。各个对象都有其自己的属性，如 Customer 对象李华具备银行卡号 12345678、密码 mima0000 等属性；同时各个对象之间用链进行连接，表示它们之间的相互交互关系。

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image008.gif)

## 4 Component Diagram

组件图又称构建图，用于显示系统各组件及各组件关系的物理视图，它通常包括组件、接口、关系、端口和连接器，用来显示程序代码中相应的模块、源文件或源文件集合之间的依赖和泛化关系，组件是定义了良好接口的物理实现单元，是系统中可替换的物理部件。一般情况下，组件表示将类、接口等逻辑元素打包而形成的物理模块。一个组件包含它所实现的一个或多个逻辑类的相关信息，创建了一个从逻辑视图到组件视图的映射。

在 ATM 系统中，我将整个系统拆分为多个完整的组件，其包括用户，银行职员，ATM 提款机，ATM 屏幕，ATM 键盘，ATM 读卡器，ATM 网络，信用卡验证器。它们相互之间通过虚线箭头来表示它们的依赖关系。

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image010.gif)

## 5 Deployment Diagram

部署图用于展现系统的物理架构。以下是 ATM 的部署图，主要的物理节点包括 ATM 机和存储银行数据库的服务器，ATM 机和服务器通过内联网上的 HTTP 传输信息。其中每台 ATM 机物理上连接读卡器、键盘、日志设备、提款机、显示屏和收据打印机。

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image012.jpg)

## 6 State Chart Diagram

状态图用于展示一个类在运行过程中的状态转换。下图描述 ATM 机类的状态转换：

1. 从用户插入银行卡开始，进入读卡状态，读卡成功后，进入登陆状态。

2. 登录状态等待用户输入用户名和密码，若输入正确，则进入事务菜单展示状态；否则提示用户重新输入，继续保持登录状态。

3. 在菜单展示状态中，ATM 机会等待用户选择需要执行的事务，包括存取款、转账等，用户选择好待执行事务后将进入事务执行状态。

4. 在事务执行状态中，将根据用户选择的不同事务执行相应的操作，当事务执行成功或者失败后，如果用户选择退出，将进入出卡状态；若用户选择继续操作，将返回菜单展示状态。

5. 在出卡状态中，ATM 机会吐出卡片，然后进入结束状态。

6. 图中除了出卡状态外，在其余状态中，用户都可以通过选择立即退出来进入出卡状态。

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image014.jpg)

## 7 Activity Diagram

**活动图**是逐步活动和动作的工作流的图形表示，支持选择、迭代和并发。在统一建模语言中，活动图旨在对计算和组织过程（即工作流）以及与相关活动相交的数据流进行建模。

在我们的活动示例图中，展示了活动的顺序约束关系，有三个 swimlanes，因为我们有三个对象控制不同的活动：Customer，ATM Machine 和 Bank。处理过程从用户插入 ATM Card 开始。接下来 ATM Machine 会检查 ATM Card 的有效性，在检查结果为无效的情况下退还卡。如果验证成功，则用户输入 PIN 交由银行进行检查，检查失败则退还卡。如果 Bank 验证成功则 Customer 可以进行操作，并在这之后 ATM 打印凭条，最后弹出卡片，整个流程以用户取走卡片为结束。

### 7.1 ATM 用户交互

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image016.gif)

### 7.2 取款 活动图

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image016.gif)

### 7.3 存款 活动图

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image018.gif)

### 7.4 转账 活动图

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image020.gif)

## 8 Sequence Diagram

在我们的时间序列图中，突出对象交互的时序关系，按照时间顺序显示三个对象之间的交互：三个对象分别是 Customer C、 ATM A 还有 Account Acc。

序列图以平行的 lifeline 显示同时存在的不同进程或对象，并以水平箭头显示它们之间交换的消息，按它们发生的顺序。这允许以图形方式指定简单的运行时场景。

### 8.1 ATM 用户交互

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image022.gif)

### 8.2 取款 序列图

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image024.gif)

### 8.3 存款 序列图

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image026.gif)

### 8.4 转账 序列图

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image028.gif)

## 9 Communication Diagram

在我们的通信图中，突出对象之间的关系，更加紧凑地显示三个对象的交互：分别是 Customer C、ATM A 和 Account Acc。

通信图根据顺序消息对对象或部分之间的交互进行建模。通信图表示从描述系统的静态结构和动态行为的类、序列和用例图中获取的信息的组合。

通信图使用对象图中使用的对象和链接的自由形式排列。为了在这种自由形式的图表中保持消息的顺序，消息用时间顺序编号标记，并放置在发送消息的链接附近。

通信图显示了与序列图相同的信息，但由于信息的呈现方式，其中一些信息在一张图中比另一张更容易找到。通信图显示了每个元素与哪些元素更好地交互，但序列图更清楚地显示了交互发生的顺序。

### 9.1 ATM 用户交互

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image030.gif)

### 9.2 取款 通信图

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image032.gif)

### 9.3 存款 通信图

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image034.gif)

### 9.4 转账 通信图

![Generated](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/typora/clip_image036.gif)
