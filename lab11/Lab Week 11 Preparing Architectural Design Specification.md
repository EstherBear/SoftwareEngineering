# Lab Week 11: Preparing Architectural Design Specification

https://github.com/EstherBear/SoftwareEngineering

# 0 人员介绍

| 姓名                 | 学号     |      | 姓名   | 学号     |
| -------------------- | -------- | ---- | ------ | -------- |
| 劳倩茹（版本管理员） | 18340086 |      | 吴天   | 19334019 |
| 刘思然               | 19335141 |      | 赖奕恺 | 19335093 |

# 1 引言

## 1.1 编写目的

> 说明编写这份概要设计说明书的目的 指出预期的读者

本文档的目的是详细地介绍人工智能(AI)教学在线评测与游戏网站的概要设计，说明对程序系统的设计考虑，包括程序系统的基本处理流程，程序系统的组织结构、模块划分、功能分配、接口设计、运行设计、数据结构设计和出错处理设计等，为程序的详细设计提供基础。本文将结合文字描述，概念图、组件图，以及关系图等来描述对网站系统的总体设计，接口设计，运行设计，系统数据结构设计，以及系统出错处理设计。本文档的预期读者是开发人员和维护人员，以及跟该项目相关的其他人员。

## 1.2 背景

> 说明:

1. > 待开发软件系统的名称: 

人工智能(AI)教学在线评测与游戏网站。

1. > 列出此项目的任务提出者 开发者 用户以及将运行该软件的计算站 中心

| 任务提出者           | 蔡老师               |
| -------------------- | -------------------- |
| 开发者               | 本小组成员           |
| 用户                 | 学生、教师和开发人员 |
| 将运行该软件的计算站 | 阿里云服务器         |

## 1.3 定义

> | 术语、简写和缩略语 | 解释                                                         |
> | ------------------ | ------------------------------------------------------------ |
> | Html               | 超文本标记语言，是一种用于创建网页的标准标记语言             |
> | CSS                | 层叠样式表(英文全称：Cascading Style Sheets)是一种用来表现HTML文件样式的计算机语言 |
> | JavaScript         | JavaScript（简称“JS”） 是一种具有函数优先的轻量级，解释型或即时编译型的编程语言 |
> | React              | React是用于构建用户界面的JavaScript库                        |
> | HTTP               | HyperText Transfer Protocol，超文本传输协议                  |
> | WebSocket          | WebSocket是一种在单个TCP连接上进行全双工通信的协议           |
> | DDS                | Data Distribution Service for Real-Time Systems，是一种物联网协议 |
> | Flask              | Flask是具备的扩展性和兼容性的web开发框架                     |
> | SqLite             | SqLite是一个完全独立的、不要任何配置、支持SQL的、开源的文件数据库引擎 |
> | Vakuum             | 一个采用MVC（模型-视图-控制器）架构设计的判题系统            |

## 1.4 参考资料

- GB/T 8567-1988 计算机软件产品开发文件编制指南

# 2 总体设计

## 2.1 需求规定

### 2.1.1 性能需求

作为一体化的人工智能(AI)教学在线评测与游戏网站，为了保证学习者、教学者和开发者的用户体验，性能需求是必须得到保障的，主要的性能规定包括以下三个方面：

- 每个用户最多支持三个终端登录，超过数量应按登录时间依次弹出之前的登录。
- 在带宽和用户负荷方面，我们初期估计用户数为 10000 人，每天登录用户数为3000人左右，网络的带宽为100M。服务器应能够同时负荷预期熟练的人数同时运行。
- 在用户响应时间方面，在非高峰时间运行用户代码，应该能在 3 秒内得到运行结果，高峰时段不超过 5 秒。其他如查询学习资料等功能，在 2 秒内得到查询结果，高峰时段不超过 3 秒。

### 2.1.2 输入输出需求

我们定义用户的数据输入或是用户对系统的操作均视为对项目系统的输入，定义项目系统针对用户的输入进行的反馈动作为系统的输出。当用户对项目系统进行输入时，前端应能根据用户所采取操作的类型，反馈给后端或其他独立组件如数据库、判题器等等，在后端根据用户的操作类型和输入信息，后端采取相应的动作，并反馈给前端，进而反馈给用户。以部分输入输出场景为例，如下所示：

- 当用户注册账号时，用户输入手机号、用户名等个人信息后，前端应将输入反馈给数据库，由数据库进行表的添加和权限的赋予，并反馈唯一、未使用过的用户id给用户，提示注册成功等信息
- 当用户请求查看课程资料时，前端应将消息发送给后端，由文件服务器通过HTML协议传输，以图片、文件、视频或者压缩包等格式反馈给用户进行查看。
- 当教学者对考试进行批改时，前端应将消息发送给后端，后端应及时的将数据写入文件，并实时的反馈给教学者显示已批改的情况。

### 2.1.3 灵活性需求

当本项目发生了更新或维护，该项目应该在灵活性方面加以保证，使得用户、管理人员和开发人员能够快速适用项目的变化，主要包括以下方面的规定：

- 操作方式：用户前端的主体操作方式、管理人员前端的管理操作页面、开发人员内部的操作接口不应发生较大的变迁，应保持与过去相同风格、相同模式，尽可能保证只增不删
- 运行环境：不管发生怎样的更新和维护，都应保证用户、管理人员和开发人员的运行环境不发生改变
- 其他软件接口：当项目中所使用的其他软件接口发生变化，该项目能够快速定位到变迁接口的位置，并替换为最新兼容版本的接口。

### 2.1.4 数据管理能力需求

不同身份的角色拥有着不同的权限，这要求特定身份的角色只能根据自己所具备的权限对自己可访问的数据进行访问、更新或删除，每个角色都不可以越权对数据进行管理，也不可以对其他角色所管理的数据进行访问和更新。以部分数据管理需求场景为例，如下所示：

- 学习者不可以对自己未加入的课程的数据进行越权访问，包括对未加入课程的资料查看、作业提交等。但可以对自己已加入的课程的资料进行查看、作业提交、加入考试等等。
- 教学者不可以对未加入课程的学习者进行管理，包括对未加入课程的学习者进行发起作业、发起考试等。但可以对已加入课程的学习者进行成绩批改，考试批改等等
- 用户不可以对他人所发表的用户博客进行修改，但可以对他人所发表的用户博客进行评论，以及对自己所发表的用户博客进行修改。

### 2.1.5 故障处理需求

当用户使用本项目发生系统故障时，该项目应该能够迅速定位到错误发生的问题和类型，明确提示错误信息，指导用户如何从系统故障中恢复，如若无法恢复，该项目应该能够快速自我定位和修复，并提示用户刷新或重新登入。以部分常见故障类型和故障处理为例，如下所示：

