# 三分频

```verilog
module Div_three(
	input clk,
	input rst_n,
	output div_three
);
reg [1:0] cnt;
reg div_clk1;
reg div_clk2;

always @(posedge clk or negedge rst_n)begin
	if(rst_n == 1'b0)begin
		cnt <= 0;
	end
	else if(cnt == 2)
		cnt <= 0;
	else begin

		cnt <= cnt + 1;
	end
end

always @(posedge clk or negedge rst_n)begin
	if(rst_n == 1'b0)begin
		div_clk1 <= 0;
	end
	else if(cnt == 0)begin
		div_clk1 <= ~div_clk1;
	end
	else
		div_clk1 <= div_clk1;
end

always @(negedge clk or negedge rst_n)begin
	if(rst_n == 1'b0)begin
		div_clk2 <= 0;
	end
	else if(cnt == 2)begin
		div_clk2 <= ~div_clk2;
	end
	else
		div_clk2 <= div_clk2;
end

assign div_three = div_clk2 ^ div_clk1;
endmodule
```

**参数化**

```verilog
module job_test(//奇数分频
    input clk,
    input rst_n,
    output clk_out
);
parameter   N  = 3 ;    //         (N-1)/2  N-1

reg clk1;
reg[5:0]cnt1;
reg clk2;
reg[5:0]cnt2;

always@(posedge clk or negedge rst_n) begin
	  if( rst_n == 1'b0 )begin   //复位
				cnt1<=0;
				clk1<=0;
	  end
	  else if(cnt1==(N-1)/2) begin
				clk1<=~clk1;   //时钟翻转
				cnt1<=cnt1+1;    //继续计数
	  end
	  else if(cnt1==N-1) begin
				clk1<=~clk1;   //时钟翻转
				cnt1<=0;    //计数清零
	  end
	  else
				cnt1<=cnt1+1;
end



always@(negedge clk or negedge rst_n) begin
	  if(rst_n == 1'b0)begin   //复位
				cnt2<=0;
				clk2<=0;
	  end
	  else if(cnt2==(N-1)/2) begin
				clk2<=~clk2;   //时钟翻转
				cnt2<=cnt2+1;    //继续计数
	  end
	  else if(cnt2==N-1) begin
				clk2<=~clk2;   //时钟翻转
				cnt2<=0;    //计数清零
	  end
	  else
			  cnt2<=cnt2+1;
end

assign   clk_out=clk1 | clk2;  //或运算

endmodule
```

