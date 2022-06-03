# Lab Week 15: System Implementation

网站地址：http://oj.skywatch.top/

项目GitHub地址：https://github.com/EstherBear/SoftwareEngineering

# 0 人员介绍

| 姓名                 | 学号     |      | 姓名   | 学号     |
| -------------------- | -------- | ---- | ------ | -------- |
| 劳倩茹（版本管理员） | 18340086 |      | 吴天   | 19334019 |
| 刘思然               | 19335141 |      | 赖奕恺 | 19335093 |

# 1 引言

本文档的目的是详细地介绍人工智能(AI)教学在线评测与游戏网站的系统模块设计与编码实现，以便项目开发人员了解产品大致和详细的设计与实现，方便用户对整个项目的使用指南和理解。本文将结合文字描述，对系统模块设计进行介绍，对于单独的关键模块，我们通过流程图、时序图、接口说明、附注等方式来描述对网站系统的模块结构设计和程序的详细设计。本文档的预期读者是开发人员和维护人员，以及跟该项目相关的其他人员。

# 2 系统功能结构

在本系统的功能结构设计中，主要包含了以下6个关键的模块：

- 登录模块：提供给用户在网站登录和注册的功能
- 前端页面模块: 运行在浏览器上展现给用户浏览的网页，管理与用户交互 
- 评测模块：提供给用户在网站提交代码，并以游戏形式显示运行结果的功能 
- 课程模块：提供给学习者
- 社区模块：实现了项目的社区功能
- 数据治理模块：项目中所有数据库的顶层管理者，接受来自其他程序模块的数据请求，返回其他模块所请求数据 

对于每个单独的模块，我们采取了分层的架构进行设计，主要包含表示层UI和业务逻辑层BLL。在表示层中，我们暴露出不同模块之间相互通信的接口，在业务逻辑层中，每个单独的模块又包含了大量的子程序，例如内存管理子程序、消息监听子程序、合法性检验子程序，业务逻辑实现子程序，通信实现子程序等等，这些子程序之间相互协作，接受来自用户的输入，通过内部的业务逻辑和对其他模块的数据交互，消息通信，共同完成用户的请求，返回给用户对应的提示和输出。

系统功能结构图如下所示：

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725593.png)

# 3 关键模块说明

## 3.1 登录模块

### 3.1.1 功能描述

登录模块主要提供注册和登录两个功能,方便用户查看自己的历史使用情况，同时也方便网站管理用户，并为后续提供用户定制化服务奠定基础。

- 若用户选择登录，程序接收用户输入的用户名和密码，判断用户名是否和用户列表数据库中的某一条记录匹配并且密码相同，如果是，输出登录成功；否则，输出登录失败，并等待用户重新输入。
- 若用户选择注册，程序接收用户输入的新用户名和密码，判断用户名是否和用户列表数据库中的某一条记录匹配或者密码是否过于简单，如果是，输出注册失败，并等待用户重新输入；否则，输出注册成功。

### 3.1.2 运行流程

登录模块接收来自用户输入的信息和请求，包括用户名和密码字符串，注册或登录等信息，通过内部子程序的数据匹配和查询，返回给用户结果信息，指示登录或注册是否成功，其运行流程如下所示：

1. 若是登录请求，查询用户名和密码是否和已有的用户记录匹配。
   1. 若查询到匹配的用户记录，调用登录接口，返回登录成功信息。
   2. 否则，返回登录失败信息。
2. 若是注册请求，查询新用户名和已有的用户名记录是否匹配以及密码是否过于简单。
   1. 若新用户名和已有的用户名记录匹配或者密码过于简单，返回注册失败信息。
   2. 否则，调用注册接口，返回登陆成功信息。

运行流程图如下：

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725584.png)

### 3.1.3 时序描述

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725603.png)

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725673.png)

### 3.1.4 接口描述

