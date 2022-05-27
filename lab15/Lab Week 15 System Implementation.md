# Lab Week 15: System Implementation

https://github.com/EstherBear/SoftwareEngineering

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

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=YjhhYjAyYmFlZDRkNmQ4ZTg5ZWU0ZDEyNDAzZmNlYjFfamJHUHBJN1hrcnBiVWZkRE1nTnRaUnpES1Q3MjB1NDdfVG9rZW46Ym94Y25rV1dUWHpURk9YS0ZyVkpkV2J1MjJwXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

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

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=NjcxMDdlOTA4NTM1ZjM2NzhmMDI1MTE2YjZkZDJiZmZfWUN5OU9uVnl6RlVJbHpoazdsUmhVVlVCbW8waWJsU01fVG9rZW46Ym94Y25MY2dQc0U2eHNnTmpHaXBMR2pQR3JnXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 3.1.3 时序描述

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=OTc4NTQzYzc2YTUwNTA5OGI0NDJmNTE2YTgxMjMwZmJfMGpPem8wbG1hc3ppTFhiV3BqcDgyclFSTTY5cDI2MnVfVG9rZW46Ym94Y25UWjQwaDJPTng3TVgyZ0M0TXVkdEVnXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=OWZjMTFmZjQwZDNmYTRiZGE0YjYyMDBhN2JkODliYTVfYUlqVnh5STVhVWZXTlhYb0EyZTBVV2txc0hSaHdIaXpfVG9rZW46Ym94Y25laHdRSU1yUEZFTEw5OVppSGtEbmFmXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 3.1.4 接口描述

登录模块连接前端以及数据治理模块，它接收来自前端的请求，简单的逻辑处理后，向数据治理模块发出对应的查询请求，得到结果后再经过简单的逻辑处理，把结果信息返回给前端。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=YmQ2N2VkZDg2YmQ1MmY5OWMyMzE5MDJhZDdkMDU2ZDdfaVJ2QjdDWWtKQVBFQjF6MU9VY3Q3blR5Sk1FY2VUN1JfVG9rZW46Ym94Y25Qdm5pWTZBWGN5MXBRWTNtNWRFbFFPXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

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

TODO

### 3.2.3 时序描述

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=MmY0Mzg1YmViOTg1NDQ0NTMxNTkwMDQxZWQyZTIxZmVfUjdTZFNobm5tTGRBc2hhSHNBczJLR3FOYnpGRlpmODhfVG9rZW46Ym94Y25Zclk3MEUyZEdkMUoyeFZKcnluTU43XzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)



### 3.2.4 接口描述

前端界面模块连接用户以及除数据治理模块的其他所有模块，它接收来自用户的请求，向除数据治理模块的其他模块发出对应的请求，并接收返回结果再展示给用户。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=MzRiM2QzNDM2ZjRiNzY2NTFmZDk1NmE2OTAzNGVkMWZfVFhFTGJDVFFIdWIwUXFaS2NKV2FPNjI0b3BUSTQxcVVfVG9rZW46Ym94Y25Ybmd0aXhuOWVVdmhHc1NMQ2hzQ0xjXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

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

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=MDE2YzQ5ZTZmNTlhMzY0ZmQwZjlmM2VlZjZlZjNkYzFfOGdEU0xnOFZRclp3MTlUZ2VkOWtncWlPVnZDQWdLdFJfVG9rZW46Ym94Y254VlltM2gxYVVYdXJUSURlZ3BlaFVkXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 3.3.3 时序描述

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=ODE0Y2I1ZDU1MmEyNTllMmZjN2QwNWYxYjUzMzhlOTVfTG1FN2Y0WDlNcFJhUkFkQ20xTUhiT0FSbEZ6TTZ2TUlfVG9rZW46Ym94Y24zZkxDNU1TbzI0ZFN5b2Z4OGVjSDZlXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)



### 3.3.4 接口描述