- 当物理服务器发生硬件故障，应当紧急关闭项目网站，提示所有在线用户下线通知，保存用户数据。同时系统监控程序应能发现故障发生，传递消息给开发人员，开发人员进行硬件维修后，做好数据恢复工作，并提示用户维护完成。
- 当软件接口发生故障时，以评测系统评测软件进程崩溃为例。监控程序应迅速定位发生故障所在，提示所有用户发生故障，等待重启，并自动重启服务端进程，在重启进程恢复服务后，应提示用户进行浏览器刷新
- 当用户操作发生异常时，以学习者搜索不存在的课程为例。当对数据库访问发现不存在该搜索课程时，应该由前端提示给用户该课程不存在，提示用户重新搜索。

## 2.2 运行环境

### 2.2.1 服务端运行环境

我们采用了如下配置的服务器作为我们的项目搭建服务器，通过系统所支持的指令集作为基础进行优化，例如采取并行向量化AVX-512编程作为软件加速，同时V100-SXM2型号GPU能够提供高可靠的强大算力，满足了我们高并发、低延迟的用户代码评测的需求，而Intel的最新Xeon Gold 6132型CPU满足了我们对搭建大量数据库、文件服务器和Web页面的支撑；最后我们整体的硬件框架都基于市场主流硬件，在软件层次上几乎支持所有的常见主流协议。

- CPU：Intel Xeon Gold 6132，14 cores @ 2.60GHz
- GPU：NVIDIA V100-SXM2 16GB
- RAM：256GB
- Hard disk: 480G SSD SATA
- HCA card：InfiniBand Mellanox ConnectX-6 HDR card
- 接口类型：Serial ATA
- 接口速率：Serial ATA 300 
- 电源类型：后备式 UPS 
- 操作系统：Centos 7
- 指令集：MMX/SSE/SSE2/SSE3/SSE4.1/SSE4.2/SSSE3/Sup-SSE3/EM64T/AVX/AVX2/AVX-512/IMCI

### 2.2.2 客户端运行环境

而对于用户来说，无需特别限定硬件配置，主流笔记本电脑配置以及主流浏览器即可完成对本项目的访问、课程的学习和代码的编写。下面我们给出最低电脑配置作为示例：

- CPU：Intel Core(TM) 2 Duo CPU E4600 @ 2.40GHZ
- GPU：512MB显存及以上
- RAM：2GB
- Hard disk: 5GB及以上(空闲)
- 操作系统：Window XP、 Windows 7/8/9/10、Windows 2003 Server、Vista等
- 浏览器：Microsoft Edge、Chrome、QQ浏览器、火狐浏览器等

## 2.3 基本设计概念和处理流程

本系统的功能设计概念图如下所示。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/2022-04-29-094808.png)

在基本设计上，本系统由采取了3-iter的设计模式，分别指表示层UI、业务逻辑层BLL和持久化层DAL。通过这样的设计模式可以实现高内聚和低耦合的特点，让不同的功能模块相互分离，彼此相互协作工作，使得结构清晰明了，功能独立性强，分工明确，层次清晰，进而使得整个系统的可扩展性强，这提高了整个业务系统的开发质量和开发效率，为日后的更新和维护都提供了帮助。

在表示层，本系统主要设计为展示给用户的前端页面以及接收用户输入的功能模块。在具体实现中，表示层由五大前端页面组成，配合输入输出进程和消息传递机制完成各种复杂的逻辑操作。在每个前端页面中，都包含和嵌套了不同页面的相互跳转UML链接，同时页面上的不同按钮表征了本系统的不同功能，用户根据表示层前端页面的设计和指引进行操作，发出对应的操作命令，通过消息传递机制传递给业务逻辑层，由业务逻辑层的不同模块，如：判题器、数据库、监控程序或文件服务器等等根据用户的操作做出不同的动作，将结果传递回表示层进行显示。主要的前端页面包括：

- 主页页面：网站主页包括题目列表，成就展示
  - 题目列表优先展示学习者被教师分配的任务，按降序展示的其他热门且未完成的题目次之，再次，展示最新题目
  - 成就展示展示学习者题目完成进度和学习者的平均得分和总得分排名。
- 评测页面：提供题目展示区、代码编辑与提交区和结果展示区
  - 题目展示区列举题目资料，为节省展示空间制作为边栏形式，学习者依需求点击相关控件展开显示。
  - 代码编辑与提交区提供代码编辑框和提交按钮，可在基本编辑功能需求满足的情况之上添加代码高亮功能和代码智能感知自动补全功能。
  - 结果展示区分为直观展示区和只读Console，以PNG或GIF格式显示代码运行结果，并在只读Console展示代码标准字符输出和标准错误输出。
- 开发者页面：提供文档浏览子页面、SDK下载链接和题目提交界面。
  - 文档浏览子页面应为开发者提供易于浏览的文档浏览界面
  - SDK下载链接利用CDN为用户提供高速下载服务
  - 题目提交界面包括：代码上传区域、审核进度展示、定价选择栏、承诺书勾选栏和提交按钮。
- 教师页面：提供班级管理区域和完成情况视图
  - 用户群组管理区：使教师可以添加用户群组和管理用户群组成员
  - 题目发布区：使教师能够在平台的题目库里选择题目分配给拥有的用户群组。
  - 完成情况视图可以展示指定用户群组，按已发布题目或学习者ID筛选并展示学习者ID和完成情况
- 论坛页面
  - 文章浏览：论坛页面的每条文章以题目名字、时间段、代码得分区间为标签，用户可使用这些标签筛选出符合需求的文章，并添加点赞和点踩按钮帮助筛选出精选评论。
  - 文章发布：需提供文字编辑框、题目选择栏、标签选择下拉列表和发布按钮。

在业务逻辑层，本系统的所有功能、业务逻辑都在本层完成接口和底层细节的实现。由表现层的功能逻辑对业务逻辑层的具体接口进行调用，进而在业务逻辑层，不同的功能模块根据用户在表现层的功能请求做出对应的动作，完成具体的业务逻辑，将结果反馈给表现层。在本项目的系统中，业务逻辑层可以按照用途划分为三类不同的组件，不同的组件中又包含了多种不同的模块，如下所示：

- 服务组件：练习模块、教学模块、博客模块、开发者模块、竞赛模块、管理模块。
- 功能组件：评测系统模块、游戏执行引擎、博客系统、数据展示模块、匹配系统、通知模块、用户管理模块
- 基础组件：安全验证模块、错误处理模块、HTTP服务器

在持久化层提供了对数据库和文件服务器的存取接口，业务逻辑层可以直接通过接口访问所需的数据。在实现上，我们通过对数据库系统和文件服务器的设计，实现了对用户全部数据的存储和分类，通过设计权限机制使得不同的身份的角色只能访问特定的数据，不同的用户不能访问对方的私有数据，进而使得整个数据逻辑和管理变得井然有序，项目的业务逻辑实现清晰、简明。