登录模块连接前端以及数据治理模块，它接收来自前端的请求，简单的逻辑处理后，向数据治理模块发出对应的查询请求，得到结果后再经过简单的逻辑处理，把结果信息返回给前端。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725540.png)

### 3.1.5 附注

1. 注册功能和登录功能设计的模块和消息的传递大致相同，故在此一同描述。

## 3.2 前端模块

### 3.2.1 功能描述

前端页面模块即网站前台部分，是运行在PC端，移动端等浏览器上展现给用户浏览的网页。网页存储在Web服务器上，当用户需要查看网页时，浏览器将经历一系列过程，将网页渲染呈现给用户。前端页面模块主要管理与用户交互的功能。一方面， 前端页面模块将根据用户的输入，进行合法性检测，检测通过后给后端发送相应的请求，另一方面， 前端页面模块能够把从后端获取的结果呈现到各个页面上。

### 3.2.2 运行流程

首先，前端界面模块进行一定的并发控制：当请求数大于指定的阀值时，将剩余请求存起来，等待有请求请求完毕后，再推出。当满足并发压力下，前端模块接收来自用户输入的请求，通过一系列渲染过程，返回给用户可视化的页面展示。其运行流程如下所示：

Pre. 判读请求数是否大于指定的阀值

1. 若请求数不大于指定阈值

直接将所有用户请求都发送给后端。

1. 若请求数大于指定阈值
   1. 超过阈值的请求进行await，状态设置为pending。
   2. 如果有请求执行完毕就推出一个，将状态置为Fulfilled

运行流程图如下：

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725652.jpeg)



### 3.2.3 时序描述

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725433.png)



### 3.2.4 接口描述

前端界面模块连接用户以及除数据治理模块的其他所有模块，它接收来自用户的请求，向除数据治理模块的其他模块发出对应的请求，并接收返回结果再展示给用户。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725321.png)

### 3.2.5 附注

1. 时序图中是对在threshold为1的情况下，模块可能表现出的行为的举例。

## 3.3 课程模块

### 3.3.1 功能描述

课程模块实现了多种多样的社区功能，主要包括：

- 学习者可以根据课程ID加入到由特定教学者创建的课程中进行学习；也可以通过自由搜索、选择其他教学者用户发布的网课视频进行自由学习。
- 学习者可以管理自己加入的课程列表，包括对课程进行删除、评分等功能。
- 学习者在加入课程后，可以根据教学者的要求提交作业，并得到来自教学者的批改反馈。
- 学习者可以根据教学者的要求，参加教学者建立的考试进行水平检测。

### 3.3.2 运行流程

课程模块接收来来自用户对课程系统的不同操作请求，通过内部子程序的相互协作完成操作，返回给用户对应的操作结果，其运行流程如下所示：

1. 课程模块持续监听，接收请求
2. 前端模块接收来自用户的操作，将请求发送给课程模块
3. 课程模块将请求在链表上排队
4. 检查程序对当前请求和已调度请求进行死锁检查
5. 调度进程池的空闲进程来处理请求
6. 向数据治理模块发出对应的数据请求
7. 根据用户权限和请求数据等信息完成操作请求
8. 返回给用户对应的状态信息

运行流程图如下：

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725511.png)

### 3.3.3 时序描述

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725636.png)



### 3.3.4 接口描述

课程模块与前端模块、数据治理模块进行通信交互，相互查询和交换数据信息。此外，课程模块具备死锁检查子程序，进程调度子程序，进程池维护子程序，交互通信子程序，业务逻辑子程序等等，它们彼此的相互合作，完成了接收来自用户的操作请求，返回给用户相应的状态信息。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725335.png)

### 3.3.5 附注

1. 死锁检测、进程调度、进程池和通讯子模块在各个模块间共享。

## 3.4 评测模块

### 3.4.1 功能描述