课程模块与前端模块、数据治理模块进行通信交互，相互查询和交换数据信息。此外，课程模块具备死锁检查子程序，进程调度子程序，进程池维护子程序，交互通信子程序，业务逻辑子程序等等，它们彼此的相互合作，完成了接收来自用户的操作请求，返回给用户相应的状态信息。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=YmViY2EwNWE4ZDhkZjY3YjE4NTIzYWI1NjY5Y2RmNDVfOG5tVWhMejhUY0Z6dlppRGFkU21QUnc0aEVQZ3htSFFfVG9rZW46Ym94Y25iVWQzVDRyYlkwblh1b3d6MHBmVmFoXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

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

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDZmOThkYmI2ZTgxMDYyNmFjNjQ3ZDc2MjhhMjRhZWJfOXZmTUVtOHVwUWpOSHh3SjlZOWdYQ21xYWxKWThzbndfVG9rZW46Ym94Y243QUZvSWMzTGgyRHNmN040TnBubXJnXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 3.4.3 时序描述

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=ZGJhOWIyZWYwYTgwNzdlNmUwNTllNjQ3MjY4MmFmOTNfRDI2Ymc1MmxzcDRtV055ZWFJTUZEVjBxV0VBaFhmSW9fVG9rZW46Ym94Y244c0ltNmo1Z051WFNSNk1uQUhBVWdmXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)



### 3.4.4 接口描述

评测模块接收来自前端的请求，简单的逻辑处理后，调用评测节点完成评测结果。根据评测结果，向数据治理模块发出对应的修改用户数据的请求。最后把评测分数返回给前端，并把游戏的动作序列发送给游戏引擎。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=MmU4MTI4NjRmNjZhM2RjYzk2NjcxZWYzZGQxMzZkNWJfdGFzdkdHTFZiTnVUTzdEY2JCa3lVYmxGMkIxTzFUNEJfVG9rZW46Ym94Y25lMUd5SmVzMUdIN0JSYm00eGlremxoXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

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

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=MmRhZDg4NmU5YzM1OTY5MzYzNGVhNjdlYzYzMjQ5OTRfN3ZQSUtBbkV1RG12b01sdXQxNUtMNHhqSjNTNlI4M1hfVG9rZW46Ym94Y253c0hqVFJQRVRTQ2lRVDJhZlVHalhiXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 3.5.3 时序描述



![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2M3MGI0NDM0NWNhZDA4NWI5ODMxMTBjMjk2NGFhNDhfVlVBand4VWJLN0ViTjVQMEVZZXA3aG80OExiR0pCY2lfVG9rZW46Ym94Y25Uc09pYm5PTmRZMXlTc3h6SFB6U0tmXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)



### 3.5.4 接口描述

社区模块与登录模块、前端模块、数据治理模块进行通信交互，相互查询和交换数据信息。在社区模块内部，它具备死锁检查子程序，进程调度子程序，进程池维护子程序，交互通信子程序，业务逻辑子程序等等，它们彼此的相互合作，完成了接收来自用户的操作请求，返回给用户相应的状态信息。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=N2FjYjUwOTViMWQyYWUyMzc5ZWMzN2JhZjE4N2U4NGRfNXp0UURST2dPaDlpSUY1VURBcU9Lc0k5dXR4ZG1BeXVfVG9rZW46Ym94Y25QcDh1bHlOdFhpME9aQTMxWHppTW0xXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

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

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=YTI5NzEzZGM1ZDg4ZTcxYTNmZDk0YjVlM2JhNmZhNjBfbmRhcnBLalExRTF1MkVhSEJ3MUNTTzdwb05tZVdRZGZfVG9rZW46Ym94Y25DWWxTNHVyNEdreXd2RGtSNzFKeURmXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 3.6.3 时序描述

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=MmNlM2RjMzViMWRmNDE3MDFlZDU5N2FmYmMzODU2ODlfeUN0aFkybnBTcnF5TGptcTJjWjhCbkdWQ3U0RVlZcGhfVG9rZW46Ym94Y25ycjZJNG1mZEl4MDhaTG9GZ2loS3doXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)



### 3.6.4 接口描述

数据治理模块连接所有其他模块以及数据库服务器，它接收来自其他模块的请求，向数据库发出对应的请求，进而数据库将对应的数据返回给其他模块。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=NTI3NjA3ODMxZmQzMjcwODNkYzVkODMzOTkxZjRlYmNfekNzazI0M2FJdU9udzlVWDVEaUdNVm44UVNVU09EUkpfVG9rZW46Ym94Y25aMHBzQ1d0U1lLQ3F4bFcxSElXVGliXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 3.6.5 附注

1. Data Governance/Manage Module: 数据治理模块

# 4 源代码说明

todo

# 5 运行测试

## 5.1 登录功能

### 5.1.1 用户注册

