Combinational Circuit
=====================

Decoder
-------

Gate Level 
^^^^^^^^^^

.. code-block:: verilog

    module Decoder24(A0, A1, EN, Y0, Y1, Y2, Y3);

        input A0, A1, EN;
        output Y0, Y1, Y2, Y3;
        wire W0, W1;

        not U0(W0, A0);
        not U1(W1, A1);
        and U2(Y0, W0, W1, EN);
        and U3(Y1, A0, W1, EN);
        and U4(Y2, W0, A1, EN);
        and U5(Y3, A0, A1, EN);

    endmodule

Behavior Level 
^^^^^^^^^^^^^^

.. code-block:: verilog

    module Decoder24(A0, A1, EN, Y0, Y1, Y2, Y3);

        input A0, A1, EN;
        output reg Y0, Y1, Y2, Y3;

        always @ (*) begin
            if (~EN) {Y3, Y2, Y1, Y0} = 4'b0000;
            else begin
                case ({A1, A0})
                    2'b00: {Y3, Y2, Y1, Y0} = 4'b0001;
                    2'b01: {Y3, Y2, Y1, Y0} = 4'b0010;
                    2'b10: {Y3, Y2, Y1, Y0} = 4'b0100;
                    2'b11: {Y3, Y2, Y1, Y0} = 4'b1000;
                    default: {Y3, Y2, Y1, Y0} = 4'b0000;
                endcase
            end
        end

    endmodule

Testbench
^^^^^^^^^

.. code-block:: verilog

    module Decoder24_tb();

        reg A0, A1, EN;
        wire Y0, Y1, Y2, Y3;
        integer  ID;
        reg [3:0] YE;

        Decoder24 UUT(.A0(A0),.A1(A1),.EN(EN),.Y0(Y0),.Y1(Y1),.Y2(Y2),.Y3(Y3));
        initial begin
            for (ID=0; ID<8; ID=ID+1) begin
                {EN, A1, A0} = ID; #10;
                YE = 4'b0000;
                if (EN) YE[{A1, A0}] = 1'b1;
                if ({Y3, Y2, Y1, Y0}!=YE) begin
                    $display("A0=%b, A1=%b, EN=%b, Y0=%b, Y1=%b, Y2=%b, Y3=%b", 
                            A0, A1, EN, Y0, Y1, Y2, Y3);
                end
            end
            $display("TB Complete");
        end

    endmodule
            