评测模块主要提供对代码文件进行评测并返回结果的功能。针对用户提交的代码文件，评测模块会把代码文件分配给一个评测节点，评测节点会对代码进行评测，如果没有编译错误，返回代码的评分以及AI游戏的控制序列，评分直接返回给前端，而控制序列发送给游戏引擎方便其显示画面；否则返回编译错误。

### 3.4.2 运行流程

评测模块接收来来自用户输入的代码文件，通过内部子程序的OJ程序进行判题，返回给用户对应的代码的运行结果或是对应的可视化，其运行流程如下所示：

1. 评测服务器接收到代码文件后会进行基本的检查，然后放入消息队列。
2. 消息队列中的评测请求会由评测节点进行处理。
3. 评测节点会返回评测结果：
   1. 若出现编译错误，返回出错信息。
   2. 否则，返回分数和动作序列，分数直接返回给前端，动作序列发送到游戏引擎。

运行流程图如下：

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725408.png)

### 3.4.3 时序描述

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725132.png)



### 3.4.4 接口描述

评测模块接收来自前端的请求，简单的逻辑处理后，调用评测节点完成评测结果。根据评测结果，向数据治理模块发出对应的修改用户数据的请求。最后把评测分数返回给前端，并把游戏的动作序列发送给游戏引擎。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725126.png)

### 3.4.5 附注

1. 消息队列用于缓冲请求，以缓冲可能的短时间内的大量提交。

## 3.5 社区模块

### 3.5.1 功能描述

社区模块实现了多种多样的社区功能，其主要包括：

- 用户发表和管理自己的博客，查看和评论他人的博客，搜索特定的博客
- 用户建立话题，发起自己问题，回答他人的问题
- 热门话题、博客、问答推荐
- 收藏、关注、分享他人的博客、问答等

以上的这些功能我们用统一的端到端模式进行实现，不同的功能操作我们视为具有特定标识的输入。用户发起操作后，对应的操作请求被发送给社区模块，由社区模块内部的子程序完成详细实现，包括与其他模块的交互（例如和数据治理模块的交互：请求新数据、将数据写入数据库等等），权限的变更，状态的更新等等。社区模块完成操作后，返回对应的提示信息，来提示用户操作成功或失败。

### 3.5.2 运行流程

社区模块接收来来自用户对社区系统的不同操作请求，通过内部子程序的相互协作完成操作，返回给用户对应的状态信息，其运行流程如下所示：

1. 社区模块持续监听，接收请求
2. 前端模块接收来自用户的操作，将请求发送给社区模块
3. 社区模块将请求在链表上排队
4. 检查程序对当前请求和已调度请求进行死锁检查
5. 调度进程池的空闲进程来处理请求
6. 向数据治理模块发出对应的数据请求，和其他模块进行交互
7. 根据用户权限和请求数据等信息完成操作请求
8. 返回给用户对应的状态信息

运行流程图如下：

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725714.png)

### 3.5.3 时序描述



![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725880.png)



### 3.5.4 接口描述

社区模块与登录模块、前端模块、数据治理模块进行通信交互，相互查询和交换数据信息。在社区模块内部，它具备死锁检查子程序，进程调度子程序，进程池维护子程序，交互通信子程序，业务逻辑子程序等等，它们彼此的相互合作，完成了接收来自用户的操作请求，返回给用户相应的状态信息。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725215.png)

### 3.5.5 附注

1. 社区模块即可以被用户的请求调用，也可以和其他模块通信。

## 3.6 数据治理模块

### 3.6.1 功能描述

数据治理模块是本项目中所有数据库的顶层管理者。实际上，无论是评测模块、社区模块等等都需要对数据库进行数据请求，为了避免出现越权访问、多模块同步访问冲突等严重问题，我们采取数据治理模块进行数据隔离，数据治理模块时刻对消息端口进行监听，接收来自其他模块的数据请求并存放在多个优先级队列中，然后数据治理模块根据优先级从队列中取出请求进行处理，首先进行合法性检验，对于检验通过的请求，数据治理模块异步的向对应的数据库发出查询SQL语句，之后异步的处理其他的请求，直到数据库发回对应的数据后，数据治理模块返回给其他的模块。

