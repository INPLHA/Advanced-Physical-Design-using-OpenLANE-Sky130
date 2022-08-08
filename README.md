### Advanced-Physical-Design-using-OpenLANE-Sky130
My takeaways from the 5 days workshop
Browse throught he README by selecteing the little icon on 


# DAY 1 Kickstart with open-source EDA, OpenLANE and Google Sky130 PDK

Introduction to Openlane 

## Invoking Openlane


```bash
$docker
```  
Invokes
```bash
 docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) openlane:rc6
```


Directly Used command on the terminal to start Openlane are

```bash
/openlane$docker
bash-4.2$ ./fow.tcl -interactive
%package require openlane 0.9
%prep -design picorv32a
```
 ![image](https://user-images.githubusercontent.com/96485068/183236344-b2060234-5fe0-4bcc-99ef-b253a38b2100.png)

## To run synthesis 


```bash
run_synthesis
```
![image](https://user-images.githubusercontent.com/96485068/183236949-e6f43906-f80a-4f37-8469-2ca4ed9cd9cc.png)

## Synthesis results 
![synthesis results](https://user-images.githubusercontent.com/96485068/183237144-473033ef-7c09-4a72-a6e3-e06b3abdd678.jpeg)
### Number of cells - 14876 
### Total Number of flops - 1613
### Flop count = Total Number of flops/ Number of cells =0.1084 
### ~ 10.84% of flops used .
### Buffer count - 1656
### Buffer count = No. of buffer/Number of cell  = 0.1113 
### ~ 11.13 % buffer used.

# DAY 2 How to floorplan and Introduction to Library cells

## Defining height and width of die and core
### Utilization factor =Area occupied by netlist/ Total area of core
#### Utilization factor is kept about 0.6 to 0.5 or even lower    (why? for future scope of adding cells into it)
### Aspect ratio = Heightof die/ Width of die


```bash
$docker
```  
Invokes
```bash
 docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) openlane:rc6
```
In the OPENLANE terminal run floorplan
```bash
run_floorplan
```
### Successful floorplanning results in a .def file as output. 
### This file contains the die area and placement of standard cells.
### DIEAREA in microns is 660.685x 671.405 = 443587.2
![image](https://user-images.githubusercontent.com/96485068/183238023-e7863a31-c3df-4d8c-9b5f-157daab06856.png)


## Floorplan
### Proper mapping of Library and tech file is important to reflect the same in magic layout
  ### Magic Layout Tool is used for visualizing the layout after  successfull floorplan. 
  ### Three files are required for viewing the layout
  ### 1. Technology File (sky130A.tech) 2. Merged LEF file (merged.lef) 3. DEF File
  /home/kunalg123/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

![image](https://user-images.githubusercontent.com/96485068/183238196-b9aaecd6-bf2e-4e46-95d9-db6d206d0580.png) 
![image](https://user-images.githubusercontent.com/96485068/183238486-ba48bf37-fd6e-493b-9d7b-1d64571f6296.png)
![image](https://user-images.githubusercontent.com/96485068/183238312-8ea05dcc-bc4d-4e4a-8ee7-7030eebebf10.png)
![image](https://user-images.githubusercontent.com/96485068/183238341-64396f50-1b8e-4acb-a2d7-dcd28dfc4501.png)

## Placement
After successfull synthesis and floorplan.
In the Openlane terminal type
```bash
run_placement
```
![image](https://user-images.githubusercontent.com/96485068/183238416-46c19463-81bc-45c6-84cf-ac83ff09f5f7.png)
![image](https://user-images.githubusercontent.com/96485068/183238527-c1c88c5f-9f65-4f14-9fe3-b13c2d37b89b.png)



# DAY 3 library cell in  Magic Layout and ngspice characterization

 open floorplan by 
```bash

 less floorplan.tcl
 ```

![image](https://user-images.githubusercontent.com/96485068/183238687-02e90a3a-1035-498b-bf21-3be740ce212e.png)
Copy    ::env (FP IO MODE)
In openlane terminal
```bash
set ::env (FP IO MODE) 2
```

![image](https://user-images.githubusercontent.com/96485068/183238750-cb3a8900-7ca9-4e00-9b4b-76b007a135af.png)

```bash
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```
## Get the open sourced inverter layout. 
![image](https://user-images.githubusercontent.com/96485068/183238830-d7fcf16f-8fd9-4c36-9871-25b5f0d46c84.png)

![image](https://user-images.githubusercontent.com/96485068/183238889-21fd6659-02ed-4f19-aac0-af0a4bd0a940.png)


### In the tkcon terminal of magic
```bash
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```
extracts all the parasitics
The spice file created will be deafault, change with proper node naming.
![image](https://user-images.githubusercontent.com/96485068/183239020-1bf0f936-32ad-4280-b0b5-990e01cee4a6.png)
```bash ngspice sky130_inv.spice
```
![image](https://user-images.githubusercontent.com/96485068/183239069-eee3c1e4-66ea-48e5-8275-df392be47bcf.png)
### Rise transition=64ps
### Cell rise delay about 60.8ps
### Fall transision =43ps
### Cell fall delay =28ps


DAY 4























# References
  - RISC-V: https://riscv.org/
  - VLSI System Design: https://www.vlsisystemdesign.com/
  - OpenLANE: https://github.com/The-OpenROAD-Project/OpenLane

# Acknowledgement
  - [Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.
  - [Nickson Jose](https://github.com/nickson-jose)