对于新用户，系统会要求进行用户注册。按照前端指引输入用户邮箱，并根据邮箱收到的邮件验证码来进行注册。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=ODdlMTg4M2Q4NzVjODJmODVmM2Q2MDE2YjI2NzI5YWFfSFZPQzQwbE51djg4d2VqTW53VGlxRUY2bURWeWd3ZGxfVG9rZW46Ym94Y25SRTBGZU9VOU1XbDNQSTVTbmgyY3ZlXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 5.1.2 用户登录

注册完毕后，用户可以通过输入账号密码来进行登录

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=Yzg2YTJjYmE2ZGVkNDdlMmQ4NTBiNGIzNmJhZWI5MmJfcXRsTnA2eXlrVmZCeHo5T2RQUTJ5TWxiSGJnWHZSTEdfVG9rZW46Ym94Y25sMlcwQUtYbTBIQ1FsbTlsWkY5dVJoXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 5.1.3 重置密码

考虑到用户可能会忘记所设置的密码。通过点击登录页面的左下角按钮，选择“忘记密码”选项，即可进入到重置密码页面，按照注册用的邮箱来重置密码

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=MDg0M2I4ZDRiYTlhN2ViNzIwODY2YWZhZmU2M2Y1NjNfM3NPV3p4RlNBSUdDVUlQU1lWMUNOVzNBaTBwSjQyRnRfVG9rZW46Ym94Y256REdGTUlZQzJ1cHQ0T3NnNld4czljXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

## 5.2 评测功能

todo

## 5.3 课程功能

从主系统页面选择AI课程功能后，系统会跳转到课程首页。

在课程的左侧功能栏中包含以下七个主要功能板块：

- 首页：跳转到AI课程首页，查看系统信息和用户信息。
- 运营：可以选择公告、VIP会员、友情链接等功能。
- 财务：可以选择订单列表、优惠码、邀请余额提现功能。
- 用户：跳转到用户页面，查看所有的用户信息。
- 点播：可以选择分类、课程功能。
- 微信公众号：可以选择消息回复、菜单功能。
- 系统：可以选择配置、管理员、管理员角色等功能。

todo

## 5.4 社区功能

从主系统页面选择AI学习社区功能后，系统会跳转到社区首页。

在社区的左侧功能栏中包含以下七个主要功能板块：

- 首页：跳转到AI学习社区首页。查看系统推荐的话题、提问、博客信息。
- 话题：跳转到话题页面，查看所有的详细话题信息
- 问答：跳转到问答页面，查看所有的提问和回答信息
- 文章：跳转到文章页面，查看所有的博客文章
- 个人资料：跳转到个人资料页面
- 人脉：跳转到人脉页面，查看关注的所有博主、博客等信息
- 通知：查看所有的通知信息，包括最近点赞信息、最近关注信息，关注的博主的发文信息

### 5.4.1 首页

在首页包含了系统推荐的话题，最近更新提问，最近热门提问，最新文章，最近热门文章等推荐信息。用户可以通过点击对应的文章或提问来进行查看。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=N2U0OTM5ZTZhNDBkZDkzZDUyMjc5YTU1NmEwNzI1YjlfWjBISnNRSlpMVHRJZlBFaURJUU95ZTBpUkdKNTYyMlJfVG9rZW46Ym94Y25wcXRKTHdhUEJ2ZmJDNExMTGpMcE9oXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 5.4.2 话题

跳转到话题页面后，主要包含两个子板块，如下：

- 已关注：只显示该用户所关注的话题

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=YWYyMWQyYWUxMzU5NWMwM2IxMDA3OGNjYmI5NmY2NDFfd0RYSllzMTFIR3pZYm5TVlQ3OER5RkpURzBmRmJyYU9fVG9rZW46Ym94Y25IZ2hXczBSZ2VTZlYzbGYwdFRVUVNWXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

- 精选：显示距今为止所有的话题

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=M2E5YmNjNWE4ODI1MGMxOWM0NTU4MzA3YjU5MzExY2FfMUlVS2JobHdPakNDNm40aG9kcEkxUndVUWRQbWRIcW1fVG9rZW46Ym94Y25YU2xlZ1BIYUFudXJrd1FhTzhCT1BmXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

我们点击其中的任一话题，系统会显示出该话题下所有的提问和文章信息。对选择的提问或文章进行点击即可查看该提问或文章的信息。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDdkOGU5YjFmMmMxZDFmMTE5MzhjZWM4OWQzZjlhYzlfY2VhWEV0M3ZuQnZCcWhTdTJBd202YzNWTUtKd1dYbzNfVG9rZW46Ym94Y25BTmZqUTBwcHA0clgzY0ZsWHBaVUV1XzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 5.4.3 问答