### 3.6.2 运行流程

数据治理模块接收来自其他模块的数据请求，返回给其他模块所请求的数据，其运行流程如下所示：

1. 数据治理模块持续监听，接收消息
2. 将接收到的来自其他模块的数据请求存放在多优先级队列中
3. 不断的根据优先级从队列中取出消息进行合法性检查
4. 对于检测合法的请求，向对应的数据库异步发出查询请求
5. 返回其他模块所请求的数据

运行流程图如下：

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725303.png)

### 3.6.3 时序描述

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725575.png)



### 3.6.4 接口描述

数据治理模块连接所有其他模块以及数据库服务器，它接收来自其他模块的请求，向数据库发出对应的请求，进而数据库将对应的数据返回给其他模块。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725582.png)

### 3.6.5 附注

1. Data Governance/Manage Module: 数据治理模块

# 4 源代码说明

## 4.1 登录模块

### 4.1.1 不同登录方式

网站支持使用重定向或二维码的方式，使用第三方账号进行登录，也支持使用微信小程序进行的登陆，不同的登录方式只需要以下面的格式调用对应的API，并在请求体中提供相应的JSON信息：

```Plain%20Text
/api/v2/login/[login Method]
```

### 4.1.2 登录状态保持

用户登录时需要调用 `POST /api/tokens` 接口，该接口的响应中会包含 `token` 字段。保持用户登录状态有以下两种方式：

1. 在 HTTP header 中添加 `token` 参数，参数的值即为调用 `/api/tokens` 接口时返回的 `token` 字段。
2. 在 Cookie 中添加 `token` 参数，参数的值即为调用 `/api/tokens` 接口时返回的 `token` 字段。

以上两种方式都能实现保持用户登录状态，若在 HTTP header 中和 Cookie 中都添加了 `token` 参数，则将以 HTTP header 中的为准。

`token` 是具有有效期的，用户在有效期内调用任意接口时，将自动续期，确保用户不需要频繁进行登录；若用户长时间未调用接口，则 `token` 可能会过期，即调用接口时返回“用户未登录”时，需要重新进行登录

## 4.2 前端模块

我们使用React框架设计前端页面，通过在html里引入存放在互联网上的 React 框架Script，完成课程界面 HTML 的设计。

## 4.3 课程模块

课程模块采用MeDu的成熟解决方案，调用其 RestFul API 完成对应功能。

## 4.4 评测模块

### 4.4.1 技术栈

评测模块的后端都是基于python代码实现，使用的技术栈如下：

- 纯python代码实现判题逻辑；
- 微框架flask作为后端；
- gunicorn+gevent提供高并发服务器的功能；
- docker作为容器打包部署。

### 4.4.2 文件组织和模块介绍

评测模块后端的源代码组织如下：

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725444.png)

其中：

- Projects下存放各个题目的文件夹，每个题目的文件夹中，包含：
  - Infrastucture，题目的基础代码，包括题目及其判题核心，以及测试用例等。
  - Submissions，用户提交代码临时存放处。
  - Description.json和Introduction.html，题目的描述和简介。
- app.py存放响应请求的视图函数。
- Dockerfile，gunicorn.conf.py以及reqirements.txt则是部署所需的配置文件。



## 4.5 社区模块

社区模块使用 Rest API, 能让任何支持HTTP请求的设备与社区的服务端进行交互，有了Rest API，我们的项目可以做到：

- 开发各种主题的前端页面
- 开发Android、IOS、Windows等客户端应用
- 在其它模块中获取社区模块服务端数据。

### 4.5.2 调用方式

Rest API 的调用方式为：Base URL + 接口名字，如：`oj.skywatch.top/api/question`

### 4.5.1 请求格式

