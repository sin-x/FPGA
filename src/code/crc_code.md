**并行方式——华为**

```verilog
// --------------------------------
// This code is used to check CRC_16 of 8-bits cell data, 
// the generator polynomial is x^16+x^12+x^5+1
// --------------------------------
always@(Crc_out or Data_in)
    begin
        Crc_tmp = Crc_out;
        
        for (i = 7; i >= 0; i = i - 1)
            begin
                Temp = Data_in[i] ^ Crc_tmp[15];
                
                for(j = 15; j > 12; j = j - 1)
                    Crc_tmp[j] = Crc_tmp[j-1];
                Crc_tmp[12] = Temp ^ Crc_tmp[11];
                
                for(k = 11; k > 5; k = k - 1)
                    Crc_tmp[k] = Crc_tmp[k-1];
                Crc_tmp = Temp ^ Crc_tmp[4];
                
                for(l = 4; l > 0; l = l - 1)
                    Crc_tmp[l] = Crc_tmp[l-1];
                Crc_tmp[0] = Temp;
            end
    end
```

**串行方式——华为**

```verilog
// --------------------------------
// This code is used to check CRC_16 of serial data, 
// the generator polynomial is x^16+x^12+x^5+1
// --------------------------------
always@(posedge Reset or posedge Gclk)
    begin
        if(Reset)
            Crc_out <= 16'b0;
        else if(Soc == 1'b1)
            Crc_out <= 16'b0;
        else
            begin
                Temp = Data_in ^ Crc_out[15];
                
                for(j = 15; j > 12; j = j - 1)
                    Crc_out <= Crc_out[j-1];
                Crc_out[12] <= Temp ^ Crc_out[11];
                
                for(k = 11; k > 5; k = k - 1)
                    Crc_out[k] <= Crc_out[k-1];
                Crc_out[5] <= Temp ^ Crc_out[4];
                
                for(l = 4; l > 0; l = l - 1)
                    Crc_out[l] <= Crc_out[l-1];
                Crc_out[0] <= Temp;
            end
    end
```

**并行方式——乐鑫笔试题**

```verilog
// -------------------------------
// 乐鑫科技数字芯片2020
// 将一个串行执行的C语言算法转化为单拍完成的并行可执行Verilog
// -------------------------------
```

```c
unsigned char cal_table_high_first(unsigned char value)
{
    unsigned char i;
    unsigned char checksum = value;
    for(i = 8; i > 0; -- i)
    {
        if(check_sum & 0x80)
        {
            check_sum = (check_sum << 1) ^ 0x31;
        }
        else
        {
            check_sum = (check_sum << 1);
        }
    }
    return check_sum;
}
```

```verilog
// -------------------------------
// 方法一
// -------------------------------
module loop1(
    input clk,
    input rst_n,
    input [7:0] check_sum,
    output reg [7:0] check_sum_o
);
always @ (posedge clk or negedge rst_n)
    if (!rst_n)
        begin
            check_sum_o <= 8'h0;
        end
    else
        begin
            check_sum_o[7] <= check_sum[3]^check_sum[2]^check_sum[5];
            check_sum_o[6] <= check_sum[2]^check_sum[1]^check_sum[4]^check_sum[7];
            check_sum_o[5] <= check_sum[1]^check_sum[7]^check_sum[0]^check_sum[3]^check_sum[6];
            check_sum_o[4] <= check_sum[7]^check_sum[0]^check_sum[3]^check_sum[6];
            check_sum_o[3] <= check_sum[3]^check_sum[7]^check_sum[6];
            check_sum_o[2] <= check_sum[2]^check_sum[6]^check_sum[5];
            check_sum_o[1] <= check_sum[1]^check_sum[5]^check_sum[4]^check_sum[7];
            check_sum_o[0] <= check_sum[0]^check_sum[4]^check_sum[3]^check_sum[6];
        end
endmodule
```

```verilog
// -------------------------------
// 方法二
// -------------------------------
module loop2(
    input            clk,
    input            rst_n,
    input      [7:0] check_sum,
    output reg [7:0] check_sum_o
);
     integer i;
    reg [7:0] ccc;
    
    always@(posedge clk or negedge rst_n)
        if(!rst_n)
            begin
                check_sum_o = 8'h0;
            end
        else
            begin
                ccc = check_sum;
                
                for(i = 0; i < 8; i = i + 1)
                    begin
                        cc = {ccc[6:0], 1'b0} ^ {8{ccc[7]} & 8'h31};
                    end
                check_sum_o = ccc;
            end
endmodule
```

```verilog
// -------------------------------
// 方法三
// 也可以将C语言函数封装成Verilog的function
// 然后在单周期内进行赋值
// -------------------------------
module loop3(
    input            clk,
    input            rst_n,
    input      [7:0] check_sum,
    output reg [7:0] check_sum_o
);
    integer i;
    
    function [7:0] cal_table_high_first;
        input [7:0] value;
        reg   [7:0] ccc;
        reg   [7:0] flag;
        
        begin
            ccc = value;
            
            for(i = 0; i < 8; i = i + 1)
                begin
                    flag = ccc & 8'h80;
                    if(flag != 0)
                        ccc = (ccc << 1) ^ 8'h31;
                    else
                        ccc = (ccc << 1);
                end
        end
    endfunction
    
    always@(posedge clk or negedge rst_n)
        if(!rst_n)
            begin
                check_sum_o = 8'h0;
            end
        else
            begin
                check_sum_o <= cal_table_high_first(check_sum);
            end
endmodule
```

**testbench**  

```verilog
module loop1_tb;
    reg        clk;
    reg        rst_n;
    reg  [7:0] check_sum;
    wire [7:0] check_sum_o;
    
    always #1 clk = ~clk;
    
    initial 
        begin
            clk = 0;
            rst_n = 0;
            
            #10
            rst_n = 1;
            
            for(check_sum = 0; check_sum < 16; check_sum = check_sum + 1)
                begin
                    #2
                    // check_sum = i;
                    $display("check_sum = %h", check_sum_o);
                    if(check_sum == 15) $stop;
                end
        end
    
    loop1 loop1_i1(
        .clk(clk),
        .rst_n(rst_n),
        .check_sum(check_sum),
        .check_sum_o(check_sum_o)
    );
    
endmodule
```