跳转到问答页面后，主要包含三子板块，如下：

- 最新：显示最新的提问
- 近期热门：显示近期最热门的提问（基于热度计算子程序实现）
- 已关注：显示该用户已经关注的提问

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=Yzk2MzQ4ZTA5MWZiN2FjNjcyZDcxYjYzMDhkNTE5YzlfYWNsdFhtRXJwalFEeGM4eFk5RjRBQm44a1RjQ3NQVUtfVG9rZW46Ym94Y242SUs1azF6TEhxNkQxSnlKdVpTNWVmXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

我们点击其中的任一提问，系统会显示出该提问的详细信息，包括标题，具体内容，所在话题，发布时间等信息。初次之外，我们还提供了多种功能，如下：

- 关注提问：将提问加入到自己的关注列表
- 查看评论：查看所有的评论信息
- 额外功能：包括复制链接、分享提问、举报提问、查看提问的所有关注者

其他用户或发起提问的用户可以在该提问下进行回答，进而形成良好的知识分享和知识解答AI学习社区。其他用户可以对该提问的评论进行点赞或点踩，系统会根据点赞量对热门评论进行置顶显示。同时对评论也实现了额外功能，包括举报评论等。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=ZWRhODRhNDBmOWFjNTkzNGFmMzExYTA3NmZmYjZmOTlfYkFhdlJJN1paOXNXQVIyWGhFVWV4azNYQThMMHdYSUdfVG9rZW46Ym94Y25WVlkxQUNMYXdvbWF6RkU2bEZQSFNmXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

点击提问或评论右侧的额外功能按钮，系统会显示出具体的额外功能。包括如下：

- 复制链接：复制本提问的链接到剪切板
- 分享：将本提问分享到微信、QQ、微博等主要社交媒体
- 举报：举报该提问，系统审核员将对该举报进行人工审查
- 查看本提问的所有关注者：帮助用户递归的进行学习

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=Zjk3MDZjOGExMjMyMTNlNDc4OWQwOGU4ZjkyNzBmZjZfN3VvNjU4cjBMY1ZsaldNc0tITmJIZ1FsTW5XVTF5THdfVG9rZW46Ym94Y244bG83cXE2dkxGQUMwMVpTZEx4ZWdkXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

下面展示额外功能中的举报功能，点击举报后，系统会弹出不同的举报理由，用户选择详细的举报理由进行举报后，系统审核员将对该举报进行人工审查。此功能是为了维护良好的社区学习环境，避免大量非学习、引战内容的渗入。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDc1NzQ4OWM3ODE1NjVmOWY1Y2IwYzA5NjgyZTI0M2FfeGtvYndQQ3RORVJYSFFlY0hqRkVSOWR0YnN5a05GY0ZfVG9rZW46Ym94Y25HbUN6R3o2TGh4RDk4Y21BdW1RNTBzXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

 此外，用户可以点击右下角的发起提问按钮来发起提问。在新提问页面，用户输入提问的标题、所属的话题、提问的详细内容，即可发表新提问，显示在提问首页中。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=YWFkNGFjNGM1ZWMyYTIzMGFhY2U3NTUwZDEyNmY2MzVfRDBWNGY3NkUwSm15NHJZSFphN0ZoeGdsN3ZWa1ZVYldfVG9rZW46Ym94Y25LVnNwcEZKT25IQzV6ZFE3STJEMjhlXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 5.4.4 文章

跳转到文章页面后，主要包含三子板块，如下：

- 最新：显示最新发布的文章
- 近期热门：显示近期最热门的文章（基于热度计算子程序实现）
- 已关注：显示该用户已经关注的文章

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=MTQ4MzQyNTk4MGUzOGQ2MTMxNTcxZmU1YjE3MTgwNzNfRE93S3c0dXN6cE9DR044TjlxbTdXSFl5SDlrN3ZKbERfVG9rZW46Ym94Y24xMUw0QWFEUU5ldTl6NlRWaHdodXhlXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

我们点击其中的任一文章，系统会显示出该文章的详细信息，包括标题，具体内容，所涉及话题，发布时间等信息。初次之外，我们还提供了多种功能，包括对文章点赞、对文章点踩、收藏文章、分享文章、举报文章等。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=MTQ3ZDBlZDY2MDFiZTM5MDRmYjdlMDM1NWY5ZGRkMzVfQzJWcmRCV1prT280NEczV3hUc09MMGNwZVhHcWphUXBfVG9rZW46Ym94Y25uOFdaZ0NFQVJXeHliRjJiUG9QQjZnXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