- POST，PUT，PATCH的请求格式为 `JSON`，首部包含 `Content-Type: application/json`
- 包含文件上传的请求格式为 `from-data`，首部包含 `Content-Type: application/from-data`

### 4.5.2 响应格式

响应格式均为JSON，包含错误码（为0则成功，否则错误）、数据、和分页信息（当返回列表且需要分页时）

```JSON
{
  "code": 0,
  "data": [],
  "pagination": {
    "page": 1,
    "per_page": 15,
    "previous": null,
    "next": 2,
    "total": 124,
    "pages": 9
  }
}
```

# 5 运行测试

## 5.1 登录功能

### 5.1.1 用户注册

对于新用户，系统会要求进行用户注册。按照前端指引输入用户邮箱，并根据邮箱收到的邮件验证码来进行注册。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725169.png)

### 5.1.2 用户登录

注册完毕后，用户可以通过输入账号密码来进行登录

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725087.png)

### 5.1.3 重置密码

考虑到用户可能会忘记所设置的密码。通过点击登录页面的左下角按钮，选择“忘记密码”选项，即可进入到重置密码页面，按照注册用的邮箱来重置密码

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725048.png)



![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725048.png)

## 5.2 主页面

在主页面，包括了对项目网站的介绍，在右上角包含了对我们三个核心功能的跳转链接：

- 评测功能：Problem
- 课程系统：Course
- 社区功能：Community

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725180.png)

## 5.3 评测功能

### 5.3.1首页

在评测首页，主页的上方的导航栏包含网站的名称、评测题目以及到我们网站其他部分(课程、社区)的链接。首页下部分包含网站logo和欢迎语句。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725180.png)

导航栏一直悬停，点击网站名称可以返回首页。

### 5.3.2题目

点击导航栏上的Problem按钮进入题目列表。目前我们只设置了一道题目。点击题目的ID或Title可以查看题目详情。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725433.png)

题目详情中，包含了一段题目描述，除此之外，上方表格中指示了需要编写的文件的数量。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725593.png)

点击sumbit按钮进入提交界面。

### 5.3.3提交

提交界面包含了该题目中提供的代码，其中名称带锁形符号的python文件是不可编辑的，用户只能够编写并提交名称不带锁形符号的python文件。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725689.png)

点击Submit按钮进行提交。

### 5.3.4结果

代码评测结果将展示在提交页面的下部分。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725767.png)

## 5.4 课程功能

### 5.4.1 用户端

从主页面通过用户端登入后，来到用户端课程首页。

在用户端课程首页，我们可以看到主页的左上角是我们项目的logo，主页的右上角是用户中心、用户的消息等信息，主页的上面包含了对课程的模糊搜索功能。在主页的下面则包含了不同的课程。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725910.png)

#### 5.4.1.1 用户中心

点击用户中心，系统跳转到用户端的用户中心首页，其包括了多个不同功能的子界面。包括：

- 用户中心：显示用户的积分情况，充值情况，邀请新用户情况（邀请新用户可以获得代金券或一定的邀请余额）

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725127.png)

- 我的资料：显示用户的详细信息，包括用户名、头像、姓名、身份证号等信息。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725096.png)

- 录播课程：显示该用户的课程购买情况和购买记录，以及课程观看和收藏记录。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725354.png)

- 我的订单、我的消息：反馈了用户的订单记录、消息记录。当没有订单记录和消息记录时，系统会显示为空。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725560.png)

- 我的邀请、积分明细：为了进一步增大对项目的推广，我们提出了邀请机制。用户可以凭借自己的邀请码来邀请新用户注册系统，来获得一定的邀请金额、代金券和积分，它们可以用来在购买课程时进行打折或抵消一部分价格。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725484.png)

#### 5.4.1.2 课程中心

在主页的下面，课程按照不同的类别进行组织，用户可以选择点击不同的按钮来进行单独的显示：

- 全部：显示所有的课程信息

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725523.png)

