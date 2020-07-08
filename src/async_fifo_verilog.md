# RTL code for Async FIFO

**fifo1.v - FIFO top-level module**

```verilog
module fifo1 #(parameter DSIZE = 8,
               parameter ASIZE = 4)
    (output [DSIZE-1:0] rdata,
     output   			wfull,
     output 			rempty,
     input 	[DSIZE-1:0] wdata,
     input 				winc, wclk, wrst_n,
     input 				rinc, rclk, rrst_n);
    
    wire [ASIZE-1:0]	waddr, raddr;
    wire [ASIZE:0]		wptr, rptr, wq2_rptr, rq2_wptr;
    
    sync_r2w sync_r2w (
        .wq2_rptr(wq2_rptr),
        .rptr(rptr),
        .wclk(wclk),
        .wrst_n(wrst_n)
    );
    
    sync_w2r sync_w2r (
        .rq2_wptr(rq2_wptr),
        .wptr(wptr),
        .rclk(rclk),
        .rrst_n(rrst_n)
    );
    
    fifomem #(DSIZE, ASIZE) fifomem
    (
        .rdata(rdata),
        .wdata(wdata),
        .waddr(waddr),
        .raddr(raddr),
        .wclken(winc),
        .wfull(wfull),
        .wclk(wclk)
    );
    
    rptr_empty #(ASIZE) rptr_empty
    (
        .rempty(rempty),
        .raddr(raddr),
        .rptr(rptr),
        .rq2_wptr(rq2_wptr),
        .rinc(rinc),
        .rclk(rclk),
        .rrst_n(rrst_n)
    );
    
    wptr_full #(ASIZE) wptr_full
    (
        .wfull(wfull),
        .waddr(waddr),
        .wptr(wptr),
        .wq2_rptr(wq2_rptr),
        .winc(winc),
        .wclk(wclk),
        .wrst_n(wrst_n)
    );
endmodule
```

**fifomem.v - FIFO memory buffer**

```verilog
module fifomem #(parameter DATASIZE = 8, // Memory data word width
                 parameter ADDRSIZE = 4) // Number of mem address bits
    (output [DATASIZE-1:0] rdata,
     input  [DATASIZE-1:0] wdata,
     input  [ADDRSIZE-1:0] waddr, raddr,
     input   			   wclken, wfull, wclk
    );
    
    `ifdef VENDORRAM
    // instantiation of a vendor's dual-port RAM
    vendor_ram mem (
        .dout(rdata),
        .din(wdata),
        .waddr(waddr),
        .raddr(raddr),
        .wclken(wclken),
        .wclken_n(wfull),
        .clk(wclk)
    );
    
    `else
    // RTL Verilog memory model
    localparam DEPTH = 1 << ADDRSIZE;
    reg [DATASIZE-1:0] mem [0:DEPTH-1];
    
    assign rdata = mem[raddr];
    
    always@(posedge wclk)
        if(wclken && !wfull)
            mem[waddr] <= wdata;
    `endif
endmodule
```

```verilog
module sync_r2w #(parameter ADDRSIZE = 4)
    (output reg [ADDRSIZE:0] wq2_rptr,
     input      [ADDRSIZE:0] rptr,
     input 					 wclk, wrst_n
    );
    
    reg [ADDRSIZE:0] wq1_rptr;
    
    always@(posedge wclk or negedge wrst_n)
    	begin
	        if(!wrst_n) {wq2_rptr, wq2_rptr} <= 0;
            else		{wq2_rptr, wq1_rptr} <= {wq1_rptr, rptr};
        end
endmodule
```