文章支持多种格式的发布，包括任意插入图片，调整字体的颜色、大小、加粗等细节。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=MDRlNmQ0OGE3NjU1N2I1NGM4ZTg4YTMyNjIyNzM2ZjNfaTh5eVFsVWpZcjBlTWdNeEI1UkxsNXJlbDhCN094T1VfVG9rZW46Ym94Y245Vk9JQUJ4aTJ0eUoweGlBNjRlTkFmXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

此外，用户可以点击右下角的发文章按钮来发起新文章。在新文章页面，用户输入文章的标题、所属的话题、文章的详细内容，即可发表新文章，显示在文章首页中。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=NGUyNGEzMTQ3MTdhMjJlYTI2ZDQzN2RkNjVmYjI1MjBfM2RNQjZxZnIweGxVa1ZhYmZlZWtBVEd3R2d0VkpSY3dfVG9rZW46Ym94Y25JYThCRTVFQnFqbGU1cEFBTm13VFJMXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=MDAxMDY3NDJjYThjMzcyM2YzNDk3NGRmODZlYzhiNzNfZkxBNTJJUERTRUpxZXlSR2ZjMmRnaVFsUFh5UGRxZlJfVG9rZW46Ym94Y25Qc1psM25lakVwRjJoUTVNNFpYOUNkXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

另外用户可以在上方搜索栏根据标题模糊搜索文章或问答。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2VkYTNhOTYxODY5NTA3YWE3NjQyNDk5ZDM5Zjg1YWZfcUluNGNOZDZEWlhLZXhBbUtEb3lmZVg4ZVhZOTNLSHJfVG9rZW46Ym94Y25GNVFvNHRnUGtKSzlKTWEydnJ5ME5iXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 5.4.5 个人资料

跳转到个人资料页面后，将会详细的显示个人信息，包括：

- 用户个人信息，包括昵称、学校、联系方式等详细信息
- 用户发表的所有提问信息
- 用户评论过的所有回答信息
- 用户发表过的所有文章信息

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=ZmU4ZjgwY2JhZjgyZjEwNWUzY2E1MDJhNTViYzM0ODlfdUlaTU1IWXA0SHMzMGlvdHVIMGVpTW03bm1yZnJSVW1fVG9rZW46Ym94Y25RSW9tQklKNXdxc1RMNE9BSVk2RnViXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 5.4.6 人脉

跳转到人脉页面后，主要包含三子板块，如下：

- 已关注：显示所有该用户关注的用户信息

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=ZWJmMzI1YWZmMmU0MDVlNTU0YzFhNGJiMjgwOGUyZjlfR1lNUzhGZW56R0JkT0Y0V1cwMG9ZZ01NdVE4UUI3ZkRfVG9rZW46Ym94Y25pcWpyZHdWUGVFRklOelAzbHZMbWxlXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

- 关注者：显示关注该用户关注的用户信息

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=MGJmZTRjNjg5YWJkMjk2NmE1ZjFkMTZjNzE1OGYyZjVfbW9YU2J3VDNJckZFS1NiNTZWd3pQSVpWVzVEWllZTUZfVG9rZW46Ym94Y25UcDJGM1h5ZUpmTTdjeDFJaWpPWWlkXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

- 找人：根据兴趣、关注方向等多维因素，推荐给用户具有类似兴趣的其他用户

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=OGI4ZWYyMTNkNTE5NzlkNjc5YTZjMjRkM2VkOGYzYWNfWGFnZ2NKbVdMQklpa002ZXZja1V1M0RtUEpNSkhKTE9fVG9rZW46Ym94Y253TFVNNUxncWR3eVFMc3FMbjJzN29mXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)

### 5.4.7 通知

跳转到通知页面后，将会详细的显示所有的个人通知。

![img](https://a9ehh4q3wr.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjI0MGIwZWMzNzJkZGQ0NDg3MjEyZGMyNTU3NDA4ZWRfb2NYcllGY0NHUDB2ZHF3ZVlPVll3b1BjbWE5dmFHcndfVG9rZW46Ym94Y25ISW1lcXl5OVNBb1Q2a285eGk5ZGplXzE2NTM2NDQ5NzM6MTY1MzY0ODU3M19WNA)