- 单独类别：只显示属于某个单独类别的课程信息。例如在Artificial Intelligence类别下：

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725938.png)

在选择了某一个类别进行显示后，我们又提供给了用户三个子按钮，包括：

- 免费：只显示课程价格为免费的课程
- 热门：显示课程热度最高的几门课程
- 推荐：显示推荐的几门课程

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725802.png)

#### 5.4.1.3 课程主页

当用户点击其中的某一门课程，系统即会跳转到某门课详细的课程主页。

以课程<最优化理论>为例，当我们点击最优化理论课程的图标后，系统跳转到最优化理论课程的主页。由于最优化理论是付费课程，所以在主页我们可以看到系统提示我们：

- 购买课程：通过购买课程来获得该课程的观看权
- VIP会员免费观看：通过充值VIP用户，在VIP有效期内，免费观看所有课程

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725846.png)

当我们购买课程或充值VIP后，系统会显示该课程可以进行学习，同时显示该课程相关的详细信息。在课程主页的右上角，我们可以对课程进行收藏。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725860.png)

可以看到，系统通过四个子模块来显示该门课程的详细学习信息，包括：课程介绍、课程目录、课程评论和课程附件，如下所示。

- 课程介绍：对最优化理论课程的详细介绍
- 课程目录：显示该门课的所有章节信息，以及每个章节下的学习视频信息。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725274.png)

我们以其中的第一章：<凸集>来进行测试，我们点击<凸集>章节下的课程视频，即可对本章节的内容进行观看和学习。同时我们在该页面新增了视频评论子页面，实现了用户可以对每个视频进行自由的评论功能。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725120.png)

- 课程评论：显示所有用户对本门课程的评论，同时用户也可以发表自己对课程的评论

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725097.png)

- 课程附件：显示本门课程所有的附件，例如教师所提供的书籍、PPT文件、作业文档等，同时显示每个附件的大小，和对应的下载功能。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725078.png)

### 5.4.2 教师端(管理员端)

从主页面通过管理员端登入后，来到管理员端课程首页。

在管理员端的左侧功能栏中包含以下七个主要功能板块：

- 首页：跳转到AI课程首页，查看系统信息和用户信息。
- 运营：可以选择公告、VIP会员、友情链接等功能。
- 财务：可以选择订单列表、优惠码、邀请余额提现功能。
- 用户：跳转到用户页面，查看所有的用户信息。
- 点播：可以选择分类、课程功能。
- 微信公众号：可以选择消息回复、菜单功能。
- 系统：可以选择配置、管理员、管理员角色等功能。

#### 5.4.2.1 首页

在首页显示了关于用户相关的信息和系统相关的信息两个方面：

- 用户信息：包括新注册用户数、已支付订单数等等
- 系统信息：包括后台版本、内核版本等等

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725763.png)

#### 5.4.2.2 运营

在运营主页面，其包含了多个不同功能的子页面，包括：

- 公告：通过设置，发送给所有学习者用户一条公告

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725979.png)

- VIP会员：管理者模式的盈利途径之一，管理员可以设置不同类型、时限的会员。充值会员的用户可以免费观看课程

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725013.png)

- 友情链接、推广链接：管理员可以设置一些外部网站的跳转链接，比如有合作关系的广告合作商、或是相互引流的教育网站等等

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725006.png)

- 首页推荐：可以手动设置哪些课程会被展示在用户端的推荐课程页面上

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725118.png)

- 课程评论、视频评论：可以查看用户对课程、视频的评论，方便管理员对用户评论的内容是否合法进行审查

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725604.png)

- 统计工具：在该子页面我们集成了一些数据统计工具，可以统计会员购买、课程购买、每日订单数、订单总额等等的趋势曲线、数据分析等。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725607.png)

#### 5.4.2.3 财务

在财务页面，主要包含了以下三个子页面:

