---
title: verilog 學習筆記
date: 2022-06-18 14:08:36
tags: [verilog]
---

## 基礎語法

1. module

   ```verilog
    module <module name>(<parameters>);
        // module content
    endmodule
   ```

2. assign

    ```verilog
    assign out = in;
    ```

3. gate
   - and ```&```
   - or ```|```
   - not ```~```
   - xor ```^```

4. wire

    ```verilog
    wire w1, w2, w3;
    ```

5. 多進制表示
   ```<位元長度> ’ <進制表示> <數值資料>```
6. vector

    ```verilog
    // type [upper:lower] vector_name;
    output reg [0:0] y;   // 1-bit reg that is also an output port (this is still a vector)
    input wire [3:-2] z;  // 6-bit wire input (negative ranges are allowed)
    output [3:0] a;       // 4-bit output wire. Type is 'wire' unless specified otherwise.
    wire [0:7] b;         // 8-bit wire where b[0] is the most-significant bit.
    ```

      - implicit nets: 永遠都是 one-bit wire

        ```verilog
        wire [2:0] a, c;   // Two vectors
        assign a = 3'b101;  // a = 101
        assign b = a;       // b =   1  implicitly-created wire
        assign c = b;       // c = 001  <-- bug
        my_module i1 (d,e); // d and e are implicitly one-bit wide if not declared.
                            // This could be a bug if the port was intended to be a vector.
        ```

      - part-select

        ```verilog
        w[3:0]      // Only the lower 4 bits of w
        x[1]        // The lowest bit of x
        x[1:1]      // ...also the lowest bit of x
        z[-1:-2]    // Two lowest bits of z
        b[3:0]      // Illegal. Vector part-select must match the direction of the declaration.
        b[0:3]      // The *upper* 4 bits of b.
        assign w[3:0] = b[0:3];    // Assign upper 4 bits of b to lower 4 bits of w. w[3]=b[0], w[2]=b[1], etc.
        ```

      - bitwise/logical or

        ```verilog
        module top_module(
            input [2:0] a, 
            input [2:0] b, 
            output [2:0] out_or_bitwise,
            output out_or_logical,
        );
            
            assign out_or_bitwise = a | b;
            assign out_or_logical = a || b;
            
        endmodule
        ```

      - concatenation

        - exmaple code

            ```verilog
            input [15:0] in;
            output [23:0] out;
            // Swap two bytes. Right side and left side are both 16-bit vectors.
            assign {out[7:0], out[15:8]} = in;
            // This is the same thing.         
            assign out[15:0] = {in[7:0], in[15:8]};   
            // This is different. The 16-bit vector on the right is extended to 
            // match the 24-bit vector on the left, so out[23:16] are zero.
            // In the first two examples, out[23:16] are not assigned.
            assign out = {in[7:0], in[15:8]};                                          
            ```

        - [例題](https://hdlbits.01xz.net/wiki/Vector3)

            ```verilog
            module top_module (
                input [4:0] a, b, c, d, e, f,
                output [7:0] w, x, y, z );//

                assign {w, x, y, z} = {a, b, c, d, e, f, 2'b11};
                
            endmodule
            ```

        - replication operator

            ```verilog
            // {num{vector}}
            // 5'b11111 (or 5'd31 or 5'h1f)
            {5{1'b1}}
            // The same as {a,b,c,a,b,c}           
            {2{a,b,c}}
            // 9'b101_110_110. It's a concatenation of 101 with
            // the second vector, which is two copies of 3'b110.          
            {3'd5, {2{3'd6}}}                   
            ```

7. always

    ```verilog
    always@(*)begin

    end
    ```

8. case
    - allow duplicate case items
    - case item 有多於一個 statement 的話用 ```begin end```

    ```verilog
    case(<case_expr>)
        condition1: statement1;
        condition2: statement2;
        default: default_statement;
    endcase
    ```

    - casez: 可以忽略一些 bit

        ```verilog
        always @(*) begin
            casez (in[3:0])
                4'bzzz1: out = 0;   // in[3:1] can be anything
                4'bzz1z: out = 1;
                4'bz1zz: out = 2;
                4'b1zzz: out = 3;   // same as 4'b1???
                default: out = 0;
            endcase
        end
        ```

9. always: 一種 procedure
    - conbinational: ```always @(*)```
    - clocked: ```always @(posedge clk)```

    ```verilog
    always @(*) begin
        // do something here
    end
    ```

    - blocking v.s. non-blocking
        - **Continuous** assignments (assign x = y;). Can only be used when not inside a procedure ("always block").
        - Procedural **blocking** assignment: (x = y;). Can only be used inside a procedure.
        - Procedural **non-blocking** assignment: (x <= y;). Can only be used inside a procedure.

10. conditional ternary operator

    就類似像 C 裡面的 conditional ternary operator

    ```verilog
    always @(posedge clk)         // A T-flip-flop.
        q <= toggle ? ~q : q;
    ```

11. reduction

    要把所有 bit and/or/xor 起來一個一個寫很麻煩，所以可以直接寫

    ```verilog
    assign w = ~& a[3:0]; // or ~& a
    ```

    這樣就是對 a 裡面每個 bit 做 NAND

## 酷酷的東西

1. bit slicing

    可以直接寫
    - `[M +: N]`: 拿從 M 開始正的 offset N 個 bits
    - `[M -: N]`: 拿從 M 開始負的 offset N 個 bits
