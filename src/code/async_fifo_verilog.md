# RTL code for Async FIFO

**fifo1.v - FIFO top-level module**

```verilog
module fifo1 #(parameter DSIZE = 8,
               parameter ASIZE = 4)
    (output [DSIZE-1:0] rdata,
     output             wfull,
     output             rempty,
     input  [DSIZE-1:0] wdata,
     input              winc, wclk, wrst_n,
     input              rinc, rclk, rrst_n);
    
    wire [ASIZE-1:0]    waddr, raddr;
    wire [ASIZE:0]      wptr, rptr, wq2_rptr, rq2_wptr;
    
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
     input                 wclken, wfull, wclk
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

**sync_r2w.v - Read-domain to write-domain synchronizer**

```verilog
module sync_r2w #(parameter ADDRSIZE = 4)
    (output reg [ADDRSIZE:0] wq2_rptr,
     input      [ADDRSIZE:0] rptr,
     input                   wclk, wrst_n
    );
    
    reg [ADDRSIZE:0] wq1_rptr;
    
    always@(posedge wclk or negedge wrst_n)
        begin
            if(!wrst_n) {wq2_rptr, wq2_rptr} <= 0;
            else        {wq2_rptr, wq1_rptr} <= {wq1_rptr, rptr};
        end
endmodule
```

**sync_w2r.v - Write-domain to read-domain synchronizer**

```verilog
module sync_w2r #(parameter ADDRSIZE = 4)
    (output reg [ADDRSIZE:0] rq2_wptr,
     input      [ADDRSIZE:0] wptr,
     input                   rclk, rrst_n);
    
    reg [ADDRSIZE:0] rq1_wptr;
    
    always @(posedge rclk or negedge rrst_n)
        begin
            if(!rrst_n) {rq2_wptr, rq1_wptr} <= 0;
            else        {rq2_wptr, rq1_wptr} <= {rq1_wptr, wptr};
        end
endmodule
```

**rptr_empty.v - Read pointer & empty generation logic**

```verilog
module rptr_empty #(parameter ADDRSIZE = 4)
    (output reg                rempty,
     output     [ADDRSIZE-1:0] raddr,
     output reg [ADDRSIZE  :0] rptr,
     input      [ADDRSIZE  :0] rq2_wptr,
     input                     rinc, rclk, rrst_n);
    
    reg  [ADDRSIZE:0] rbin;
    wire [ADDRSIZE:0] rgraynext, rbinnext;
    
    // -------------------------
    // GRAYSTYLE2 pointer
    // -------------------------
    always @(posedge rclk or negedge rrst_n)
        begin
            if(!rrst_n) {rbin, rptr} <= 0;
            else        {rbin, rptr} <= {rbinnext, rgraynext};
        end
    
    // Memory read-address pointer (okay to use binary to address memory)
    assign raddr     = rbin[ADDRSIZE-1:0];
    
    assign rbinnext  = rbin + (rinc & ~rempty);
    assign rgraynext = (rbinnext >> 1) ^ rbinnext;
    
    // --------------------------
    // FIFO empty when the next rptr == synchronized wptr or on reset
    // --------------------------
    assign rempty_val = (rgraynext == rq2_wptr);
    
    always @(posedge rclk or negedge rrst_n)
        begin
            if(!rrst_n) rempty <= 1'b1;
            else        rempty <= rempty_val;
        end
endmodule
```

**wptr_full.v - Write pointer & full generation logic**

```verilog
module wptr_full #(parameter ADDRSIZE = 4)
    (output reg                wfull,
     output     [ADDRSIZE-1:0] waddr,
     output reg [ADDRSIZE  :0] wptr,
     input      [ADDRSIZE  :0] wq2_rptr,
     input                     winc, wclk, wrst_n);
    
    reg  [ADDRSIZE:0] wbin;
    wire [ADDRSIZE:0] wgraynext, wbinnext;
    
    // GRAYSTYLE2 pointer
    always @(posedge wclk or negedge wrst_n)
        begin
            if(!wrst_n) {wbin, wptr} <= 0;
            else        {wbin, wptr} <= {wbinnext, wgraynext};
        end
    
    // Memory write-address pointer (okay to use binary to address memory)
    assign wbinnext  = wbin + (winc & ~wfull);
    assign wgraynext = (wbinnext >> 1) ^ wbinnext;

    // ---------------------------------------------
    // Simplified version of the three necessary full-tests:
    // assign wfull_val = ((wgraynext[ADDRSIZE]     != wq2_rptr[ADDRSIZE]    ) &&
    //                     (wgraynext[ADDRSIZE-1]   != wq2_rptr[ADDRSIZE-1]  ) &&
    //                     (wgraynext[ADDRSIZE-2:0] == wq2_rptr[ADDRSIZE-2:0]));
    // ---------------------------------------------
    assign wfull_val = (wgraynext == {~wq2_rptr[ADDRSIZE:ADDRSIZE-1],
                                       wq2_rptr[ADDRSIZE-2:0]});
    
    always @(posedge wclk or negedge wrst_n)
        begin
            if(!wrst_n) wfull <= 1'b0;
            else        wfull <= wfull_val;
        end
endmodule
```

