# LabWeek01-Preparations

https://github.com/EstherBear/SoftwareEngineering/tree/main/LabWeek01-Preparations

## 熟悉机房工作环境(硬件平台、软件平台)

由于之前已经在实验楼的机房上过其他实验课，对机房工作环境较为熟悉，所以此处不再赘述熟悉过程。

## 熟悉任务管理平台matrix

由于之前的实验课已经多次使用过Matrix，对Matrix较为熟悉，所以此处不再赘述熟悉过程。

## 熟悉draw.io(在线工具)或其他画图工具

为了熟悉draw.io的使用，使用draw.io画出一幅活动图如下：

![image-20220225164024895](https://cdn.jsdelivr.net/gh/EstherBear/PictureBed@master/img/image-20220225164024895.png)

并导出为pdf格式：

![image-20220225164037012](https://cdn.jsdelivr.net/gh/EstherBear/PictureBed@master/img/image-20220225164037012.png)

## 熟悉一个markdown编辑器(可能需要自行下载安装)

由于之前已经配置好了本地的markdown编辑环境(Typora)，并使用markdown撰写过一些实验报告，所以此处不再赘述熟悉过程。

## 建立一个本课程任务的git仓库

建立好本课程的本地目录，并初始化为git仓库：

```shell
git init
```

<img src="https://cdn.jsdelivr.net/gh/EstherBear/PictureBed@master/img/image-20220225164046982.png" alt="image-20220225164046982" style="zoom:67%;" />

添加并创建提交：

```shell
git add .
git commit -m "first commit"
```

<img src="https://cdn.jsdelivr.net/gh/EstherBear/PictureBed@master/img/image-20220225164107417.png" alt="image-20220225164107417" style="zoom:67%;" />

在Github上创建一个新的仓库：

![image-20220225164123500](https://cdn.jsdelivr.net/gh/EstherBear/PictureBed@master/img/image-20220225164123500.png)

推送本地仓库到Github：

```shell
git remote add origin https://github.com/EstherBear/SoftwareEngineering.git
git branch -M main
git push -u origin main
```

<img src="https://cdn.jsdelivr.net/gh/EstherBear/PictureBed@master/img/image-20220225164133482.png" alt="image-20220225164133482" style="zoom:67%;" />

至此，已经建立好了一个本课程任务的git仓库，并把本地的内容同步上去：

![image-20220225164141823](https://cdn.jsdelivr.net/gh/EstherBear/PictureBed@master/img/image-20220225164141823.png)