## 2.4 结构

本系统的所有运行模块包括：

- 展示层：游戏音像渲染引擎、Web渲染引擎
- 业务逻辑层：
  - 服务组件：练习模块、教学模块、博客模块、开发者模块、竞赛模块、管理模块。
  - 功能组件：评测系统模块、游戏执行引擎、博客系统、数据展示模块、匹配系统、通知模块、用户管理模块
  - 基础组件：安全验证模块、错误处理模块、HTTP服务器
- 持久化层：
  - 持久化模块：提供对数据库的存取接口。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/2022-04-29-094808.png)

以下框图展示了本系统的表示层UI、业务逻辑层BLL和持久化层DAL的划分设计。在表现层主要由主页页面、评测页面、开发者页面、教师页面和论坛页面五大前端页面以及输入输出逻辑组成，通过对业务逻辑层的接口进行调用，进而实现底层业务逻辑功能的实现；而在业务逻辑层，又包含了服务组件、功能组件和基础组件三大类用途组件组成，而每一类组件中又包含了许多不同的功能模块；在持久化层，提供了对数据库和文件服务器的存取接口，业务逻辑层可以直接通过接口访问所需的数据。

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/2022-04-29-094809.png)

## 2.5 功能需求与程序的关系

### 2.5.1 功能需求与服务模块

|          | 练习模块 | 教学模块 | 博客模块 | 开发者模块 | 竞赛模块 | 成就模块 | 管理模块 |
| -------- | -------- | -------- | -------- | ---------- | -------- | -------- | -------- |
| 登录注册 |          |          |          |            |          |          | √        |
| 课程管理 | √        | √        |          |            |          |          | √        |
| 提交作业 | √        | √        |          |            |          |          | √        |
| 课程考试 |          |          |          |            | √        |          | √        |
| 知识博客 |          |          | √        |            |          |          | √        |
| 游戏评测 |          |          |          |            |          | √        | √        |
| 成就系统 |          |          |          |            |          | √        | √        |
| 竞赛功能 |          |          |          |            | √        |          | √        |
| 授课功能 |          | √        |          |            |          |          | √        |
| 开发功能 |          |          |          | √          |          |          | √        |
| 盈利功能 | √        | √        | √        | √          | √        | √        | √        |

### 2.5.2 功能需求与功能组件

|          | 评测系统模块 | 游戏执行引擎 | 博客系统 | 数据展示模块 | 匹配系统 | 通知模块 | 用户管理模块 |
| -------- | ------------ | ------------ | -------- | ------------ | -------- | -------- | ------------ |
| 登录注册 |              |              |          |              |          | √        | √            |
| 课程管理 | √            | √            |          | √            |          | √        | √            |
| 提交作业 |              |              |          |              |          | √        | √            |
| 课程考试 | √            | √            |          | √            |          | √        | √            |
| 知识博客 |              |              | √        |              |          |          | √            |
| 游戏评测 | √            | √            |          |              |          | √        | √            |
| 成就系统 |              |              |          |              |          | √        | √            |
| 竞赛功能 | √            | √            |          |              | √        |          | √            |
| 授课功能 |              |              |          | √            |          | √        | √            |
| 开发功能 | √            | √            |          |              |          |          | √            |
| 盈利功能 |              |              |          |              |          |          | √            |

### 2.5.3 功能需求与基础组件

|          | 安全验证模块 | 错误处理模块 | HTTP服务器 |
| -------- | ------------ | ------------ | ---------- |
| 登录注册 | √            | √            | √          |
| 课程管理 | √            | √            | √          |
| 提交作业 | √            | √            | √          |
| 课程考试 | √            | √            | √          |
| 知识博客 | √            | √            | √          |
| 游戏评测 | √            | √            | √          |
| 成就系统 | √            | √            | √          |
| 竞赛功能 | √            | √            | √          |
| 授课功能 | √            | √            | √          |
| 开发功能 | √            | √            | √          |
| 盈利功能 | √            | √            | √          |

## 2.6 尚未解决的问题

- 用户之间的竞赛匹配机制暂时未成功以高效、多并发的方式实现

# 3 接口设计

## 3.1 用户接口

### 3.1.1首页

**登录注册界面**：当访问项目主页时，进入用户登录界面，该界面包含用户名输入栏、密码输入栏，包含登录按钮。

**参数**：用户名输入、密码输入

| 参数名   | 类型   | 是否必填 | 参数描述 | 最大长度 |
| -------- | ------ | -------- | -------- | -------- |
| user_id  | int    | yes      | 用户名   | 12       |
| password | string | yes      | 密码     | 32       |

**响应**：登录成功 或者 登录失败

| 参数名 | 类型 | 是否必填 | 参数描述                           |
| ------ | ---- | -------- | ---------------------------------- |
| state  | bool | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | bool | 必填     | 登录结果，0表示失败，1表示请求成功 |

### 3.1.2学生界面

#### 3.1.2.1学生界面

  学生用户登录后进入学生界面，该界面包含三个按钮，分别可进入学生的三个主要功能：课程学习、知识博客、学习教学。

**参数**：用户可点击的三个按钮

| 参数名 | 类型 | 是否必填 | 参数描述                   | 最大长度 |
| ------ | ---- | -------- | -------------------------- | -------- |
| choice | int  | yes      | 按钮id，0~2分别代表3个按钮 | 1        |

**响应**：按钮对应的网页

| 参数名 | 类型   | 是否必填 | 参数描述                           |
| ------ | ------ | -------- | ---------------------------------- |
| state  | bool   | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | string | 必填     | 即将弹出的网址url                  |

#### 3.1.2.2课程学习

##### **3.1.2.2.1课程管理**

学习者可以根据课程ID加入到由特定教学者创建的课程中进行学习；也可以通过自由搜索、选择其他教学者用户发布的网课视频进行自由学习。此外，学习者可以管理自己加入的课程列表，包括对课程进行删除、评分等功能。

**参数**：课程ID，加入课程按钮

| 参数名   | 类型 | 是否必填 | 参数描述                   | 最大长度 |
| -------- | ---- | -------- | -------------------------- | -------- |
| class_id | int  | yes      | 课程id，代表特定的课程     | 8        |
| user_id  | int  | yes      | 用户id，请求加入课程的用户 | 12       |

**响应**：加入成功 或 加入失败

| 参数名 | 类型 | 是否必填 | 参数描述                           |
| ------ | ---- | -------- | ---------------------------------- |
| state  | bool | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | bool | 必填     | 加入结果，0表示失败，1表示请求成功 |

**参数**：网课视频

| 参数名    | 类型 | 是否必填 | 参数描述 | 最大长度 |
| --------- | ---- | -------- | -------- | -------- |
| choice_id | int  | yes      | 网课id   | 8        |

**响应**：对应网课所在网页

