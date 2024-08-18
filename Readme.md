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
`package require openlane 0.9`

![Screenshot 2024-07-11 155641](https://github.com/user-attachments/assets/ace56d0f-263e-409f-b74f-19981deb8dd0)

## Prepare all the required files to run the tool commands:  
`prep -design picorv32a`  

![Screenshot 2024-07-11 160543](https://github.com/user-attachments/assets/8fc8e27a-00cf-43ca-a14f-2339ce541ceb)

## Run Synthesis  
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


## Run Floorplan  
`run_floorplan`  

## To view floorplan
`magic -T (tech_file path) lef read (lef_file path) def read (floorplan.def_file path)`



![Screenshot 2024-07-15 134743](https://github.com/user-attachments/assets/50c9420c-49a1-4e54-aca3-3030a3c1ee9b)


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


