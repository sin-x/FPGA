# 题目：用Verilog实现串并转换

* LSB优先
* MSB优先

```verilog
// -----------------------
// LSB first
// -----------------------

module Deserialize (
    input            clk    ,
    input            rst_n  ,
    input            data_i ,
    output reg [7:0] data_o
);
    
    reg [2:0] cnt;
    always @(posedge clk or negedge rst_n)
        begin
            if(rst_n == 1'b0)
                begin
                    data_o <= 8'b0 ;
                    cnt    <= 3'd0 ;
                end
            else
                begin
                    data_o[cnt] <= data_i     ;
                    cnt         <= cnt + 1'b1 ;
                end
        end
    
endmodule
```

```verilog
// -----------------------
// MSB first
// -----------------------

module Deserialize (
    input            clk    ,
    input            rst_n  ,
    input            data_i ,
    output reg [7:0] data_o
);
    
    reg [2:0] cnt;
    always @(posedge clk or negedge rst_n)
        begin
            if(rst_n == 1'b0)
                begin
                    data_o <= 8'b0 ;
                    cnt    <= 3'd0 ;
                end
            else
                begin
                    data_o[7-cnt] <= data_i     ;
                    cnt           <= cnt + 1'b1 ;
                end
        end
    
endmodule
```

```verilog
input  [3:0] data_in  ;
output [3:0] data_out ;
input  [1:0] mode     ;
input        clk      ;
input        rst_n    ;

mode 0: 串行输入data_in[0]，  并行输出data_out[3:0]
mode 1: 并行输入data_in[3:0], 串行输出data_out[0]
mode 2: 并行输入data_in[3:0], 并行输出data_out[3:0], 延迟一个时钟周期
mode 3: 并行输入data_in[3:0], 并行反序输出data_out[3:0],延迟一个时钟周期
data_out[3] = data_in[0];
data_out[2] = data_in[1];
data_out[1] = data_in[2];
data_out[0] = data_in[3];

附加要求：将输入输出的位宽做成参数化
```

```verilog
module Deserialize
    #(
        parameter    DATA_WIDTH = 4,
        parameter    CNT_WIDTH  = log2(DATA_WIDTH)
    )
    (
        input                       clk          ,
        input                       rst_n        ,
        input      [1:0]            mode         ,
        input      [DATA_WIDTH-1:0] data_i       ,
        output reg [DATA_WIDTH-1:0] data_o
    );
    
    // mode 0
    reg [DATA_WIDTH-1:0] data_r0;
    reg                  data_r1;
    reg [DATA_WIDTH-1:0] data_r2;
    reg [DATA_WIDTH-1:0] data_r3;
    
    reg [CNT_WIDTH-1:0]  cnt;
    reg [1:0]            mode_r;
    
    // mode change once cnt restart count
    always@(posedge clk or negedge rst_n)
        begin
            if(rst_n == 1'b0)
                mode_r <= 0;
            else
                mode_r <= mode;
        end
    
    assign change = (mode_r ^ mode) ? 1'b1 : 1'b0;
    
    always@(posedge clk or negedge rst_n)
        begin
            if(rst_n)
                cnt <= 0;
            else if(change == 1'b1)
                cnt <= 0;
            else
                 cnt <= cnt + 1;   
        end
    
    always@(posedge clk or negedge rst_n)
        begin
            if(rst_n == 0)
                data_r0 <= 0;
            else
                data_r0[cnt] <= data_i[0];
        end
    
    // mode 1
    always@(posedge clk or negedge rst_n)
        begin
            if(rst_n == 0)
                data_r1 <= 0;
            else
                data_r1 <= data_i[cnt];
        end
    
    // mode 2
    always@(posedge clk or negedge rst_n)
        begin
            if(rst_n == 0)
                data_r2 <= 0;
            else
                data_r2 <= data_i;
        end
    
    // mode 3
    integer i;
    reg [DATA_WIDTH-1:0] data_r;
    always@(posedge clk)
        begin
            for(i = 0; i <= DATA_WIDTH-1; i = i + 1)
                begin
                    data_r[DATA_WIDTH-1-i] <= data_i[i];
                end
        end
    
    always@(posedge clk or negedge rst_n) 
        begin
            if(rst_n == 0)
                data_r3 <= 0;
            else
                data_r3 <= data_r;
        end
    
    // mux4
    always@(*)
        begin
            case(mode)
                2'b00: data_o = data_r0;
                2'b01: data_o = data_r1;
                2'b10: data_o = data_r2;
                2'b11: data_o = data_r3;
            endcase
        end
    
    // -------------------
    // 以下两个函数任用一个
    // 求2的对数函数
    // -------------------
    function integer log2;
        input integer value;
        begin
            value = value - 1;
            for (log2 = 0; value > 0; log2 = log2 + 1)
                value = value >> 1;
        end
    endfunction
    
    // -------------------
    // 另一个求2的对数的函数
    // -------------------
    function integer clogb2 (input integer bit_depth);
        begin
            for (clogb2 = 0; bit_depth > 0; clogb2 = clogb2 + 1)
                bit_depth = bit_depth >> 1;
        end
    endfunction
    
endmodule
```