| 参数名 | 类型   | 是否必填 | 参数描述                           |
| ------ | ------ | -------- | ---------------------------------- |
| state  | bool   | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | string | 必填     | 即将弹出的网址url                  |

**参数**：选择课程，操作(删除、评分)

| 参数名   | 类型 | 是否必填 | 参数描述               | 最大长度 |
| -------- | ---- | -------- | ---------------------- | -------- |
| class_id | int  | yes      | 课程id，代表特定的课程 | 8        |
| action   | int  | yes      | 要进行的操作           | 1        |

**响应**：操作成功 或 操作失败

| 参数名 | 类型 | 是否必填 | 参数描述                           |
| ------ | ---- | -------- | ---------------------------------- |
| state  | bool | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | bool | 必填     | 操作结果，0表示失败，1表示请求成功 |

##### **3.1.2.2.2提交作业**

学习者在加入课程后，可以根据教学者的要求提交作业，并得到来自教学者的批改反馈。

**参数**：课程作业、作业内容(用户上传)

**响应**：提交成功 或 提交失败

| 参数名 | 类型 | 是否必填 | 参数描述                           |
| ------ | ---- | -------- | ---------------------------------- |
| state  | bool | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | bool | 必填     | 操作结果，0表示失败，1表示请求成功 |

**参数**：已提交的作业

| 参数名        | 类型 | 是否必填 | 参数描述 | 最大长度 |
| ------------- | ---- | -------- | -------- | -------- |
| assignment_id | int  | yes      | 作业id   | 8        |

**响应**：作业评分以及老师评语

| 参数名  | 类型   | 是否必填 | 参数描述                           |
| ------- | ------ | -------- | ---------------------------------- |
| state   | bool   | 必填     | 请求结果，0表示失败，1表示请求成功 |
| score   | char   | 必填     | 评分，0~100                        |
| comment | string | 可选     | 评语                               |

##### 3.1.2.2.2课程考试

学习者可以根据教学者的要求，参加教学者建立的考试进行水平检测。

**参数**：考试ID

| 参数名  | 类型 | 是否必填 | 参数描述 | 最大长度 |
| ------- | ---- | -------- | -------- | -------- |
| exam_id | int  | yes      | 考试id   | 8        |

**响应**：考试ID对应页面

| 参数名 | 类型   | 是否必填 | 参数描述                           |
| ------ | ------ | -------- | ---------------------------------- |
| state  | bool   | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | string | 必填     | 即将弹出的网址url                  |

**参数**：已结束的考试

| 参数名  | 类型 | 是否必填 | 参数描述 | 最大长度 |
| ------- | ---- | -------- | -------- | -------- |
| exam_id | int  | yes      | 考试id   | 8        |

**响应**：考试得分和考试排名

| 参数名 | 类型 | 是否必填 | 参数描述                           |
| ------ | ---- | -------- | ---------------------------------- |
| state  | bool | 必填     | 请求结果，0表示失败，1表示请求成功 |
| score  | char | 必填     | 评分，0~100                        |
| rank   | int  | 必填     | 排名                               |

#### 3.1.2.3知识博客

用户可以自由发表知识博客，进行知识分享和交流；也可以任意观看博客，并对博客进行留言评论。

**参数**：博客标题、博客内容

**响应**：发布成功 或者 发布失败

| 参数名 | 类型 | 是否必填 | 参数描述                           |
| ------ | ---- | -------- | ---------------------------------- |
| state  | bool | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | bool | 必填     | 发布结果，0表示失败，1表示请求成功 |

**参数**：选择博客

| 参数名  | 类型 | 是否必填 | 参数描述 | 最大长度 |
| ------- | ---- | -------- | -------- | -------- |
| blog_id | int  | yes      | 博客id   | 8        |

**响应**：已选择博客对应的页面

| 参数名 | 类型   | 是否必填 | 参数描述                           |
| ------ | ------ | -------- | ---------------------------------- |
| state  | bool   | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | string | 必填     | 即将弹出的网址url                  |

**参数**：用户评论

| 参数名  | 类型  | 是否必填 | 参数描述 | 最大长度 |
| ------- | ----- | -------- | -------- | -------- |
| blog_id | int   | yes      | 博客id   | 8        |
| comment | sting | yes      | 评论     | 500      |

**响应**：评论成功 或者 评论失败

| 参数名 | 类型 | 是否必填 | 参数描述                           |
| ------ | ---- | -------- | ---------------------------------- |
| state  | bool | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | bool | 必填     | 评论结果，0表示失败，1表示请求成功 |

**参数**：选择已发布的博客，操作(删除、隐藏)

| 参数名  | 类型 | 是否必填 | 参数描述 | 最大长度        |
| ------- | ---- | -------- | -------- | --------------- |
| blog_id | int  | yes      | 博客id   | 8               |
| action  | int  | yes      | 操作     | 0~k表示不同操作 |

**响应**：操作成功 或 操作失败

| 参数名 | 类型 | 是否必填 | 参数描述                           |
| ------ | ---- | -------- | ---------------------------------- |
| state  | bool | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | bool | 必填     | 操作结果，0表示失败，1表示请求成功 |

#### 3.1.2.4游戏教学

##### 3.1.2.4.1游戏评测

用户可以根据所学进行实践，编写操作游戏人物的核心代码文件或者脚本文件，或是提交训练的关键代码，项目能够实时对代码进行评测，并反馈展示给用户提交代码的游戏实时效果。

**参数**：用户代码

| 参数名 | 类型 | 是否必填 | 参数描述 | 最大长度 |
| ------ | ---- | -------- | -------- | -------- |
| code   | File | yes      | 用户代码 | 100kb    |

**响应**：用户提交程序运行效果

| 参数名 | 类型 | 是否必填 | 参数描述                           |
| ------ | ---- | -------- | ---------------------------------- |
| state  | bool | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | ——   | 必填     | 程序运行效果                       |

##### 3.1.2.4.2成就系统

不同的评测游戏建立了Rank天梯机制、成就获取和收集机制，用户可以查看自己获得的成就。

**参数**：游戏ID

| 参数名  | 类型 | 是否必填 | 参数描述 | 最大长度 |
| ------- | ---- | -------- | -------- | -------- |
| game_id | int  | yes      | 游戏id   | 8        |

**响应**：用户排名、用户成就

| 参数名      | 类型 | 是否必填 | 参数描述                           |
| ----------- | ---- | -------- | ---------------------------------- |
| state       | bool | 必填     | 请求结果，0表示失败，1表示请求成功 |
| achievement | File | 可选     | 成就                               |
| rank        | int  | 必填     | 排名                               |

##### 3.1.2.4.3竞赛界面

用户可以在比赛前期报名，在比赛中编写和提交代码，比赛后得到成绩排名。

**参数**：选择比赛、报名比赛

