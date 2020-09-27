# 数字IC笔记整理

<!-- TOC -->

- [数字IC笔记整理](#数字ic笔记整理)
    - [数电知识点](#数电知识点)
    - [数字信号处理](#数字信号处理)
    - [IC知识点](#ic知识点)
        - [FPGA原理](#fpga原理)
        - [STA静态时序分析](#sta静态时序分析)
        - [跨时钟域](#跨时钟域)
        - [低功耗](#低功耗)
        - [协议相关](#协议相关)
        - [CRC相关](#crc相关)
        - [Glitch Free时钟切换](#glitch-free时钟切换)
    - [验证相关](#验证相关)
        - [SystemVerilog相关](#systemverilog相关)
        - [DFT相关](#dft相关)
    - [SOC相关](#SOC相关)
    - [项目相关](#项目相关)
        - [Serdes相关](#serdes相关)
        - [波形捕获率](#波形捕获率)
        - [插值滤波器](#插值滤波器)
    - [题目](#题目)
        - [经典题目](#经典题目)
        - [笔试题目](#笔试题目)
    - [代码](#代码)
    - [参考书目及帖子清单](#参考书目清单)

<!-- /TOC -->

## 数电知识点
[CMOS Example lnv(A+BC)C+D](./src/vedio/CMOS%20Example%20[Inv(A+BC)C+D].mp4)

[CMOS Digital Logic Circuits](http://fourier.eng.hmc.edu/e84/lectures/ch4/node15.html)

[加法器-半加器、全加器与超前进位加法器](https://blog.csdn.net/vivid117/article/details/91980665#comments)

[Brent-Kung树形加法器原理与设计](https://blog.csdn.net/zhouxuanyuye/article/details/104034586)

[乘法器相关的知识包括Booth乘法器](./src/docs/Verilog_HDL_那些事儿_时序篇v2.pdf)

[乘法器的原理](https://note.youdao.com/ynoteshare1/index.html?id=21a32bc5552244d7091ba2b4d84d8129&type=note)

## 数字信号处理

[深入浅出数字信号处理](./src/docs/深入浅出数字信号处理.pdf)

## IC知识点  

### FPGA原理

[FPGA相关知识](./src/docs/FPGA相关知识.pdf)

FPGA开发中，细节的描述总结：[FPGA设计与验证流程](./src/docs/FPGA设计与验证流程.md)

[数字前端和FPGA的技能与区别](http://news.moore.ren/industry/145195.htm)

[数字前端后端的区别、以及流程简介](https://www.cnblogs.com/youngforever/p/3142483.html)

### STA静态时序分析

[建立时间保持时间笔记](./src/docs/建立时间保持时间.md)

[建立时间保持时间关系详解](https://www.cnblogs.com/lilto/p/9581143.html)

[从CMOS到建立时间和保持时间](https://zhuanlan.zhihu.com/p/120863919)

[建立时间保持时间经典题目](https://reborn.blog.csdn.net/article/details/100049997?utm_source=app)

两篇总结的很好的时序约束的文章：

[八小时超长视频教你掌握FPGA时序约束！](https://mp.weixin.qq.com/s/V3qCQNCcxpO_PaWso3GWkw)

[时序约束策略](https://mp.weixin.qq.com/s/dmJck_7vDd57JFvAL3dFpg)

结合上面一文整理的时序约束笔记: [时序约束笔记](./src/docs/时序约束整理.md)

[FPGA时序分析—vivado篇](https://mp.weixin.qq.com/s/gkXRNblISyUIIrIxLRmLgw)

[UltraFast 设计方法时序收敛快捷参考指南——xilinx文档](./src/docs/c_ug1292-ultrafast-timing-closure-quick-reference.pdf)

### 跨时钟域

[跨时钟域处理3大方法揭秘](./src/docs/跨时钟域处理3大方法揭秘.pdf)

针对快时钟域到慢时钟域情况：

[FPGA跨时钟域的处理方法](https://blog.csdn.net/emperor_strange/article/details/82491085?utm_source=app)

[《硬件架构的艺术》](./src/docs/硬件架构的艺术.pdf)第三章

[格雷码与异步FIFO笔记](./src/docs/异步FIFO.md)

[**异步FIFO深度计算**](./src/docs/fifodepthcalculationmadeeasy2.pdf)

[异步FIFO面试题](./src/docs/异步FIFO面试题.md)

这个PPT对跨时钟域几种情况进行了很好的总结：[CDC——讲师卢子威](./src/docs/CDC--讲师卢子威.pptx)

根据上面PPT总结的文档：[CDC总结](./src/docs/CDC总结.md)

[Clifford E. Cummings的异步FIFO设计论文1](./src/docs/Simulation%20and%20Synthesis%20Techniques%20for%20Asynchronous%20FIFO%20Design.pdf)

[Clifford E. Cummings的异步FIFO设计论文2](./src/docs/Simulation%20and%20Synthesis%20Techniques%20for%20Asynchronous%20FIFO%20Design%20with%20Asynchronous%20Pointer%20Comparisons.pdf)

[跨时钟域文献:跨越鸿沟_同步世界中的异步信号(英文版)](./src/docs/CrossClockDomain_design.pdf)

[跨时钟域文献:跨越鸿沟_同步世界中的异步信号(中文版)](./src/docs/跨越鸿沟_同步世界中的异步信号.pdf)

下面两篇专利介绍了深度不是2的幂的FIFO设计：

[深度不是2的幂的异步FIFO存储器设计](./src/docs/深度不是2的幂的异步FIFO存储器设计.pdf)

[实现任意深度异步FIFO的方法及系统](./src/docs/实现任意深度异步FIFO的方法及系统.pdf)

### 低功耗

[低功耗文章](./src/docs/低功耗.pdf) [原文地址](https://mp.weixin.qq.com/s/Hwgj-sarqxDNt4ILYxMOVQ)

[《硬件架构的艺术》](./src/docs/硬件架构的艺术.pdf)第五章

[低功耗笔记](./src/docs/低功耗笔记.md)


### 协议相关

[IIC和SPI](https://mp.weixin.qq.com/s/0WFeSQjcTPNIUfeHHCOw1A)  

[AXI-4](https://mp.weixin.qq.com/s/jCd78u7Gx1l5XfJuf5AEhg)  

[JESD204B](https://mp.weixin.qq.com/s/wPxqvxnwuCwKLfoqUbxGGg)

### CRC相关

[A PAINLESS GUIDE TO CRC ERROR DETECTION ALGORITHMS](./src/docs/A%20PAINLESS%20GUIDE%20TO%20CRC%20ERROR%20DETECTION%20ALGORITHMS.pdf)

[CRC、LFSR电路](https://note.youdao.com/ynoteshare1/index.html?id=a99dd6686501a06c8ed39cd2c40f9aed&type=note)

[环形、扭环、LFSR计数器](https://blog.csdn.net/Reborn_Lee/article/details/102172111?utm_source=app)

[线性反馈移位寄存器(Linear Feedback Shift Register, LFSR)](https://blog.csdn.net/qq_44113393/article/details/89852994)

[循环冗余校验(CRC)算法入门](https://www.cnblogs.com/sinferwu/p/7904279.html)

[CRC算法的硬件电路实现：串行电路和并行电路](https://zhuanlan.zhihu.com/p/59666086)

[使用Verilog实现CRC-8的串行计算](https://blog.csdn.net/zhangningning1996/article/details/106795689)

[数字IC笔试题_CRC并行计算](https://zhuanlan.zhihu.com/p/69969288)

[数字IC笔试——乐鑫提前批笔试编程题源码](https://blog.csdn.net/qq_41844618/article/details/106822610)

两个在线生成并行CRC Verilog代码工具：[CRC在线工具](https://www.easics.com/webtools/crctool) [CRC generator](http://outputlogic.com/?page_id=321)

### Glitch Free时钟切换

[Glitch Free时钟切换技术](https://mp.weixin.qq.com/s/w3Wu7HkSr5v94kHrLvRIcw) 

[Glitch Free时钟切换技术另一篇博客](https://blog.csdn.net/Reborn_Lee/article/details/90378355?tdsourcetag=s_pctim_aiomsg)

## 验证相关

### SystemVerilog相关

SystemVerilog验证--测试平台编写指南：

[SystemVerilog验证](./src/docs/SystemVerilog验证.pdf)

[SystemVerilog覆盖率](https://mp.weixin.qq.com/s/qVSfcVtxHgKDYzXfEelT8w)

[SystemVerilog函数和任务](https://mp.weixin.qq.com/s/kU7g_u4M2vrh1ZHs9TGcpA)

### DFT相关

[DFT基础](./src/docs/DFT基础.pdf)

### SOC相关

[SoC设计方法与实现_第3版.pdf](./src/docs/SoC设计方法与实现_第3版.pdf)

[DarkRISC-V开源代码](https://github.com/darklife/darkriscv)

risc-v介绍博客：[从零开始写RISC-V处理器](https://liangkangnan.gitee.io/2020/04/29/%E4%BB%8E%E9%9B%B6%E5%BC%80%E5%A7%8B%E5%86%99RISC-V%E5%A4%84%E7%90%86%E5%99%A8/)

博客对应代码repo: [tinyriscv](https://gitee.com/liangkangnan/tinyriscv)

[RISC-V手册中文版](./src/docs/RISC-V手册ch.pdf)

此repo实现了一个简单的MIPS五级流水CPU：

[计算机组成原理实验与参考实现](https://github.com/lvyufeng/step_into_mips)

实现简单MIPS五级流水CPU对应视频：[教你写一个简单的CPU](https://www.bilibili.com/video/BV1pK4y1C7es)

## 项目相关

### Serdes相关

[SerDes知识讲解_通俗易懂](https://blog.csdn.net/zjy900507/article/details/99833625?utm_source=app)

[Serdes知识讲解](./src/docs/serdes知识讲解.pdf)

[SerDes知识讲解网页版](https://blog.csdn.net/Next_FSE/article/details/73521821)

[Serdes原理](https://blog.csdn.net/dumgeewang/article/details/104557326)

[深入浅出理解SerDes](https://mp.weixin.qq.com/s/OT7bR_XZAtNf5RpQ6pPiaw)

[xilinx 高速收发器Serdes深入研究](https://blog.csdn.net/u010161493/article/details/77688024)

[轻松实现高速串行](./src/docs/轻松实现高速串行.pdf)

### 波形捕获率

[波形捕获率的计算](./src/docs/波形捕获率.md)

### 插值滤波器

[插值代码分析](./src/docs/插值代码分析.pdf)

[FIR滤波器](https://blog.csdn.net/vast_sea/article/details/8194814)

## 题目

### 经典题目

[FPGA&ASIC笔面试题船新版本](./src/docs/FPGA&amp;ASIC笔面试题船新版本.pdf)

[FPGA&数字IC开发工程师笔试116题](./src/docs/FPGA&数字IC开发工程师笔试116题.pdf)

[逻辑问题汇总思维导图](./src/docs/逻辑问题汇总.xmind) **[逻辑问题汇总资料整理](./src/docs/逻辑问题汇总整理.md)**

[师兄整理的笔记](./src/docs/师兄笔记.pdf)

[数字IC面试题—来自师兄的整理](./src/docs/数字IC面试题.pdf)

[数字电路基础知识点](./src/docs/数字电路基础知识修改版2017_SK.txt)

### 笔试题目

[2021 vivo数字IC提前批笔试题](https://mp.weixin.qq.com/s/agjqs9Z1UG4Ep05NI1PhHA)

[2021年vivo校招提前批笔试解析](https://mp.weixin.qq.com/s/Q3uMxQx7MT8Zy2hDIaDX-w)

[2021 乐鑫数字IC提前批笔试题](https://mp.weixin.qq.com/s/sliUUUQMdLJKTarxxy2VPQ)

[2021 乐鑫数字IC提前批笔试题解析](https://mp.weixin.qq.com/s/PtRJ9FlN8dMJbjmcZNIMkg)

[2020华为海思校招芯片岗笔试解析](https://mp.weixin.qq.com/s/sxnnhUtYUuwzJWaCEyKtQg)

[达尔闻笔试题系列](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzg5MDIwNjIwMA==&action=getalbum&album_id=1319397476696244225&scene=173#wechat_redirect)

## 代码

[Verilog HDL程序设计-135例](./src/docs/Verilog%20HDL程序设计-135例.pdf)

[异步FIFO的Verilog代码实现](./src/code/async_fifo_verilog.md)

[跨时钟域_握手信号代码](./src/code/cdc_code.md)

[CRC相关代码](./src/code/crc_code.md)

[用Verilog实现Glitch free时钟切换电路](./src/code/glitch_free.md)

[Verilog实现串并/并串转换](./src/code/easy_serdes_code.md)

[三分频/奇数分频代码](./src/code/divide_three.md)

[用Verilog实现PWM控制呼吸灯](./src/code/pwm_cnt.md)

[按键消抖代码](./src/code/debounce.md)

## 参考书目清单

[硬件架构的艺术](./src/docs/硬件架构的艺术.pdf)

硬件架构的艺术英文原版：[The Art of Hardware Architecture](./src/docs/The_Art_of_Hardware_Architecture.pdf)

[ASIC高性能数字系统设计](./src/docs/ASIC高性能数字系统设计.pdf)

[UVM实战卷I](./src/docs/UVM实战%20卷Ⅰ.pdf)

[Verilog\_HDL\_那些事儿\_时序篇v2](./src/docs/Verilog_HDL_那些事儿_时序篇v2.pdf)

[数字IC设计前端推荐书籍](https://zhuanlan.zhihu.com/p/105718069)

[SoC设计方法与实现](./src/docs/SoC设计方法与实现_第3版.pdf)

[DarkRISC-V开源代码](https://github.com/darklife/darkriscv)

risc-v介绍博客：[从零开始写RISC-V处理器](https://liangkangnan.gitee.io/2020/04/29/%E4%BB%8E%E9%9B%B6%E5%BC%80%E5%A7%8B%E5%86%99RISC-V%E5%A4%84%E7%90%86%E5%99%A8/)

博客对应代码repo: [tinyriscv](https://gitee.com/liangkangnan/tinyriscv)

[RISC-V手册中文版](./src/docs/RISC-V手册ch.pdf)

[综合与时序分析的设计约束中文版](./src/docs/综合与时序分析的设计约束.pdf)

[综合与时序分析的设计约束英文版](./src/docs/Constraining_Designs_for_Synthesis_and_Timing_Analysis.pdf)