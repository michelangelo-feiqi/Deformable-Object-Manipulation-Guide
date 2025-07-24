<h1 align="center">柔性物体操作指南 Deformable-Object-Manipulation-Guide</h1>

<p align="center"> </p>

> Deformable-Object-Manipulation 为从事可变形物体 (DOM) 机器人操纵工作的研究人员和从业人员提供的一份指南。
Latest Update: July. 24, 2025

# Contents - 目录
<nav>
  <ul>
    <li><a href="#start">1. Start From Here - 从这里开始</a></li>
    <li><a href="#info">2. Useful Info - 有利于搭建认知的资料</a></li>
    <li><a href="#task">3. Task - 任务</a>
        <ul>
            <li><a href="#algorithm">3.1 1D任务</a></li>
            <li><a href="#algorithm">3.2 2D任务</a></li>
            <li><a href="#algorithm">3.3 3D任务</a></li>
        </ul>        
    </li>
    <li><a href="#algorithm">4. Algorithm - 算法</a>
        <ul>
            <li><a href="#algorithm">4.1 Traditional Method - 传统方法</a>
                <ul>
                    <li><a href="#algorithm">4.1.1 感知</a></li>
                    <li><a href="#algorithm">4.1.2 建模</a></li>
                    <li><a href="#algorithm">4.1.3 规划</a></li>
                    <li><a href="#algorithm">4.1.4 控制</a></li>
                </ul>
            </li>
            <li><a href="#algorithm">4.2 Learning Based Method - 基于学习方法</a>
                <ul>
                    <li><a href="#algorithm">4.2.1 强化学习</a></li>
                    <li><a href="#algorithm">4.2.2 模仿学习</a></li>                
                </ul>
            </li>
            <li><a href="#algorithm">4.3 End to End Method - 端到端方法</a>
                <ul>
                    <li><a href="#algorithm">4.3.1 隐式端到端</a></li>
                    <li><a href="#algorithm">4.3.2 显式端到端</a></li>
                    <li><a href="#algorithm">4.3.3 分层端到端</a></li>                    
                </ul>
            </li>
            <li><a href="#algorithm">4.4 其他范式</a></li>
        </ul>
    </li>
    <li><a href="#software">5.Software - 软件</a>
        <ul>
            <li><a href="#algorithm">5.1 Simulation - 仿真</a></li>
            <li><a href="#algorithm">5.2 Datasets - 数据集</a></li>
            <li><a href="#algorithm">5.3 Benchmark - 基准</a></li>
        </ul>
    </li>    
    <li><a href="#hardware">6. Hardware - 硬件</a>
        <ul>
            <li><a href="#algorithm">6.1 夹爪</a></li>
            <li><a href="#algorithm">6.2 灵巧手</a></li>                
        </ul>
    </li>
  </ul>
</nav>

<section id="start"></section>

# 1. Start From Here - 从这里开始
> 柔性物体：在外力作用下会发生“显著且可恢复或不可恢复的形变”的物体,其形变自由度远超刚性体，无法用有限个位姿参数描述。<br/>
柔性物体操作：是指机器人利用末端执行器（夹爪、灵巧手或工具）对在外力作用下会发生显著形变的物体进行感知、建模、规划与控制，以实现特定任务目标的全过程。
## How - 如何学习这份指南
希望可以帮助新人快速建立领域认知, 所以设计理念是：**简要**介绍目前具身智能涉及到的主要技术, 让大家知道不同的技术能够解决什么问题, 未来想要深入发展的时候能够有头绪。
## About us - 关于我们
我们是一个初学者团队，在我们自己的学习过程中，将一些资料、论文、Code整理总结，搭建理论框架的同时, 为后来者提供一些帮助。

<section id="info"></section>

# 2. Useful Info - 有利于搭建认知的资料
* 会议与期刊：Science Robotics, RSS, IROS, ICRA等
* Daily Paper：
    * Hugging Face-Daily Paper：[website](https://huggingface.co/)
    * arXiv-Daily Pape：[website](http://www.arxivdaily.com/)
* Survey：
    * 同济大学（23年12月）：A Survey on Robotic Manipulation of Deformable Objects: Recent Advances, Open Challenges and New Frontiers[J] [paper](https://arxiv.org/abs/2312.10419)
    * 约克大学（22年2月）：Challenges and outlook in robotic manipulation of deformable objects[J] [paper](https://arxiv.org/abs/2105.01767)

<section id="task"></section>

# 3. Task - 任务
| 类别 | 典型任务 | 柔性对象示例 | 技术难点 |
|-------|------|------|------|
| 0D-零碎柔性体	| 塑料颗粒堆形 |              塑料颗粒 |      类似流体的形态难以建模 |
| 1D-线状柔性体	| 打结、解结、布线 |          绳子、线缆 |    高维状态、自交叉、遮挡 |
| 2D-面状柔性体	| 展开、折叠、铺平、遮挡移除 | 布料、衣物 |    形变复杂、抓取点选择、动态遮挡 |
| 3D-体状柔性体	| 抓取、装袋、开启、填充 |     软袋、软组织	|  体积变化、材质差异、形变耦合 |
| 动态柔性操作	| 甩绳击打、空中形变控制 |     弹性杆、柔性板 | 惯性建模、高速视觉、实时控制 |

## 3.1 0D任务
<img src="./image/0D-task.png" width="300" height="350"/>
任务介绍：在这个任务中，机器人需要将一堆杂乱的颗粒状物体（如坚果）扫成字符t的形状。

## 3.2 1D任务
<img src="./image/1D-task.png"  width="300" height="350"/>
任务介绍：绳索成型，在本任务中，机器人使用拾取-放置原语动作a = (p, q)将随机形状的绳索形成圆形

## 3.3 2D任务
<img src="./image/2D-task.png"  width="300" height="350"/>
任务介绍：展开t恤：这项任务的目标是将一件高度皱褶的短袖t恤弄平,或折叠t恤：将一件展平的短袖折叠

## 3.4 3D任务
<img src="./image/3D-task.png"  width="500" height="350"/>
任务介绍：装袋任务要求将多个刚性（如苹果）和可变形物体（如t恤）装入可变形袋中。


## 3.4 动态任务
<img src="./image/dynamic-task.png" width="500" height="350"/>
任务介绍：上图：绳子操作任务。 目标配置由目标尖端位置（绿色叉）定义。下图：布料操作任务。 目标配置由目标关键点位置（绿点）定义。


<section id="algorithm"></section>

# 4. Algorithm - 算法

## 4.1 Traditional Method - 传统方法

### 4.1.1 感知
### 4.1.2 建模
### 4.1.3 规划
### 4.1.4 控制

## 4.2 Learning Based Method - 基于学习方法

### 4.2.1 强化学习
### 4.2.2 模仿学习

## 4.3 End to End Method - 端到端方法
### 4.3.1 隐式端到端
### 4.3.3 分层端到端

<section id="sftware"></section>

# 5. Software - 软件

## 5.1 Simulation - 仿真
## 5.2 Datasets - 数据集
## 5.3 Benchmark - 基准

<section id="hardware"></section>

# 6. Hardware - 硬件
## 6.1 夹爪
## 6.2 灵巧手