| 参数名         | 类型 | 是否必填 | 参数描述 | 最大长度 |
| -------------- | ---- | -------- | -------- | -------- |
| competition_id | int  | yes      | 比赛id   | 8        |

**响应**：报名成功 或者 报名失败

| 参数名 | 类型 | 是否必填 | 参数描述                           |
| ------ | ---- | -------- | ---------------------------------- |
| state  | bool | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | bool | 必填     | 报名结果，0表示失败，1表示请求成功 |

**参数**：用户代码

| 参数名 | 类型 | 是否必填 | 参数描述 | 最大长度 |
| ------ | ---- | -------- | -------- | -------- |
| code   | File | yes      | 用户代码 | 100kb    |

**响应**：提交成功 或者 提交失败

| 参数名 | 类型 | 是否必填 | 参数描述                           |
| ------ | ---- | -------- | ---------------------------------- |
| state  | bool | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | bool | 必填     | 提交结果，0表示失败，1表示请求成功 |

**参数**：比赛ID

| 参数名         | 类型 | 是否必填 | 参数描述 | 最大长度 |
| -------------- | ---- | -------- | -------- | -------- |
| competition_id | int  | yes      | 比赛id   | 8        |

**响应**：用户排名、比赛成绩

| 参数名 | 类型 | 是否必填 | 参数描述                           |
| ------ | ---- | -------- | ---------------------------------- |
| state  | bool | 必填     | 请求结果，0表示失败，1表示请求成功 |
| score  | char | 必填     | 评分，0~100                        |
| rank   | int  | 必填     | 排名                               |

### 3.1.3教师界面

课程教师的基本需求是授课功能，发布学习任务、追踪学生课程完成情况。另外，课程教师对课程版权的要求较高，存在课程权限控制的需求，为鼓励共享精神以促进平台发展，平台对课程权限控制功能收取合理的维护费用。

#### 3.1.3.1授课界面

课程教师可以建立学习者群组，将课程资源（题目以及配套资料）推送至学习者或学习者群组。

**参数**：课程资源（题目以及配套资料）、提交方式（私有或者公开）

| 参数名   | 类型 | 是否必填 | 参数描述 | 最大长度 |
| -------- | ---- | -------- | -------- | -------- |
| material | File | yes      | 课程资料 | 5M       |
| method   | bool | yes      | 提交方式 | 1        |

**响应**：支付接口、推送成功 或者 推送失败

| 参数名 | 类型   | 是否必填 | 参数描述                           |
| ------ | ------ | -------- | ---------------------------------- |
| state  | bool   | 必填     | 请求结果，0表示失败，1表示请求成功 |
| result | bool   | 必填     | 推送结果，0表示失败，1表示请求成功 |
| pay    | string | 必填     | 支付链接                           |

### 3.1.4自由开发者界面

本平台为自由开发者提供了再就业的灵活渠道，自由开发者能够在平台上传和应用个人项目，同时具有合理的盈利模式为其开发带来收益。

#### 3.1.4.1开发提交

自由开发者需要在本地通过我们提供的SDK进行题目的开发和测试。在完成开发后可以发起上线提交请求，经人工或系统检查通过后即可部署上线。

**参数**：开发项目源码、项目描述

| 参数名   | 类型 | 是否必填 | 参数描述 | 最大长度 |
| -------- | ---- | -------- | -------- | -------- |
| code     | File | yes      | 代码     | 100kb    |
| describe | File | yes      | 描述     | 5M       |

**响应**：提交审核状态(待审核、审核成功、审核失败)

| 参数名 | 类型 | 是否必填 | 参数描述                                    |
| ------ | ---- | -------- | ------------------------------------------- |
| state  | bool | 必填     | 请求结果，0表示失败，1表示请求成功          |
| result | int  | 必填     | 审核状态，0~1分别表示请求失败、成功、审核中 |

## 3.2 外部接口

- 软硬件之间接口： 使用 Html+CSS+js，数据库直接建在总的服务器上，通过json将数据库信息发送到AI教学平台前端。用户通过浏览器可以在这个 Web 上进行应用程序的操作，前端也会以json将数据发送到后端进行处理。 
- 本系统与外部的支付模块：本项目调用支付宝的支付接口。支付宝是应用最为广泛的互联网支付方式之一，引入支付宝支付，能够便利用户使用网站的付费功能。

电脑端调用支付宝，在页面有一个立即支付的按钮，点击进入商户的后台，商户的后台将支付所需的参数传给支付宝，支付宝返回给商户一个字符串形式的form表单，商户将这个form表单传给前台，前台对表单进行提交即可跳转到支付宝页面，用户在支付宝页面支付完成后，支付宝会先调取我们的通知接口进行支付结果通知。然后跳转到我们传给支付包的回调页面。

**公共请求参数**

Unable to copy while content loads

**请求参数**

Unable to copy while content loads

**公共响应参数**

Unable to copy while content loads

**响应参数**

参数类型是否必填最大长度描述示例值trade_noString必填64支付宝交易号2013112011001004330000121536out_trade_noString必填64商户订单号6823789339978248seller_idString必填28收款支付宝账号对应的支付宝唯一用户号。
以2088开头的纯16位数字2088111111116894total_amountPrice必填11交易金额128merchant_order_noString必填32商户原始订单号，最大长度限制32位20161008001

## 3.3 内部接口

该项目的系统元素主要可分为四大类：评测系统、教学系统、博客、安全验证。评测系统、教学系统、博客这三个大类之间一般不进行内部通信，各个大类内部的系统元素联系较为紧密，除此之外，各类从用户接口获取的参数都将经由安全验证，再进行处理。

### 3.3.1安全验证

安全验证模块首先对发送请求的用户身份进行核查，查看是否出现越权访问的情况。

安全验证模块还会对用户发送的参数进行合法性检查，如是否包含恶意脚本代码，若用户输入不合法，将拒绝请求；若用户输入合法，将用户输入发送到对应的程序接口进行处理。

在参数合法的情况下，安全验证模块的输入参数和输出完全相同。

### 3.3.2评测接口

- 竞赛和游戏评测功能需要调用评测接口，服务器调用评测引擎和游戏执行引擎对游戏进行评测，若正确则调用Web渲染引擎对游戏结果进行渲染，若错误则返回运行结果，最后将提交记录保存在数据库中。
- 结果的查询：根据用户的不同请求参数，评测模块则从持久化模块中取出相应得分或排名数据并返回给HTTP服务器。

### 3.3.3教学接口