- 订单列表：可以查看和管理用户所有的订单记录，对于每一条订单记录，其记录了ID、用户ID、用户名、订单号、总价等信息。同时支持通过查找用户ID或订单号来搜索定位到具体的订单记录上。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725748.png)

- 优惠码：管理员可以设置用户购买课程的优惠码，以及优惠码的使用次数、抵扣金额、使用限制等等。可以在定期的活动中分发给用户

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725802.png)

- 邀请余额提现：可以查看用户邀请新用户的记录，以及对应的邀请提现余额

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725811.png)

#### 5.4.2.4 用户

在用户页面，可以管理所有的学习者注册用户。该子页面以列表的形式展示用户的全部信息，包括ID、昵称、手机号、注册时间等等，可以通过用户的昵称和手机号对用户记录进行搜索。管理员可以对用户所在的课程、用户的VIP等级进行管理，也可以对行为不规范的用户进行禁言或者封号。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725815.png)

#### 5.4.2.5 点播

在点播页面，主要分为分类和课程两个子页面。

在分类子页面，主要是对课程分类信息的相关管理。包括编辑现有的课程分类信息，新增新的课程分类信息等等。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725328.png)

在课程子页面，主要负责对所有课程进行管理。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725385.png)

在新添课程中，系统会指示管理者输入课程的名称、分类、价格、封面、简短介绍和相信介绍等信息。并将输入的记录按照具体格式保存在数据库中

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725950.png)

对于任意一门课程，我们可以对它的章节、视频、附件和观看记录四个方面进行管理。

- 章节：管理本门课程的章节信息(下图以最优化课程为例)

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725815.png)

- 视频：为不同的章节插入对应的讲解视频。对于每个视频可以单独查看单个视频的销售记录、用户观看记录等等。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725655.png)

- 课程附件：管理者可以上传与本门课相关的附件，包括对应的书籍、PPT、作业文档等。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725637.png)

- 观看记录：查看和管理用户对本门课程的观看记录

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725121.png)

#### 5.4.2.6 微信公众号

在微信公众号页面，其集成了对微信公众号的自动化设置插件，可以对绑定的微信公众号进行自动化的管理和设置，包括：

- 微信公众号消息回复：设置官方公众号的消息回复内容
- 微信公众号内容管理：设置官方公众号的主页内容展示和同步

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725162.png)

#### 5.4.2.7 系统

在系统设置页面，可以完成对系统详细的配置设置，包括对系统、支付、会员等信息详细的设置，以下面的几个配置为例：

- 系统：对系统的主页面URL链接，网站LOGO，ICP备案号等信息的设置
- 短信：对短信服务商，短信签名的设置
- 图片存储：图片的存储路径，访问链接
- 支付：支付宝支付或微信支付的对接公钥私钥

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725548.png)

## 5.5 社区功能

从主系统页面选择AI学习社区功能后，系统会跳转到社区首页。

在社区的左侧功能栏中包含以下七个主要功能板块：

- 首页：跳转到AI学习社区首页。查看系统推荐的话题、提问、博客信息。
- 话题：跳转到话题页面，查看所有的详细话题信息
- 问答：跳转到问答页面，查看所有的提问和回答信息
- 文章：跳转到文章页面，查看所有的博客文章
- 个人资料：跳转到个人资料页面
- 人脉：跳转到人脉页面，查看关注的所有博主、博客等信息
- 通知：查看所有的通知信息，包括最近点赞信息、最近关注信息，关注的博主的发文信息

### 5.5.1 首页

在首页包含了系统推荐的话题，最近更新提问，最近热门提问，最新文章，最近热门文章等推荐信息。用户可以通过点击对应的文章或提问来进行查看。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725446.png)

### 5.5.2 话题

跳转到话题页面后，主要包含两个子板块，如下：

- 已关注：只显示该用户所关注的话题

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725723.png)

- 精选：显示距今为止所有的话题

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725580.png)

我们点击其中的任一话题，系统会显示出该话题下所有的提问和文章信息。对选择的提问或文章进行点击即可查看该提问或文章的信息。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725857.png)

