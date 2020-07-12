# 数字IC笔记整理

<!-- TOC -->

- [数字IC笔记整理](#数字ic笔记整理)
    - [数电知识点](#数电知识点)
    - [IC知识点](#ic知识点)
        - [建立时间保持时间](#建立时间保持时间)
        - [跨时钟域](#跨时钟域)
        - [低功耗](#低功耗)
        - [协议相关](#协议相关)
        - [CRC相关](#crc相关)
        - [SystemVerilog相关](#systemverilog相关)
    - [项目相关](#项目相关)
    - [题目](#题目)
        - [经典题目](#经典题目)
        - [笔试题目](#笔试题目)
    - [代码](#代码)

<!-- /TOC -->

## 数电知识点
[CMOS Example lnv(A+BC)C+D](./src/CMOS%20Example%20[Inv(A+BC)C+D].mp4)

[CMOS Digital Logic Circuits](http://fourier.eng.hmc.edu/e84/lectures/ch4/node15.html)

## IC知识点  
### 建立时间保持时间

[建立时间保持时间笔记](./src/建立时间保持时间.md)

[建立时间保持时间关系详解](https://www.cnblogs.com/lilto/p/9581143.html)

[从CMOS到建立时间和保持时间](https://zhuanlan.zhihu.com/p/120863919)

[建立时间保持时间经典题目](https://reborn.blog.csdn.net/article/details/100049997?utm_source=app)

### 跨时钟域

[跨时钟域处理3大方法揭秘](./src/跨时钟域处理3大方法揭秘.pdf)

针对快时钟域到慢时钟域情况：

[FPGA跨时钟域的处理方法](https://blog.csdn.net/emperor_strange/article/details/82491085?utm_source=app)

[《硬件架构的艺术》](./src/硬件架构的艺术.pdf)第三章

[格雷码与异步FIFO笔记](./src/异步FIFO.md)

[**异步FIFO深度计算**](./src/fifodepthcalculationmadeeasy2.pdf)

[异步FIFO面试题](./src/异步FIFO面试题.md)

[Clifford E. Cummings的异步FIFO设计论文1](./src/Simulation%20and%20Synthesis%20Techniques%20for%20Asynchronous%20FIFO%20Design.pdf)

[Clifford E. Cummings的异步FIFO设计论文2](./src/Simulation%20and%20Synthesis%20Techniques%20for%20Asynchronous%20FIFO%20Design%20with%20Asynchronous%20Pointer%20Comparisons.pdf)

[跨时钟域文献:跨越鸿沟_同步世界中的异步信号](./src/CrossClockDomain_design.pdf)

[跨时钟域文献:跨越鸿沟_同步世界中的异步信号(中文版)](./src/跨越鸿沟_同步世界中的异步信号.pdf)

### 低功耗

[低功耗文章](./src/低功耗.pdf) [原文地址](https://mp.weixin.qq.com/s/Hwgj-sarqxDNt4ILYxMOVQ)

[《硬件架构的艺术》](./src/硬件架构的艺术.pdf)第五章

[低功耗笔记](./src/低功耗笔记.md)

### 协议相关

[IIC和SPI](https://mp.weixin.qq.com/s/0WFeSQjcTPNIUfeHHCOw1A)  

[AXI-4](https://mp.weixin.qq.com/s/jCd78u7Gx1l5XfJuf5AEhg)  

[JESD204B](https://mp.weixin.qq.com/s/wPxqvxnwuCwKLfoqUbxGGg)

### CRC相关

[A PAINLESS GUIDE TO CRC ERROR DETECTION ALGORITHMS](./src/A%20PAINLESS%20GUIDE%20TO%20CRC%20ERROR%20DETECTION%20ALGORITHMS.pdf)

[CRC、LFSR电路](https://note.youdao.com/ynoteshare1/index.html?id=a99dd6686501a06c8ed39cd2c40f9aed&type=note)

[循环冗余校验(CRC)算法入门](https://www.cnblogs.com/sinferwu/p/7904279.html)

[CRC算法的硬件电路实现：串行电路和并行电路](https://zhuanlan.zhihu.com/p/59666086)

[使用Verilog实现CRC-8的串行计算](https://blog.csdn.net/zhangningning1996/article/details/106795689)

[数字IC笔试题_CRC并行计算](https://zhuanlan.zhihu.com/p/69969288)

[数字IC笔试——乐鑫提前批笔试编程题源码](https://blog.csdn.net/qq_41844618/article/details/106822610)

### SystemVerilog相关

[SystemVerilog覆盖率](https://mp.weixin.qq.com/s/qVSfcVtxHgKDYzXfEelT8w)

[SystemVerilog函数和任务](https://mp.weixin.qq.com/s/kU7g_u4M2vrh1ZHs9TGcpA)

## 项目相关

[SerDes知识讲解_通俗易懂](https://blog.csdn.net/zjy900507/article/details/99833625?utm_source=app)

[Serdes知识讲解](./src/serdes知识讲解.pdf)

[SerDes知识讲解网页版](https://blog.csdn.net/Next_FSE/article/details/73521821)

[Serdes原理](https://blog.csdn.net/dumgeewang/article/details/104557326)

[xilinx 高速收发器Serdes深入研究](https://blog.csdn.net/u010161493/article/details/77688024)

## 题目

### 经典题目

[FPGA&ASIC笔面试题船新版本](./src/FPGA&amp;ASIC笔面试题船新版本.pdf)

[FPGA&数字IC开发工程师笔试116题](./src/FPGA&数字IC开发工程师笔试116题.pdf)

### 笔试题目

[2021 vivo数字IC提前批笔试题](https://mp.weixin.qq.com/s/agjqs9Z1UG4Ep05NI1PhHA)

[2021年vivo校招提前批笔试解析](https://mp.weixin.qq.com/s/Q3uMxQx7MT8Zy2hDIaDX-w)

[2021 乐鑫数字IC提前批笔试题](https://mp.weixin.qq.com/s/sliUUUQMdLJKTarxxy2VPQ)

[2021 乐鑫数字IC提前批笔试题解析](https://mp.weixin.qq.com/s/PtRJ9FlN8dMJbjmcZNIMkg)

[2020华为海思校招芯片岗笔试解析](https://mp.weixin.qq.com/s/sxnnhUtYUuwzJWaCEyKtQg)

## 代码

[异步FIFO的Verilog代码实现](./src/async_fifo_verilog.md)