- 教师资料上传：接收资料上传请求，向支付宝发起支付请求，获取支付链接，将支付链接返回HTTP浏览器，检测用户的支付状态，若用户成功支付，调用持久化模块将课程资料写入数据库，并返回上传结果给HTTP服务器。
- 学生查看资料：根据用户的不同请求参数，教学模块则从持久化模块中取出相应结构化数据并返回给HTTP服务器。
- 教师创建、删除课程，学生加入、退出课程：根据用户的不同请求参数，教学模块通过持久化模块修改数据库的相应数据，并将操作结果返回给HTTP服务器。
- 教师布置作业：教学模块接收布置作业请求，调用持久化模块将其写入数据库，并返回提交结果给HTTP服务器。
- 学生提交作业：教学模块接收作业提交请求，调用持久化模块将其写入数据库，并返回提交结果给HTTP服务器。
- 查看作业完成情况：在收到查看请求时，教学模块从持久化模块中取出相应的数据，对已提交作业、教师评分等进行统计分析，并返回统计结果给HTTP服务器。

### 3.3.4博客接口

- 用户发布博文：博客模块接收博文提交请求，对该博文进行自动审核和分类等操作，并调用持久化模块将其写入数据库，并返回提交结果给HTTP服务器。
- 用户发表评论：博客模块接收评论提交请求，对评论进行自动审核，查看评论是否合法合规，并调用持久化模块将其写入数据库，并返回提交结果给HTTP服务器。
- 用户查看博文：根据用户的不同请求参数，博客模块则从持久化模块中取出相应结构化数据并返回给HTTP服务器。
- 用户删除博文：博客模块通过持久化模块删除相应结构化数据，并返回操作结果给HTTP服务器。

# 4 运行设计

## 4.1 运行模块组合

> 说明对系统施加不同的外界运行控制时所引起的各种不同的运行模块组合 说明每种运行所历经的内部模块和支持软件

### 4.1.1 学生运行控制下的运行模块组合

学生运行控制包括练习场景、竞赛场景和博客场景下的运行控制。使用到的运行模块包括

- 展示层：游戏音像渲染引擎、Web渲染引擎
- 业务逻辑层：
  - 服务组件：练习模块、博客模块、竞赛模块
  - 功能组件：评测系统模块、游戏执行引擎、博客系统、匹配系统。
  - 基础组件：安全验证模块、错误处理模块、HTTP服务器
- 持久化层：
  - 持久化模块：提供对数据库的存取接口。

### 4.1.2 教师运行控制下的运行模块组合

- 展示层：游戏音像渲染引擎、Web渲染引擎
- 业务逻辑层：
  - 服务组件：教学模块、博客模块。
  - 功能组件：博客系统、数据展示模块、通知模块、用户管理模块。
  - 基础组件：安全验证模块、错误处理模块、HTTP服务器
- 持久化层：
  - 持久化模块：提供对数据库的存取接口。

### 4.1.3 开发者运行控制下的运行模块组合

- 展示层：Web渲染引擎
- 业务逻辑层：
  - 服务组件：开发者模块
  - 功能组件：博客系统、数据展示模块、通知模块
  - 基础组件：安全验证模块、错误处理模块、HTTP服务器
- 持久化层：
  - 持久化模块：提供对数据库的存取接口。

### 4.1.4 管理员运行控制下的运行模块组合

- 展示层：Web渲染引擎
- 业务逻辑层：
  - 服务组件：练习模块、教学模块、博客模块、开发者模块、竞赛模块、管理模块。
  - 功能组件：评测系统模块、游戏执行引擎、博客系统、数据展示模块、匹配系统、通知模块、用户管理模块
  - 基础组件：安全验证模块、错误处理模块、HTTP服务器
- 持久化层：
  - 持久化模块：提供对数据库的存取接口。

## 4.2 运行控制

> 说明每一种外界的运行控制的方式方法和操作步骤

所有用户与系统发生交互都需要通过HTTP服务器；所有界面展示都通过位于浏览器的Web渲染引擎统一渲染；登陆系统都会调用用户管理模块和安全验证模块进行登陆验证；发生故障时，由错误处理模块设置容错等级、判断错误类型并调用错误处理例程。

所有外部运行控制都会使用到基础组件中的安全验证模块、错误处理模块和HTTP服务器，故下文不再赘述。

### 4.2.1 学生运行控制

- 练习场景：练习模块为学生提供练习题目的测评功能，并为学生以丰富的方式展示运行结果。
  - 提交代码：服务器调用评测引擎和游戏执行引擎对游戏进行评测，若正确则调用Web渲染引擎对游戏结果进行渲染，若错误则返回运行结果，最后将提交记录保存在数据库中。
- 竞赛场景：竞赛模块调用匹配系统对当前正在匹配系统内排队的学生进行匹配，组建房间并限定时间，在竞赛结束后通过HTTP服务器发送竞赛结果并将结果写入持久化层。
- 博客场景：
  - 用户发布博文：博客模块接收HTTP服务器传递的博文提交请求，对该博文进行自动审核和分类等操作，并调用持久化模块将其写入数据库。
  - 用户查看博文：HTTP服务器根据用户的不同请求参数，向博客模块转发相应的请求；博客模块则从持久化模块中取出相应结构化数据并返回给HTTP服务器。
  - 所有用户都设计博客场景，下文不再赘述。

### 4.2.2 教师运行控制

- 管理题目或班级：服务器再次调用安全验证模块对操作者进行验证，验证成功后对数据库进行存取，并对涉及的学生经通知模块发送信件和站内信，并将结果写入持久化层。
- 查看题目完成率和学生完成率：通过教学模块在数据库中取出数据，并调用数据展示模块生成统计数据。

### 4.2.4 开发者运行控制

- 查看API文档：由开发者模块从持久化模块中取出相应的API文档经由HTTP服务器发送给客户浏览器。
- 提交题目：由开发者模块调用安全验证模块对代码是否有注入式攻击进行自动检测，并调用持久化模块写入持久化层。
- 查看审核进度：由开发者模块从持久化模块中取出相应数据的审核进度经由HTTP服务器发送给客户浏览器。

### 4.2.6 管理员运行控制

- 审核题目：
  - 拉取：管理模块调用存储模块从数据库中加载待审核题目队列；
  - 通过/拒绝：管理模块调用通知模块向开发者发送站内信并通过开发者预留的邮箱地址发送邮件，并调用持久化模块从待审核题目表删除待审核题目，对审核通过的题目持久化。
- 删除用户、评论等：
  - 通过博客模块、用户管理模块等模块预留的管理接口，对违反规定的评论和用户进行删除和封禁。

## 4.3 运行时间

> 说明每种运行模块组合将占用各种资源的时间

- 学生运行控制：学生数量最多，由于竞赛和代码评测的需要，对CPU和GPU时间的消耗较大，同时会产生比较大的内存开销。
- 教师运行控制：教师运行控制发起请求次数少且轻量，对各类资源的时间消耗都小。
- 开发者运行控制：开发者发起提交请求的数据极少，提交的题目是所有运行控制中数据量最大的，提交时对网络资源和硬盘通道的占用较高。
- 管理员运行控制：管理员站用户数量最少，且其任务几乎对所有资源都不产生任何开销。



