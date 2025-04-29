1. WHAT IS SYSTEM VERILOG?
Hardware Description Languages (HDL) like Verilog and VHDL are used to describe  hardware behavior so that it can be converted to digital blocks made up of combinational gates and sequential elements.
In order to verify that the hardware description in HDL is correct, there is a need for a language with more features in OOP that will support complicated testing procedures and is often called a Hardware Verification Language.

SystemVerilog is an extension of Verilog with many such verification features that allow engineers to verify the design using complex testbench structures and random stimuli in simulation.

2. WHY IS VERILOG NOT PREFERRED?
Back in the 1990's, Verilog was the primary language to verify functionality of designs that were small, not very complex and had less features.
As design complexity increases, so does the requirement of better tools to design and verify it.
SystemVerilog is far superior to Verilog because of its ability to perform constrained random stimuli, use OOP features in testbench construction, functional coverage, assertions among many others.

3. WHAT IS VERIFICATION?
Verification is the process of ensuring that a given hardware design works as expected.
Chip design is a very extensive and time consuming process and costs millions to fabricate.
Functional defects in the design if caught at an earlier stage in the design process will help save costs.
If a bug is found later on in the design flow, then all of the design steps have to be repeated again which will use up more resources, money and time.
If the entire design flow has to be repeated, then its called a respin of the chip.

4. WHAT ABOUT VERA, E, AND OTHER SIMILAR HVL?
They have been in use for some time.
SystemVerilog can be considered an extension of Verilog (the most popular HDL), and it makes sense to verify a Verilog design in SystemVerilog.
Also SystemVerilog supports OOP which makes verification of designs at a higher level of abstraction possible.

5. HOW IS IT USED IN VERIFICATION?
A hardware design mostly consists of several Verilog (.v) files with one top module, in which all other sub-modules are instantiated to achieve the desired behavior and functionality.
An environment called testbench is required for the verification of a given verilog design and is usually written in SystemVerilog these days.
The idea is to drive the design with different stimuli to observe its outputs and compare it with expected values to see if the design is behaving the way it should.

In order to do this, the top level design module is instantiated within the testbench environment, and design input/output ports are connected with the appropriate testbench component signals.
The inputs to the design are driven with certain values for which we know how the design should operate.
The outputs are analyzed and compared with the expected values to see if the design behavior is correct.

6. EXAMPLE
Consider a simple verilog design of a D-flip flop which is required to be verified.
The functionality of DFF is that Q output pin gets latched to the value in D input pin at every positive clock edge, which makes it a positive edge-triggered flip-flop.
Let us also assume that the flip-flop has an active-low reset pin and a clock.

// File : d_ff.v
module d_ff (clk, resetn, q, d);

	input clk;
	input resetn;
	input d;
	output q;

	reg q;

	always @ (posedge clk)
		if (! resetn)
			q <= 0;
		else
			q <= d;

endmodule
We need to build a testbench for this design inorder to drive some signal values to its input pins clk, reset, d and observe what the output looks like. By driving appropriate stimuli and checking results, we can be assured of its functional behavior. Synthesis tools can then convert this design into real  hardware logics and gates.

// File : tb_top.sv
module tb_top ();

	reg clk;
	reg resetn;
	reg d;
	wire q;

	// Instantiate the design
	d_ff  d_ff0 (	.clk (clk),
		       		.resetn (resetn),
		       		.d (d),
		       		.q (q));

	// Create a clock
	always #10 clk <= ~clk;

	initial begin
		resetn <= 0;
		d <= 0;

		#10 resetn <= 1;
		#5      d <= 1;
		#8      d <= 0;
		#2      d <= 1;
		#10     d <= 0;
	end
endmodule
The file tb_top represents a simple testbench in which you have created an object of the design d_ff0 and connected it's ports with signals in the testbench. Then, you only need to assign or drive signals in the testbench and they will be passed on to the design.
