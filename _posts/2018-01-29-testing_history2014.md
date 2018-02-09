---
layout:             post
title:                 Software Testing
subtitle:           软件测试领域 近些年的学术发展
date:      	        2018-01-29
author:             XR
header-img:     img/post-bg-unix-linux.jpg
catalog: 	         true
tags:
        - 软件测试
        - 读paper笔记
---

> 这篇paper的完整题目是 
Software Testing: A Research Travelogue (2000–2014)

背景故事
-
- 2000年开了一个会，大家讨论软件测试未来的发展，于是有了Mary Jean Harrold写的roadmap
- 2014年又开会，要求作者写“游记”

背景
-
软件测试直接影响软件的质量
软件测试是学术研究的热点

出题讨论
-
1. 盘点2000年以来的软件测试的发展
2. 未来的挑战

答案整理
-
- research contributions 关注学术界（第2节）
- practical contributions 关注工业界（第3节）

2、Research contributions
-
(1) automated test input generation
(2) testing strategies
(3) regression testing
(4) support for empirical studies

2.1 自动化生成测试集
-
这个方法试图生成一系列的input值，输入到（局部）程序，目标是“覆盖” 或者 达到某些状态（比如某个assertion失效）

- Test input generation不是新的概念，但是近10年出现复兴
	- 可能因为硬件计算能力提高
	- 也是技术本身提高了
		- symbolic execution
		- search-based testing
		- random and fuzz testing
		- 以上的混
		
2.1.1 Symbolic Execution
-
- 符号执行的进步是“自动测试输入生成”进步的主要原因之一
- 静态符号执行是1976年King首次提出的一种程序分析技术
	- 在其最一般的表述中，符号执行使用符号而不是具体输入来执行程序
	- 在计算中的任何一点，程序状态都由一个**符号状态**组成，这个符号状态表示为输入的一个函数
	- 执行到达该点的输入条件通常被表示为一组连接形式的约束: **path condition** (PC)
	
2.1.2 Search-based Testing
-
- “如何生成 使得程序中所有的分支都得到遍历 的最小的测试集？”
- 方法是，首先找到一堆 candidate solution，然后使用 fitness function 进行挑选

2.1.3 Random Testing
-
- 改进了纯随机：Adaptive random testing
- 通过不断增加测试集的 diversity

具体来说，分为2步（有点像遗传算法）

- 1、生成一部分纯随机的测试集
- 2、生成多组测试集
	- 挑选其中“与历史测试集 距离最远”的测试集
	- 循环此步骤
	
本方法的开销比较大，并且难以生成结构复杂的特定的 input

2.1.4 Combined Techniques
-
混合上述的3种测试方法

2.3 Regression Testing（回归测试）
-
- 这个方法适用于出现更改的程序。本质上就是拿程序更改之前的测试集，放到新版本的程序上跑
- 其中注意要进行**选择**，那就是 只留下值得重新跑的那部分测试集，扔掉不必要的测试集
- 另外，还要注意增加新的测试集 (test suite augmentation)

3、Practical contributions
-
- Frameworks for Test Execution
	- 最流行的 framework 是 JUnit
		- 支持许多IDE
		- 支持许多语言
			- C# 的 NUnit
			- PHP 的 PHPUnit
- 这种工具非常适合Agile软件开发
	- 及早测试，经常测试
	- 使用在XP开发和 Scrum 过程中