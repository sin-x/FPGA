### ASIC设计流程以及用到的工具

1. 芯片架构

   考虑芯片定义、工艺、封装

   工具：Fabless

2. RTL设计

   使用Verilog、System Verilog、VHDL进行描述

3. 功能仿真

   理想情况下的仿真

   工具：ModelSim(Questsim Mentor公司)、VCS(Synopsys公司)、NC-Verilog(Cadence公司)

4. 验证

   UVM验证方法学、FPGA原型验证

5. 综合

   逻辑综合，将描述的RTL代码映射到基本逻辑单元门、触发器上

6. DFT技术

   插入扫描链

   工具：Synplify(Synopsys公司)、Design Compile(Synopsys公司)、DFT Compile(Synopsys公司)

7. 等价性检查

   使用形式验证技术

   工具：Formality(Synopsys公司)、Conformal(Cadence公司)

8. 静态时序分析

   工具：Prime Time(Synopsys公司)

9. 布局规划

   保证没有太多的内部交互，避免布线上的拥堵和困扰

   工具：IC Compile(Synopsys公司)

10. 时钟树综合

    均匀地分配时钟，减少设计中不同部分间的时钟偏移

11. DRC

    设计规则检查

    工具：Callibre(Mentor公司)

12. LVS

    布线图和原理图进行比较

    工具：Dracula(Cadence公司)

13. 生成GDSII

这整个流程称为RTL2GDSII，利用GDSII来生产芯片的过程称作流片（Tapeout），以上是一个Fabless公司的简易设计流程，最后将GDSII送至Foundry生产芯片。