# 5 系统数据结构设计

## 5.1 逻辑结构设计要点

> 给出本系统内所使用的每个数据结构的名称 标识符以及它们之中每个数据项 记录 文卷和系的标识 定义 长度及它们之间的层次的或表格的相互关系

下面给出本软件系统所使用的各个数据库的记录类型以及字段，它们之间的相互关系如下：

![img](https://typora-1304243409.cos.ap-guangzhou.myqcloud.com/SoftwareEngineering/2022-04-29-094811.png)

### 5.1.1 用户信息数据库

用户信息数据库用来存储每个用户的最基本身份信息，包括其个人资料、账号密码信息等等，用来帮助系统对其进行身份识别、账号登入、权限修改等等。在用户数据库中，每条记录维护了一个用户的个人信息，其中每种个人信息设定为字符串类型，每条记录设定为由多个字符串组合的复合结构体类型。

具体的，数据库的每条记录组成字段如下：

| 字段说明 | 类型   | 长度 | 字段说明   | 类型   | 长度 |
| -------- | ------ | ---- | ---------- | ------ | ---- |
| 账号信息 | String | 50   | 用户昵称   | String | 50   |
| 密码信息 | String | 50   | 用户邮箱   | String | 50   |
| 用户手机 | String | 50   | 实名认证   | String | 50   |
| 账户余额 | String | 50   | 学校       | String | 50   |
| 用户类型 | String | 20   | 学号(工号) | String | 20   |
| 关联项目 | Ptr    | 8    |            |        |      |

### 5.1.2 课程数据库

课程数据库维护了每个用户所加入或创建的课程信息，并对关键的数据进行了保存和维护。每条记录代表一个用户所关联的课程信息。每条记录由复合结构体类型组成。

具体的，数据库的每条记录组成字段如下：

| 字段说明   | 类型         | 长度 | 字段说明 | 类型 | 长度 |
| ---------- | ------------ | ---- | -------- | ---- | ---- |
| 课程列表   | List         | 50   | 课程考试 | Int  | 4    |
| 课程编号   | Int          | 4    |          |      |      |
| 课程创建者 | String       | 50   |          |      |      |
| 课程加入者 | List<String> | 200  |          |      |      |
| 课程文件   | List<Ptr>    | 500  |          |      |      |
| 课程作业   | List<Ptr>    | 500  |          |      |      |

### 5.1.3 考试数据库

考试数据库维护了考试功能所关联的全部数据信息。每条记录代表一场考试的信息，由复合结构体类型组成。

具体的，数据库的每条记录组成字段如下：

| 字段说明 | 类型         | 长度     | 字段说明     | 类型         | 长度 |
| -------- | ------------ | -------- | ------------ | ------------ | ---- |
| 考试编号 | Int          | 4        | 考试排名     | List<String> | 200  |
| 考生名单 | List<String> | 200      | 考试次数     | Int          | 4    |
| 考生学号 | List<String> | 200      | 是否公布结果 | bool         | 1    |
| 考试题目 | File         | 文件长度 |              |              |      |
| 考试答案 | File         | 文件长度 |              |              |      |
| 考试结果 | File         | 文件长度 |              |              |      |

### 5.1.4 游戏数据库

游戏数据库记录了所有游戏的相关数据，以及包括各个参与用户的历史代码、游戏排名等等。每条记录代表一个游戏的信息。每条记录由复合结构体类型组成。

具体的，数据库的每条记录组成字段如下：

| 字段说明 | 类型      | 长度 | 字段说明 | 类型         | 长度 |
| -------- | --------- | ---- | -------- | ------------ | ---- |
| 游戏编号 | Int       | 4    | 排名信息 | List<String> | 1000 |
| 游戏名称 | String    | 50   | 成就信息 | Ptr          | 8    |
| 游戏文件 | Ptr       | 8    | 用户     | List<String> | 1000 |
| 历史代码 | List<Ptr> | 500  |          |              |      |
| 进度说明 | List<int> | 500  |          |              |      |

### 5.1.5 成就数据库

成就数据库记录了每个用户在项目学习过程中所获得的成就信息。每条记录代表一个用户所完成、未完成的全部成就系统所涉及的数据。每条记录由复合结构体类型组成。

具体的，数据库的每条记录组成字段如下：

| 字段说明 | 类型      | 长度 | 字段说明 | 类型 | 长度 |
| -------- | --------- | ---- | -------- | ---- | ---- |
| 成就列表 | List<int> | 500  | 成就数量 | Int  | 4    |
| 完成成就 | Bool      | 1    |          |      |      |
| 成就描述 | Ptr       | 8    |          |      |      |
| 成就动画 | List<Ptr> | 500  |          |      |      |
| 排名信息 | Int       | 4    |          |      |      |

### 5.1.6 竞赛数据库

竞赛数据库记录了所有竞赛所涉及的用户信息、排名信息等等。每条记录代表一场竞赛从竞赛报名、竞赛途中到竞赛结束、结果公开所产生的全部数据信息。每条记录由复合结构体类型组成。

具体的，数据库的每条记录组成字段如下：

| 字段说明 | 类型         | 长度     | 字段说明 | 类型 | 长度     |
| -------- | ------------ | -------- | -------- | ---- | -------- |
| 竞赛编号 | Int          | 4        | 竞赛题目 | File | 文件长度 |
| 报名列表 | List<User>   | 200      | 评测程序 | Ptr  | 8        |
| 筛选程序 | Ptr          | 8        | 竞赛文件 | File | 文件长度 |
| 竞赛结果 | File         | 文件长度 |          |      |          |
| 排名信息 | List<String> | 200      |          |      |          |

### 5.1.7 博客数据库

博客数据库记录了用户发布的每条博客的相关信息。每条记录代表一篇博客的所有相关数据。每条记录由复合结构体类型组成。

具体的，数据库的每条记录组成字段如下：

| 字段说明     | 类型   | 长度     | 字段说明 | 类型       | 长度 |
| ------------ | ------ | -------- | -------- | ---------- | ---- |
| 博客编号     | int    | 4        | 发布时间 | Time       | 8    |
| 博客文件     | File   | 文件长度 | 标签     | List<Enum> | 50   |
| 创作者       | String | 50       |          |            |      |
| 博客评论     | File   | 文件长度 |          |            |      |
| 博客统计信息 | Dict   | 100      |          |            |      |

### 5.1.8 项目数据库

项目数据库记录了每个开发者所提交、开发的项目信息。每条记录代表着一个项目所涉及的全部信息。每条记录由复合结构体类型组成。

具体的，数据库的每条记录组成字段如下：

| 字段说明 | 类型   | 长度     | 字段说明 | 类型 | 长度 |
| -------- | ------ | -------- | -------- | ---- | ---- |
| 项目编号 | int    | 4        | 项目代码 | Ptr  | 8    |
| 盈利信息 | Ptr    | 8        | 审核人员 | Int  | 4    |
| 项目描述 | Ptr    | 8        | 审核程序 | Ptr  | 8    |
| 开发日志 | File   | 文件长度 |          |      |      |
| 开发者   | String | 50       |          |      |      |

### 5.1.9 交易数据库

交易数据库记录了用户和平台每次的交易，每条交易均可为用户与平台的交易、用户与开发者之间的交易、平台与开发者之间的交易。每条记录维护了单条交易信息，其类型为复合结构体类型。

具体的，数据库的每条记录组成字段如下：

| 字段说明 | 类型   | 长度 | 字段说明 | 类型   | 长度 |
| -------- | ------ | ---- | -------- | ------ | ---- |
| 交易号   | Int    | 4    | 交易类型 | String | 20   |
| 付款方   | String | 50   | 支付渠道 | String | 20   |
| 收款方   | String | 50   | 交易金额 | Float  | 4    |
| 交易内容 | Ptr    | 8    |          |        |      |
| 交易时间 | Time   | 8    |          |        |      |

## 5.2 物理结构设计要点

> 给出本系统内所使用的每个数据结构中的每个数据项的存储要求 访问方法 存取单位 存取的物 理关系 索引 设备 存储区域 设计考虑和保密条件

下面给出每个数据库的物理结构设计要点。

|                | 存储要求     | 访问方法 | 存取单位 | 索引       | 设备     | 保密级别 |
| -------------- | ------------ | -------- | -------- | ---------- | -------- | -------- |
| 用户信息数据库 | 关系型数据库 | sqlite3  | 记录     | 聚集索引   | 云服务器 | 强       |
| 课程数据库     | 关系型数据库 | sqlite3  | 记录     | 聚集索引   | 云服务器 | 强       |
| 考试数据库     | 文档型数据库 | sqlite3  | 记录     | 聚集索引   | 云服务器 | 强       |
| 游戏数据库     | 关系型数据库 | sqlite3  | 记录     | 聚集索引   | 云服务器 | 强       |
| 成就数据库     | 关系型数据库 | sqlite3  | 记录     | 无         | 云服务器 | 弱       |
| 竞赛数据库     | 文档型数据库 | sqlite3  | 记录     | 非聚集索引 | 云服务器 | 弱       |
| 博客数据库     | 文档型数据库 | sqlite3  | 记录     | 非聚集索引 | 云服务器 | 强       |
| 项目数据库     | 文档型数据库 | sqlite3  | 记录     | 非聚集索引 | 云服务器 | 强       |
| 交易数据库     | 关系型数据库 | sqlite3  | 记录     | 非聚集索引 | 云服务器 | 强       |

## 5.3 数据结构与程序的关系

> 说明各个数据结构与访问这些数据结构的各个程序之间的对应关系 可采用如下的矩阵图的形式

下面给出各个数据结构与访问这些数据结构的各个程序之间的对应关系。

|                | 课程模块 | 博客模块 | 游戏模块 | 教学模块 | 盈利模块 | 开发模块 |
| -------------- | -------- | -------- | -------- | -------- | -------- | -------- |
| 用户信息数据库 | √        | √        | √        | √        | √        | √        |
| 课程数据库     | √        |          |          | √        |          |          |
| 考试数据库     | √        |          |          |          |          |          |
| 游戏数据库     |          |          | √        |          |          |          |
| 成就数据库     |          |          | √        |          |          |          |
| 竞赛数据库     |          |          | √        |          |          |          |
| 博客数据库     |          | √        |          |          |          |          |
| 项目数据库     |          |          |          |          |          | √        |
| 交易数据库     |          |          |          |          | √        |          |

# 6 系统出错处理设计

## 6.1 出错信息

> 用一览表的方式说明每种可能的出错或故障情况出现时 系统输出信息的形式 含意及处理方法

| 出错/故障情况         | 系统输出信息形式 | 处理方式                                         |
| --------------------- | ---------------- | ------------------------------------------------ |
| 用户注册/登录信息错误 | userError        | 提示注册/登录失败，请重新输入                    |
| 用户权限错误          | permissionError  | 提示用户无权访问/修改相关内容                    |
| 数据库异常            | dataError        | 提示用户当前数据库异常，需要检查维修，请稍后重试 |
| 网络异常              | networkError     | 提示用户现在网络环境不稳定，请稍后重试           |
| 服务器异常            | serverError      | 提示用户当前服务器出错，需要检查维修，请稍后重试 |

## 6.2 补救措施

> 说明故障出现后可能采取的变通措施 包括

1. > 后备技术 说明准备采用的后备技术 当原始系统数据万一丢失时启用的副本的建立和启动 的技术 例如周期性地把磁盘信息记录到磁带上去就是对于磁盘媒体的一种后备技术

2. > 降效技术 说明准备采用的后备技术 使用另一个效率稍低的系统或方法来求得所需结果的 某些部分 例如一个自动系统的降效技术可以是手工操作和数据的人工记录

3. > 恢复及再启动技术 说明将使用的恢复再启动技术 使软件从故障点恢复执行或使软件从头 开始重新运行的方法

> 

1. 后备技术：利用云服务商提供的创建镜像功能，周期性地保存系统状态，一旦系统出错则读取最近的保存，降低损失和影响。
2. 降效技术：人工审查用户代码异常运行日志以及项目平台的异常运行记录。
3. 恢复及再启动技术：让多件事务做绑定，一旦其中一个撤销，其余所有相关的事务也要撤销。若事务已经写入数据库，其余相关的事务也要重做。采用投票机制进行恢复：多个组件或子系统采用相同或不同地算法计算同一件事，如果各自结果不一致，则少数服从多数。

## 6.3 系统维护设计

> 说明为了系统维护的方便而在程序内部设计中作出的安排 包括在程序中专门安排用于系统的检查与维护的检测点和专用模块

1. 定时检查模块

一方面，通过自动的方式，定时扫描服务器的内存使用情况、工作负载和硬盘占用情况，以及数据库的一致性等，从而可以预防异常情况的发生；另一方面，通过人工的方式，定期修复用户报告的bug，从而不断提高系统的鲁棒性。

1. 日志模块

系统运行的过程中，会有大量的用户操作，大量的数据更新以及产生大量的输出，通过记录这些内容，在出错时可以快速恢复到出错前的状态，同时也可以通过分析这些数据，预防可能发生的错误，及时地解决问题。

1. 运行数据分析模块

如前所述，系统运行的过程中，会产生大量的数据，同时也有大量的用户行为记录。一方面，可以通过一些PL技术自动分析出系统运行是否正常；另一方面，也可以通过ml技术分析出用户的行为模式以及偏好，从而为下一个版本的迭代提供基础。