# 题目：用Verilog实现按键抖动消除电路，抖动小于15ms,输入时钟12MHz

```verilog
module debounce(
	input    clk, // 12mhz
    input    rst_n,
    input    key_in,
    output   key_flag
);
    
    parameter JITTER = 240;  // (12MHz / (1 / 20ms))
    
    reg  [ 1:0] key_r;
    wire        change;
    reg  [15:0] delay_cnt;
    
    always@(posedge clk or negedge rst_n)
        begin
            if(rst_n == 1'b0)
                begin
                    key_r <= 0;
                end
            else
                begin
                    key_r <= {key[0], key_in};
                end
        end
    
    assign change = (~key_r[1] & key_r[0]) | (key_r[1] & ~key_r[0]);
    
    always@(posedge clk or negedge rst_n)
        begin
            if(rst_n == 1'b0)
                delay_cnt <= 0;
            else if(change == 1'b1)
                delay_cnt <= 0;
            else if(delay_cnt == JITTER)
                delay_cnt <= delay_cnt;
            else
                delay_cnt <= delay_cnt + 1'b1;
        end
    
    assign key_flag = ((delay_cnt == JITTER - 1) && (key_in == 1'b1)) ? 1'b1 : 1'b0;
endmodule
```

