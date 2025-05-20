# Traffic_light_controller_Synthesis

## Aim:

Synthesize Traffic Light Controller design using Constraints and analyse area and Power reports.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)

### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.

### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being used.

◦ csh

◦ source /cadence/install/cshrc

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.

• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

## Traffic.v:
```
`timescale 1 ns / 1 ps

module TrafficLight_tb();

reg rst, clk;
wire [2:0] LED_NS, LED_WE;

TrafficLight L(clk, rst, LED_NS, LED_WE);

initial begin 
clk = 1;
forever #1 clk = ~clk;
end

initial begin
   rst = 1;

#10 rst = 0;

#4000 $finish;
end

endmodule


```
## Traffic_run.tcl:
```
read_libs /cadence/install/FOUNDRY-01/digital/90nm/dig/lib/slow.lib
read_hdl ALU.v
elaborate
read_sdc alu_design_constrains.sdc 
syn_generic
report_area
syn_map
report_area
syn_opt
report_area 
report_area > alu_area.txt
report_power > alu_power.txt
write_hdl > alu_netlist.v
gui_show
```
## input constraints:
```
create_clock -name clk -period 1 -waveform {0 0.5} [get_ports "clk"]
set_clock_transition -rise 0.1 [get_clocks "clk"]
set_clock_transition -fall 0.1 [get_clocks "clk"]
set_clock_uncertainty 0.01 [get_ports "clk"]
set_input_delay -max 1.0 -clock clk [all_inputs]
set_output_delay -max 1.0 -clock clk [all_outputs]

```

## Synthesis RTL Schematic :

![WhatsApp Image 2025-05-20 at 14 41 20_33986c2b](https://github.com/user-attachments/assets/e1b30479-585e-4cc8-b9fb-ea0ee790f141)


## Area report:

![WhatsApp Image 2025-05-20 at 14 41 20_b7c9b736](https://github.com/user-attachments/assets/57fa1fb8-8d45-419b-82f1-e3fa4890f92c)


## Power Report:

![WhatsApp Image 2025-05-20 at 14 41 20_42ba5763](https://github.com/user-attachments/assets/d01f9a60-9a34-4186-a045-2ebb4c8b8d04)


## Result:

The generic netlist of Traffic Light Controller has been created, and area, power reports have been tabulated and generated using Genus.