### 5.5.3 问答

跳转到问答页面后，主要包含三子板块，如下：

- 最新：显示最新的提问
- 近期热门：显示近期最热门的提问（基于热度计算子程序实现）
- 已关注：显示该用户已经关注的提问

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725167.png)

我们点击其中的任一提问，系统会显示出该提问的详细信息，包括标题，具体内容，所在话题，发布时间等信息。初次之外，我们还提供了多种功能，如下：

- 关注提问：将提问加入到自己的关注列表
- 查看评论：查看所有的评论信息
- 额外功能：包括复制链接、分享提问、举报提问、查看提问的所有关注者

其他用户或发起提问的用户可以在该提问下进行回答，进而形成良好的知识分享和知识解答AI学习社区。其他用户可以对该提问的评论进行点赞或点踩，系统会根据点赞量对热门评论进行置顶显示。同时对评论也实现了额外功能，包括举报评论等。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725317.png)

点击提问或评论右侧的额外功能按钮，系统会显示出具体的额外功能。包括如下：

- 复制链接：复制本提问的链接到剪切板
- 分享：将本提问分享到微信、QQ、微博等主要社交媒体
- 举报：举报该提问，系统审核员将对该举报进行人工审查
- 查看本提问的所有关注者：帮助用户递归的进行学习

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725954.png)

下面展示额外功能中的举报功能，点击举报后，系统会弹出不同的举报理由，用户选择详细的举报理由进行举报后，系统审核员将对该举报进行人工审查。此功能是为了维护良好的社区学习环境，避免大量非学习、引战内容的渗入。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725687.png)

 此外，用户可以点击右下角的发起提问按钮来发起提问。在新提问页面，用户输入提问的标题、所属的话题、提问的详细内容，即可发表新提问，显示在提问首页中。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725646.png)

### 5.5.4 文章

跳转到文章页面后，主要包含三子板块，如下：

- 最新：显示最新发布的文章
- 近期热门：显示近期最热门的文章（基于热度计算子程序实现）
- 已关注：显示该用户已经关注的文章

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725899.png)

我们点击其中的任一文章，系统会显示出该文章的详细信息，包括标题，具体内容，所涉及话题，发布时间等信息。初次之外，我们还提供了多种功能，包括对文章点赞、对文章点踩、收藏文章、分享文章、举报文章等。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725170.png)

文章支持多种格式的发布，包括任意插入图片，调整字体的颜色、大小、加粗等细节。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725587.png)

此外，用户可以点击右下角的发文章按钮来发起新文章。在新文章页面，用户输入文章的标题、所属的话题、文章的详细内容，即可发表新文章，显示在文章首页中。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725526.png)

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725497.png)

另外用户可以在上方搜索栏根据标题模糊搜索文章或问答。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725769.png)

### 5.5.5 个人资料

跳转到个人资料页面后，将会详细的显示个人信息，包括：

- 用户个人信息，包括昵称、学校、联系方式等详细信息
- 用户发表的所有提问信息
- 用户评论过的所有回答信息
- 用户发表过的所有文章信息

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725920.png)

### 5.5.6 人脉

跳转到人脉页面后，主要包含三子板块，如下：

- 已关注：显示所有该用户关注的用户信息

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725933.png)

- 关注者：显示关注该用户关注的用户信息

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725265.png)

- 找人：根据兴趣、关注方向等多维因素，推荐给用户具有类似兴趣的其他用户

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725442.png)

### 5.5.7 通知

跳转到通知页面后，将会详细的显示所有的个人通知。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/lab15/202206031725406.png)

### 5.6 用于测试的账户

1. 课程 ：

   用户端：账号 18855556666，密码 qwer123456

   后台：账号 meedu@meedu.meedu，密码 meedu123，网址 http://course.skywatch.top/admin/#/

2. 社区： 账号dayday，密码dayday