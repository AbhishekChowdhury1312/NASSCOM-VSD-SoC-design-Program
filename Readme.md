# Day 1:
## Topics covered:
- VLSI Physical Design flow
- Introduction to RISC-V
- Intoduction to OpenLane
- Basic Linux Terminal Commands
- Openlane commands till Synthesis


# Openlane Commands:
##To invoke Openlane
go to the directory where flow.tcl file is located. Now type the following commands to invoke the tool.  
`docker`  
`./flow.tcl -interactive`  

![Screenshot 2024-07-11 155537](https://github.com/user-attachments/assets/daf3a177-8ba2-437e-af6c-6c4ffde5464a)

## Command to call all the required packages
The following command calls all the required packages.   
`package require openlane 0.9`

![Screenshot 2024-07-11 155641](https://github.com/user-attachments/assets/ace56d0f-263e-409f-b74f-19981deb8dd0)

## Prepare all the required files to run the tool commands:
`prep -design picorv32a`  

![Screenshot 2024-07-11 160543](https://github.com/user-attachments/assets/8fc8e27a-00cf-43ca-a14f-2339ce541ceb)

## Run Synthesis
Our first step is to run synthesis, which means converting RTL to a gate level netlist.   
`run_synthesis`

## Day 1 TASK:
### Find out percentage of D Flipflop:  
percentage of d flip-flops = number of D flip-flops / total number of cells * 100  
find out both the numbers of flip-flops and total cells from yosys_stat report; also it can be found from the generated text after running `run_synthesis` command in the openlane environment.  
Now use the previous equation to find out the percentage of flip-flops present in the design.  

![Screenshot 2024-07-11 235609](https://github.com/user-attachments/assets/907c3d11-c6c5-48d8-b75a-30b650dd92dd)



# Day 2
## Topics Covered  
- Core utilization factor and aspect ratio
-
- Ground bounce and voltage droop

## Utilization factor and aspect ratio
Utilization factor = Area occupied by netlist/Total area of core  
Aspect ratio = Height/Width  
Practically 50-60% utilization is desirable.  

## Ground bounce and voltage droop
If there is only a signle group line and a large number of capacitors are getting discharged, it could create a ground bounce. All the capactiors try to charge from 0->1.  
If there is a single line for all the capacitors, there is voltage droop. All the capactiors try to charge from 1->0.  
If the voltage is in such a level that it's not either 0 or 1; things become unpredictable.  

Supply is from only 1 point, that is the problem. Also, it is not feasible to place decoupling capacitors everywhere in the chip. So we will use multiple Vdd, Vss lines both horizontally and vertically. This is called power mesh.  

## Run Floorplan  
`run_floorplan`  

## To view floorplan
`magic -T (tech_file path) lef read (lef_file path) def read (floorplan.def_file path)`



![Screenshot 2024-07-15 134743](https://github.com/user-attachments/assets/50c9420c-49a1-4e54-aca3-3030a3c1ee9b)

Press 's' to select layout and then 'v' to fit in the entire screen.  
Left mouse button + RIght mouse button + z to zoom in.  
u to undo.  
after selecting any component, type 'what after' in TKcon windows to show metal layer.  

## placement
`run_placement`  
In global placement, main objective is to reduce the wire length. In openlane half parameter wire length concept is used. 

`magic -T (tech_file path) lef read (lef_file path) def read (placement.def file's path)`
* We will do power ground generation before routing and after CTS.

![Screenshot 2024-07-15 160135](https://github.com/user-attachments/assets/e8420622-6386-4f0c-96ac-b76182120483)


# Day 3
## Topics Covered
-
-


## To watch floorplan:
`magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def`

We can see the pins are equidistant. Open openlane/configuration/floorplan.tcl file and find out the following command. If we wanna change pin distance, reset the associated variable.

## changing pin configuration 
`set: env (FP_IO_MODE) 0`



![Screenshot 2024-07-18 015705](https://github.com/user-attachments/assets/265adcd3-e244-41b0-a488-7a4ec082be69)




## Spice simulation:
4 steps:
1. Create spice deck
2. component values
3. identify nodes
4. name nodes

## Some important notes:

"*" is used for commenting.
serial: drain, gate, source, substrate
for example: 
M1 out in vdd vdd pmos W=0.375u L=.25u
Here, drain: out, gate: in,  source: vdd, substrate: vdd. Pmos indicates the device type. W and L indicates width and length of the device.

M2 out in 0 0 nmos W=0.375u L=.25u
Here, drain: out, gate: in,  source: 0, substrate: 0. Nmos indicates the device type. W and L indicates width and length of the device.

### W/L ratio of pmos should be greater than nmos.

cload out 0 10f [out & 0 are 2 nodes; 10f is the value]

Vdd vdd 0 2.5
Vin in 0 2.5

### **Simulation commands**
.op
.dc Vin 0 2.5 0.05 [.05 is step]

### **describe model file**
.LIB "tsmc_025um_model.mod" CMOS_MODELS 
.end

## characteristics:

switching threshold:
where vin = vout
Changing pmos:nmos changes threshold
in threshold region, pmos and nmos both are kind of turned on and there's a high possibility leakage current which flows directly from power to ground.



`magic -T sky130A.tech sky130_inv.mag &` 

press 's' twice to see the connectivity

`extract file into .ext file`
`extract all`

`ext2spice cthresh 0 rthresh 0`
`ext2spice`

extract parasitic components

![Screenshot 2024-08-20 024610](https://github.com/user-attachments/assets/697ed2fb-abe6-4959-8569-dc3e3dfb6014)

need to edit scale. To know the correct scale we open our design and select a grid. Then in TKcon window use the following command:
`box`
![Screenshot 2024-08-20 030046](https://github.com/user-attachments/assets/c34427bd-f0f3-43ce-9ffe-e03dce1f81c9)


After making the edits,
![Screenshot 2024-08-20 033041](https://github.com/user-attachments/assets/47a6b63e-bde4-4d0f-a41c-ce2fc4308035)


To simulate the spice file use the following command:
`ngspice sky130_inv.spice`

![Screenshot 2024-08-20 033057](https://github.com/user-attachments/assets/d79fad61-f6a8-4ee3-bd59-f1217700cefb)

We also can source the file after invoking the tool by the following command:
`source sky130_inv.spice`

We can see the output & input characteristic with respect to time by using following command.
`plot y vs time a`
here a is the input signal in the spice file. y is the output.

![Screenshot 2024-08-20 034357](https://github.com/user-attachments/assets/c25113f7-4514-42c2-ad53-8e8de747de90)

## Characteristics of the curve
![Screenshot 2024-08-20 040720](https://github.com/user-attachments/assets/50e3dfa5-78cc-4dcc-bbfa-cd5a83e772fb)

We can find rise transition, fall transition, cell fall delay, rise fall delay from this plot.

### RISE TRANSITION:
The time output curve takes to rise from 20% of the peak value to 80% of the peak value.

20%: x0 = 2.23842e-09, y0 = 0.660008
80%: x1 = 2.23842e-09, y1 = 2.64016
So, rise transition = x1 - x0 =  2.23842e-09 - 2.23842e-09 = 
### FALL TRANSITION:
The time output curve takes to fall from 80% of the peak value to 20% of the peak value.

80%: x0 = 4.05003e-09, y0 = 2.64
20%: x1 = 4.09287e-09, y1 = 0.660024

So, rise transition = x1 - x0 =  4.09287e-09 - 4.05003e-09

### CELL FALL DELAY:
PROPAGATION DELAY WHEN OUTPUT IS FALLING.
x0 = 4.07481e-09, y0 = 1.65

x0 = 4.05e-09, y0 = 1.65
### CELL RISE DELAY:
PROPAGATION DELAY WHEN OUTPUT IS FALLING:

x0 = 2.20636e-09, y0 = 1.65

x0 = 2.15e-09, y0 = 1.65

