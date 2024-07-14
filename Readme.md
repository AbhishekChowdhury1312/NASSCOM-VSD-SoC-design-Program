# Day 1:
##Topics covered:
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


## Command to call all the required packages
`package require openlane 0.9`

## Prepare all the required files to run the tool commands:  
`prep -design picorv32a  

## Run Synthesis  
run_synthesis  
## Day 1 TASK:
### Find out percentage of D Flipflop:  
percentage of d flip-flops = number of D flip-flops / total number of cells * 100  
find out both the numbers of flip-flops and total cells from yosys_stat report; also it can be found from the generated text after running `run_synthesis` command in the openlane environment.  
Now use the previous equation to find out the percentage of flip-flops present in the design.  




# Day 2
