# demo05 - getting started with digital
Up until now we have been focusing on analog design. Now we are going to take a look at digital design. This is a MASSIVE topic and we will only cuver the most simple basics to enable you to get started on your own; please see the resources section.
The large over view is as follows:
- *icarus verilog* is a verilog simulation and systhesis tool. We will use it to compile our verilog into a `*.vvp` file to then be simulated.
- *cocotb* is a python library used for design simulation and verification. We will input the `*.vvp` file and output a waveform `*.vvp` file.
- *gtkwave* is used to view the output waveforms from the `*.vvp` file.
- *OpenLane* is the synthsis, implementation, placement and routing. OpenLane is the tool that takes our verilog and turns it into GDS.

## Design

clk_div.v
```
`default_nettype none
`timescale 10ns/1ns

module clk_div(
	input clk,
	input reset,
	output reg clk_out
);

	//;wire 	in;
	

always @(posedge clk)
begin
	if (reset)
		clk_out = 1;
	else
		clk_out <= ~clk_out;
end

endmodule
```

## Simulation and Verification

dump.v
```
module dump();
    initial begin
        $dumpfile ("clk_div.vcd");
        $dumpvars (0, clk_div);
        #1;
    end
endmodule
```

clk_div_cocotb.py
```
import cocotb
from cocotb.triggers import Timer


@cocotb.test()
async def clk_div_cocotb(dut):
    dut.clk.value = 0
    dut.reset.value = 1
    await Timer(1, units="ns")
    dut.clk.value = 1
    await Timer(1, units="ns")
    dut.reset.value = 0
    await Timer(1, units="ns")

    for cycle in range(10):
        dut.clk.value = 0
        await Timer(1, units="ns")
        dut.clk.value = 1
        await Timer(1, units="ns")

    assert dut.clk_out == 1

```

## Verilog to GDS

```
cd OpenLane

# Start the docker container
make mount

# Create a new design
./flow.tcl -design <design_name> -init_design_config -add_to_designs


# Flow a design
./flow.tcl -design <design_name>


```

Two different configuration files: tcl and json.

config.json example.
```
{
    "DESIGN_NAME": "clk_div",
    "VERILOG_FILES": "dir::src/*.v",
    "CLOCK_PORT": null,
    "CLOCK_PERIOD": 10.0,
    "DESIGN_IS_CORE": false,
    "FP_SIZING": "absolute",
    "DIE_AREA": "0 0 34.5 57.12"

}

```
Please see the OpenLane repo for tcl examples.


## Resources
[Icarus Verilog](http://iverilog.icarus.com/)

[gtkwave](https://gtkwave.sourceforge.net/)

[cocotb](https://www.cocotb.org/)

[OpenLane Docs](https://openlane.readthedocs.io/en/latest/)

[OpenLane Repo](https://github.com/The-OpenROAD-Project/OpenLane)

