# FPGA逻辑设计与验证流程

1. HDL代码编写设计

   逻辑设计。逻辑设计完成后必须进行下面的逻辑验证，逻辑验证是为了修改和优化逻辑。

2. 功能仿真

3. 综合

   检查代码书写是否符合verilog语法规范，根据时序约束综合成相应的逻辑电路(RTL)，将HDL语言综合成生成相应的门级电路网表文件。

4. 实现

   * Translating（目的导入约束并合并到网表）

     在翻译过程中，NGDBuild程序执行以下功能：

     * 转换综合后的网表，并且将转换后的结果写入Merged NGD netlist
     * 执行时序规范和逻辑设计规则检查
     * 将用户约束文件（UCF）中的约束导入到Merged netlist。Merged NGD netlist描述了在设计中的位置约束、时序约束等信息。

   * Mapping（映射）

     将网表（设计）映射到的FPGA的CLB和IOB。mapping执行以下功能：

     * 给设计中所有的基本逻辑单元分配相应的CLB和IOB资源
     * 优化LOC物理约束和时序约束，并对映射的网表进行DRC设计规则检查

   * Placing and Routing（布局和布线）

     在设计映射完成后，就可以进行布局布线了。其中有两种布局和布线算法（Place and Route, PAR）

     * 时序驱动布局布线（PAR）
     
       PAR与输入的网表指定的定时约束，约束文件，或两者中运行，默认算法1。
     
     * 无时序驱动PAR
     
       PARA运行，忽略所有时序约束

5. Timing Analysis（时序分析）