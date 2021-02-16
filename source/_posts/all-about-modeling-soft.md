---
title: 关于建模软件的。。。
tags:
  - math
  - mathematic modelling
categories:
date: 2016-08-31 12:04:12
---

_1年前老博客上的文章了，刚好在这次国赛前找到了，修正一些数据后重新发布_

<!-- more -->

# Lingo

> LINGO全称是Linear INteractive and General Optimizer的缩写—-交互式的线性和通用优化求解器。它是一套设计用来帮助您快速，方便和有效的构建和求解线性，非线性，和整数最优化模型的功能全面的工具。包括功能强大的建模语言，建立和编辑问题的全功能环境，读取和写入Excel和数据库的功能，和一系列完全内置的求解程序.

[Official Website](http://www.lindo.com/)

_强迫症表示不用正版不舒服233333（其实是因为破解版大多都不完整）_

1.  在官网上找到合适的版本[List](http://www.lindo.com/index.php?option=com_content&view=article&id=35&Itemid=20)
    ![](http://7rf2em.com1.z0.glb.clouddn.com/blogsoft-1.png)
    填写好信息
    ![](http://7rf2em.com1.z0.glb.clouddn.com/blogsoft-2.png)
    在邮箱里可以收到下载地址
    或者直接在此下载（我只下了Lingo15的64位Linux版和Windows版，需要其他版本的只能自己去下了）
    [Linux64](http://www.lindo.com/downloads/LINGO-LINUX-64x86-15.0.run)
    [Windos64](http://www.lindo.com/downloads/LINGO-WINDOWS-64x86-15.0.zip)
2.  首次运行选择Demo，即免费试用版，然后会有提示教育用途可以申请完整的lingo licence。确认之后会在lingo的安装目录下生成一个userinfo.txt
3.  将该文件以附件的形式发送给sales@lindo.com。_所以建议使用edu邮箱_
    ![](http://7rf2em.com1.z0.glb.clouddn.com/blogsoft-3.png)
    在72小时内会收到回复（我只等了1个小时）
    ![](http://7rf2em.com1.z0.glb.clouddn.com/blogsoft-4.png)
    就有了一个为期6个月的educational research
    license key。
4.  输入license
    ![](http://7rf2em.com1.z0.glb.clouddn.com/blogsoft-5.png)
    就可以使用正版的lingo了
    ![](http://7rf2em.com1.z0.glb.clouddn.com/blogsoft-6.png)
5.  刚刚发邮件确认了这个key到期了可以再申请（所以学生期间可以无限用咯）

_补充：demo的license是1年，edu申请的是半年但是后者功能没有任何限制，变量个数等都是unlimited_

# Matlab
> MATLAB® 是一个高级语言交互环境，可执行数值计算、可视化和编程。使用 MATLAB，您可以分析数据、开发算法以及创建模型和应用程序。使用相应语言、工具和内置数学函数，您可以探索多种方法并实现比电子表格或其他编程语言（例如 C/C++ 或 Java®）更快的解决方案。您可以将 MATLAB 用于大量应用，包括信号处理和通信、图像和视频处理、源代码管理系统、测试和测量、计算金融和计算生物。本行业及学术界中有超过一百万工程师和科学家使用 MATLAB 这种技术计算语言。

## 安装

[Download](https://cn.mathworks.com/downloads/web_downloads/select_platform)
_由于官网速度以及流量问题，还是推荐去pt站下载吧_
在[MathWorks](https://cn.mathworks.com/programs/trials/trial_request.html)上可以申请30天的封装试用版的试用。
![](http://7rf2em.com1.z0.glb.clouddn.com/blogsoft-7.png)
要是像我一样有正版强迫症也可以叫美国同学帮你弄教育版或者自己去[生成美国人信息](http://www.fakenamegenerator.com/advanced.php)，再[注册edu](https://eims.maricopa.edu/MAW/MAW.html)_没有验证过可行性_

PS: MathWorks 为 cumcm提供软件支持. 如果您的团队参加该竞赛并需要相关软件，可以[申请使用](http://cn.mathworks.com/academia/student-competitions/cumcm/)

## 工具箱

从matlab7.0的1G不到到现在2015a的7G多，除了软件的更新，另外就是工具箱的扩充。安装的时候其实没必要全装了（120G的SSD实在吃不消

常用的有

1.计算金融学

> 开发定量化应用程序并为市场数据建模，以实现最佳化性能和最小化风险。您可以将模型与数据来源集成，然后部署应用程序。

*   MATLAB
*   Curve Fitting Toolbox
*   Database Toolbox
*   Datafeed Toolbox
*   Econometrics Toolbox
*   Financial Instruments Toolbox
*   Financial Toolbox
*   Global Optimization Toolbox
*   Optimization Toolbox
*   Parallel Computing Toolbox
*   Spreadsheet Link EX
*   Statistics and Machine Learning Toolbox
*   Symbolic Math Toolbox

2.控制系统

> 通过开发定制算法，在实施之前检验设计一致性并自动部署代码，来建模、确认和验证您的控制设计。

*   MATLAB
*   Simulink
*   Control System Toolbox
*   Signal Processing Toolbox
*   Simulink Control Design
*   Stateflow
*   System Identification Toolbox

3.图像处理和计算机视觉

> 使用图形工具对图像和视频进行可视化和操作。连接到硬件并使用参考标准算法库开发新思路。

*   MATLAB
*   Computer Vision System Toolbox
*   Image Acquisition Toolbox
*   Image Processing Toolbox
*   Parallel Computing Toolbox
*   Signal Processing Toolbox
*   Statistics and Machine Learning Toolbox

4.信号处理和通信

> 探索信号处理和应用，如音频和语音处理、信号可视化、时间和频率分析以及信号数据分析。

*   MATLAB
*   Simulink
*   Communications System Toolbox
*   Data Acquisition Toolbox
*   DSP System Toolbox
*   Instrument Control Toolbox
*   Signal Processing Toolbox

5.计算生物学

> 使用数据分析和数学建模来理解和预测生物行为。您可以导入数据并为其建模，自动化工作流程元素，然后为研究定制算法和工具。

*   MATLAB
*   Bioinformatics Toolbox
*   Curve Fitting Toolbox
*   Global Optimization Toolbox
*   Image Processing Toolbox
*   Instrument Control Toolbox
*   Optimization Toolbox
*   Parallel Computing Toolbox
*   Signal Processing Toolbox
*   SimBiology
*   Statistics and Machine Learning Toolbox

6.数据分析

> 通过实施数据拟合与动态系统建模和控制，对数据进行分析和深入了解。找到最佳解决方案并开发预测模型。

*   MATLAB
*   Curve Fitting Toolbox
*   Database Toolbox
*   Global Optimization Toolbox
*   Neural Network Toolbox
*   Optimization Toolbox
*   Parallel Computing Toolbox
*   Statistics and Machine Learning Toolbox
*   Symbolic Math Toolbox

# Maple
> Maple内置5,000多个计算函数，覆盖几乎所有的数学领域，领先的计算引擎提供广度、深度、和性能满足各种科学计算需求。Maple透过友好的智能界面提供强大的符号推导功能、无限精度数值计算、丰富的可视化工具、完整的编程语言、广泛的接口、美观的技术文件等，是您进行数学工作的理想工作环境。

中国用户在此查看[产品列表](http://www.cybernet.sh.cn/product/2.html)包括Maple，MapleSim，Maple T.A.等
还有免费的[Maple Player](http://www.cybernet.sh.cn/product/146.html)

> Maple Player 是一个免费程序，允许用户浏览和交互式操作任意的Maple文件。用户即使没有安装Maple软件，也能完成计算、可视化结果、以及探索概念。用户可以使用Maple Player打开其他人创建的Maple文件，Maplesoft也提供了大量的Maple文件，这些资料收集在Application Center，Teacher Resource Center， Maple Primes，The Mobius Project等。

(其实够用了，毕竟用Maple用得少)

# Wolfram Mathematica
> 全球现代技术计算的终极系统

真.神器
介绍看[官网](http://www.wolfram.com/mathematica/)

有Online版的和Desktop版的，但是价格是惊人的贵，而且也只有15天的试用期，无免费版。但是Mathematica和WolframLanguage集成到Raspbian中。所以可以购买只有25刀的[树莓派](https://www.raspberrypi.org/)享用正版Mathematica。（为什么还不剁手！

# R
> R是用于统计分析、绘图的语言和操作环境。R是属于GNU系统的一个自由、免费、源代码开放的软件，它是一个用于统计计算和统计制图的优秀工具。

[下载](https://www.r-project.org/)
新手推荐使用[RStudio](https://www.rstudio.com/)当作IDE。

另也有spss,eviews等收费软件可以使用

# $\LaTeX$
> 基于$TeX$的排版系统，由美国电脑学家莱斯利·兰伯特在20世纪80年代初期开发，利用这种格式，即使使用者没有排版和程序设计的知识也可以充分发挥由$TeX$所提供的强大功能，能在几天，甚至几小时内生成很多具有书籍品质的印刷品。对于生成复杂表格和数学公式，这一点表现得尤为突出。因此它非常适用于生成高印刷质量的科技和数学类文档。这个系统同样适用于生成从简单的信件到完整书籍的所有其他种类的文档。

已经不是建模软件了（关于写论文用$\TeX$还是Word还是看个人喜好吧，$\TeX$更方便更漂亮而已

~~可以使用[CTeX](http://www.ctex.org/HomePage)作为入门套件~~

不推荐使用CTeX

> *   CTeX 封装的 MikTeX 在实现 XeTeX 以及字体库的时候有一些问题，前者导致运行 XeLaTeX 异常缓慢，后者导致使用一些数学字体的时候会报错。
> *   CTeX 封装的默认编辑器 WinEdt 是闭源软件，实际上是在使用盗版软件。
> *   CTeX 封装的默认编辑器 WinEdt 修改了默认编码为 GBK, 这将在后续使用过程中产生很多问题，对初学者来说是不良的。
> *   CTeX 封装的默认编辑器 WinEdt 集成了太多的功能，并且修改了很多 LaTeX 的默认行为，对于初学者来说，这些未经通告的默认行为修改对于其对 LaTeX 的理解是不良的。
> *   CTeX 套装的 2.9.2.164 版本至今已经超过一年未更新，aloft 老大似乎也没有更新的愿望，事实上也没有必要再更新了。
> *   CTeX 由于封装 MikTeX 而只能运行于 Windows 平台。
> *   CTeX 是因为 CJK 包的字体配置复杂，为了免去入门用户的配置成本而推出的。
> *   而现在因为 XeTeX 引擎以及 xeCJK 宏包的出现，CJK 包已经成为过去。并且使用 zhm 可以与 CJK 结合方便地动态配置字体。因此 CTeX 曾经的优势实际上已经不成为优势，并且因其引起的各种国内期刊模板的老旧问题正不断成为阻碍中国 TeX 社区进步的恼人因素。.

推荐使用[TeXlive](http://tug.org/texlive/)+[TeXstudio](http://texstudio.sourceforge.net/)或者用Sublime Text替代TeXstudio

Update: CTeX在2月份开始又由Harry Chen和Liam Huang接手负责开发。所以随意选择啦。。。
PS: WinEdt也拿到了授权
（网上大部分模板都是在CTeX下制作的，如果想省事点的话就。。。