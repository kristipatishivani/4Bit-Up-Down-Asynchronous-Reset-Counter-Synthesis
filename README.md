# Exp-4
# 4Bit-Up-Down-Asynchronous-Reset-Counter-Synthesis

## Aim:

Synthesize 4Bit-Up-Down-Asynchronous-Reset-Counter design using Constraints and analyse reports, Timing, area and Power.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)

◦ SDC (Synopsis Design Constraint) File (.sdc)

 ### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.

•	The SDC File must contain the following commands;

create_clock -name clk -period 2 -waveform {0 1} [get_ports "clk"]

set_clock_transition -rise 0.1 [get_clocks "clk"]

set_clock_transition -fall 0.1 [get_clocks "clk"]

set_clock_uncertainty 0.01 [get_ports "clk"]

set_input_delay -max 0.8 [get_ports "rst"] -clock [get_clocks "clk"]

set_output_delay -max 0.8 [get_ports "count"] -clock [get_clocks "clk"]

i→ Creates a Clock named “clk” with Time Period 2ns and On Time from t=0 to t=1.

ii, iii → Sets Clock Rise and Fall time to 100ps.

iv → Sets Clock Uncertainty to 10ps.

v, vi → Sets the maximum limit for I/O port delay to 1ps.

### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being
used.

◦ csh

◦ source /cadence/install/cshrc

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.

• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.
# Programe
# counter.v
```
`timescale 1ns / 1 ns
module counter(clk,m,rst,count);
input clk,m,rst;
output reg [3:0] count;
always@(posedge clk or negedge rst)
begin
if (!rst)
count=0;
else if(m)
count=count+1;
else
count=count-1;
end
endmodule
```
# run.tcl
```
read_libs /cadence/install/FOUNDRY-01/digital/90nm/dig/lib/slow.lib
read_hdl counter.v
elaborate
read_sdc input_constraints.sdc 

syn_generic
report_area
syn_map
report_area
syn_opt
report_area 

report_area > counter_area.txt
report_power > counter_power.txt
report_timing > counter_timing.txt
report_area > counter_cell.txt
report_gates > counter_gates.txt

write_hdl > counter_netlist.v
write_sdc > output_constraints.sdc 
gui_show
```
# sdc1.t
```
create_clock -name clk -period 2 -waveform {0 1} [get_ports "clk"]
set_clock_transition -rise 0.1 [get_clocks "clk"]
set_clock_transition -fall 0.1 [get_clocks "clk"]
set_clock_uncertainty 0.01 [get_ports "clk"]
set_input_delay -max 0.8 [get_ports "rst"] -clock [get_clocks "clk"]
set_output_delay -max 0.8 [get_ports "count"] -clock [get_clocks "clk"]
```


# Synthesis RTL Schematic :
![WhatsApp Image 2024-11-18 at 13 07 08_63d578d0](https://github.com/user-attachments/assets/a5c6d641-3bc1-4ae9-a33b-b195a42df4b7)
# Area report:
![WhatsApp Image 2024-11-18 at 13 08 24_81db892c](https://github.com/user-attachments/assets/bdea9096-e782-4242-a033-17de81f1ff07)
# Power Report:
![WhatsApp Image 2024-11-18 at 13 07 30_f9f087a4](https://github.com/user-attachments/assets/782a8d23-7b4f-41eb-8c08-76081d32269a)

# Timing Report: 
![WhatsApp Image 2024-11-18 at 13 08 54_e2b7a4bb](https://github.com/user-attachments/assets/c7de1f92-241a-4775-9b1e-dbf65760544d)

# Result:

The generic netlist has been created, and area, power, and timing reports have been tabulated and generated using Genus.





