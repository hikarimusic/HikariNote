Sequential Circuit 
==================

S-R Flip Flop 
-------------

Gate Level (Latch)
^^^^^^^^^^^^^^^^^^

*UNVERIFIED*

.. code-block:: verilog

    module SRLatch(G, S, R, Q, QN);

        input G, S, R;
        output Q, QN;
        wire W0, W1;

        nand U0(W0, G, S);
        nand U1(W1, G, R);
        nand U2(Q, QN, W0);
        nand U3(QN, Q, W1);

    endmodule

Behavior Level 
^^^^^^^^^^^^^^

.. code-block:: verilog

    module SRFlipFlop(CLK, CLR, S, R, Q);

        input CLK, CLR, S, R;
        output reg Q;

        always @ (posedge CLK) begin
            if (CLR) Q <= 0;
            else begin
                case ({S, R})
                    2'b00: Q <= Q;
                    2'b01: Q <= 1'b0;
                    2'b10: Q <= 1'b1;
                    2'b11: Q <= 1'bz;
                endcase
            end
        end

    endmodule

Testbench
^^^^^^^^^

.. code-block:: verilog

    module SRFlipFlop_tb();

        reg CLK, CLR, S, R;
        wire Q;
        integer I;

        always begin
            CLK = 1'b0; #5;
            CLK = 1'b1; #5;
        end
        SRFlipFlop UUT(.CLK(CLK),.CLR(CLR),.S(S),.R(R),.Q(Q));
        initial begin
            $dumpfile("SRFlipFlop.vcd");
            $dumpvars(0, SRFlipFlop_tb);
            {S, R} = 2'b00;
            CLR = 1; #10;
            CLR = 0; #10;
            for (I=0; I<8; I=I+1) begin
                {S, R} = I;
                #100;
            end
            $finish;
        end

    endmodule

J-K Flip Flop 
-------------

Gate Level (Latch)
^^^^^^^^^^^^^^^^^^

*UNVERIFIED*

.. code-block:: verilog

    module JKLatch(G, J, K, Q, QN);

        input G, J, K;
        output Q, QN;
        wire W0, W1;

        nand U0(W0, G, J, QN);
        nand U1(W1, G, K, Q);
        nand U2(Q, QN, W0);
        nand U3(QN, Q, W1);

    endmodule

Behavior Level 
^^^^^^^^^^^^^^

.. code-block:: verilog

    module JKFlipFlop(CLK, CLR, J, K, Q);

        input CLK, CLR, J, K;
        output reg Q;

        always @ (posedge CLK) begin
            if (CLR) Q <= 0;
            else begin
                case ({J, K})
                    2'b00: Q <= Q;
                    2'b01: Q <= 1'b0;
                    2'b10: Q <= 1'b1;
                    2'b11: Q <= ~Q;
                endcase
            end
        end

    endmodule

Testbench
^^^^^^^^^

.. code-block:: verilog

    module JKFlipFlop_tb();

        reg CLK, CLR, J, K;
        wire Q;
        integer I;

        always begin
            CLK = 1'b0; #5;
            CLK = 1'b1; #5;
        end
        JKFlipFlop UUT(.CLK(CLK),.CLR(CLR),.J(J),.K(K),.Q(Q));
        initial begin
            $dumpfile("JKFlipFlop.vcd");
            $dumpvars(0, JKFlipFlop_tb);
            {J, K} = 2'b00;
            CLR = 1; #10;
            CLR = 0; #10;
            for (I=0; I<8; I=I+1) begin
                {J, K} = I;
                #100;
            end
            $finish;
        end

    endmodule

D Flip Flop 
-------------

Gate Level (Latch)
^^^^^^^^^^^^^^^^^^

*UNVERIFIED*

.. code-block:: verilog

    module DLatch(G, D, Q, QN);

        input G, D;
        output Q, QN;
        wire W0, W1, W2;

        not U0(W0, D);
        nand U1(W1, G, D);
        nand U2(W2, G, W0);
        nand U3(Q, QN, W1);
        nand U4(QN, Q, W2);

    endmodule

Behavior Level 
^^^^^^^^^^^^^^

.. code-block:: verilog

    module DFlipFlop(CLK, CLR, D, Q);

        input CLK, CLR, D;
        output reg Q;

        always @ (posedge CLK) begin
            if (CLR) Q <= 1'b0;
            else Q <= D;
        end

    endmodule

Testbench
^^^^^^^^^

.. code-block:: verilog

    module DFlipFlop_tb();

        reg CLK, CLR, D;
        wire Q;
        integer I;

        always begin
            CLK = 1'b0; #5;
            CLK = 1'b1; #5;
        end
        DFlipFlop UUT(.CLK(CLK),.CLR(CLR),.D(D),.Q(Q));
        initial begin
            $dumpfile("DFlipFlop.vcd");
            $dumpvars(0, DFlipFlop_tb);
            D = 0;
            CLR = 1; #10;
            CLR = 0; #10;
            for (I=0; I<4; I=I+1) begin
                D = I;
                #100;
            end
            $finish;
        end

    endmodule

T Flip Flop 
-------------

Gate Level (Latch)
^^^^^^^^^^^^^^^^^^

*UNVERIFIED*

.. code-block:: verilog

    module TLatch(G, T, Q, QN);

        input G, T;
        output Q, QN;
        wire W0, W1;

        nand U0(W0, G, T, QN);
        nand U1(W1, G, T, Q);
        nand U2(Q, QN, W0);
        nand U3(QN, Q, W1);

    endmodule

Behavior Level 
^^^^^^^^^^^^^^

.. code-block:: verilog

    module TFlipFlop(CLK, CLR, T, Q);

        input CLK, CLR, T;
        output reg Q;

        always @ (posedge CLK) begin
            if (CLR) Q <= 1'b0;
            else begin
                if (T) Q <= ~Q;
                else Q <= Q;
            end
        end

    endmodule

Testbench
^^^^^^^^^

.. code-block:: verilog

    module TFlipFlop_tb();

        reg CLK, CLR, T;
        wire Q;
        integer I;

        always begin
            CLK = 1'b0; #5;
            CLK = 1'b1; #5;
        end
        TFlipFlop UUT(.CLK(CLK),.CLR(CLR),.T(T),.Q(Q));
        initial begin
            $dumpfile("TFlipFlop.vcd");
            $dumpvars(0, TFlipFlop_tb);
            T = 0;
            CLR = 1; #10;
            CLR = 0; #10;
            for (I=0; I<4; I=I+1) begin
                T = I;
                #100;
            end
            $finish;
        end

    endmodule