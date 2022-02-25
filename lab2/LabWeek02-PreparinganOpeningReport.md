# **LabWeek02-Preparing an Opening Report**

https://github.com/EstherBear/SoftwareEngineering

## 1 人员介绍

| 姓名   | 学号     |
| ------ | -------- |
| 劳倩茹 | 18340086 |
| 刘思然 | 19335141 |
| 吴天   | 19334019 |
| 赖奕恺 | 19335093 |

## 2 选题介绍

人工智能在线游戏教学网站。

## **3** **目的与意义**

### **3.1** **产物形态**

一款应用于人工智能(AI)教学的**在线评测与游戏网站**。用户可以通过网站上的指导教学来学习和实现各类统计学习、深度学习、强化学习等算法，并将其应用于多种有趣的游戏。提供给用户通过编写AI通关游戏、多玩家通过优化AI进行对战和排名、基于博客社区的知识的扩展与分享等产品形态，从而帮助用户可以在游戏中更好的理解和学习人工智能知识。

### **3.2** **现实价值**

1. 降低人工智能入门学习门槛，普及优质AI实验和游戏，让所有人都可以有机会通过简单有趣的游戏来进行学习AI；
2. 寓教于乐，增加学习AI的趣味性，激发学习者的学习兴趣，并通过游戏的胜利、不同玩家AI的相互对抗、Rank机制、成就系统等设计来给予学习者正反馈，让学习者可以有坚持学习的动力；
3. 提供优质人工智能课程以及各类算法的可视化、可操作，用户通过可视化的操作更好的理解相关知识，人工智能教师可以使用我们的游戏网站作为课程辅助教学
4. 游戏开发者可以使用我们的网站发布可以用于教学的AI游戏，从而获取收益，或者进行免费的知识共享与交流

## **4** **选题思路**

### **4.1** **项目产物的主要功能**

#### 4.1.1 教学机制

1. 网站提供了教学课程以及相关知识的可视化、可操作，用户可以在自行演示中更好的理解算法
2. 网站提供了多种游戏框架的可视化评测，用户提交训练的关键代码，网站能够实时展现用户提交代码的训练的游戏结果，使得用户在学习中编写AI来通关多种游戏

#### 4.1.2 竞技机制

1. 网站提供了不同玩家的AI对战功能，例如应用于坦克大战等对抗类游戏。为多种游戏建立了rank天梯机制、成就收集机制，激励玩家通过编写AI相互对抗、相互学习
2. 网站定期开展人工智能对抗类比赛，以激励用户的学习积极性和提高用户的学习创新能力

#### 4.1.3 盈利机制

1. 网站提供了博客社区功能，用户之间可以进行知识分享、程序开源、付费解答
2. 网站提供了自定义游戏上传功能，用户能够提交自己的编写的游戏来丰富网站的内容，以此来帮助拓展网站的教学和知识分享功能，也为相关的游戏开发展带来一定的收益

### **4.2** **项目产物的性能**

- 高并发：多个用户可以同时在线学习、评测
- 低延迟：编辑代码过程流畅，游戏运行流畅，实时计算、实时显示。

### **4.3** **开发过程**

1. 计划阶段

计划阶段，小组成员充分讨论，确定开发的软件方向和主要功能。

1. 可行性分析阶段

可行性分析阶段，对现有的技术进行充分调研，确定产品各层具体实现方式，明确技术路线。

1. 需求分析阶段

需求分析阶段，确定产品各个模块的具体功能。

1. 程序实现阶段

程序实现阶段，将产品各个部分任务合理分配，小组成员各司其职，完成各部分任务，并进行整合。

1. 测试阶段

测试阶段，对产品各个功能进行测试，保证产品的稳定性、安全性。

1. 维护阶段

最后，产品提供了bug反馈的渠道，由小组成员进行产品的后续维护。

## **5** **技术路线**

### **5.1** **技术原理**

本项目技术原理以UML三层组织框架展开：

- 表现层（UI）
  - 基于html，css，javascript和react框架构建前端用户界面
- 业务逻辑层（BLL）
  - 基于python / pytorch等 对人工智能算法的开发
  - 基于c++ / Rust等 对用户提交代码的评测功能开发
  - 基于Unity / Java等游戏框架的开发
- 数据访问层（DAL)
  - 基于MySQL / sqlite搭建用户数据库，存储用户信息和用户的应用进度信息
  - 基于flask框架搭建文件服务器，便于上层对关键程序文件进行访问

### **5.2** **开发工具**

- 开发框架：React, Pytorch, Flask, Unity, MySQL, sqlite3
- 开发语言：CSS, HTML, JavaScript, Python, C++, Java, Rust

### **5.3** **技术难题**

- 如何实现可视化游戏画面、算法的可操作展示性
- 如何在保障低延迟的条件下实现多用户并发使用
- 如何传递控制器代码到后端，在避免重复编译的条件下运行游戏

## **6** **相关支持条件**

### **6.1** **采用的平台**

平台：Windows 应用市场，云服务器4核 8GB一台

### **6.2** **采用的工具**

- 硬件设备：PC 4台，服务器 1台

- 软件工具：VSCode, 飞书, draw.io, SSH

- 版本控制工具：git, GitHub

### **6.3** **技术成熟度**

- 网站开发技术非常成熟；
- 有很多经典的AI算法和相适配的小游戏；
- 在线游戏可视化技术成熟度还需调研。