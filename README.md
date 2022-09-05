### Advanced-Physical-Design-using-OpenLANE-Sky130
My takeaways from the 5 days workshop
---
## TABLE OF CONTENT
- [Day 1](# Kickstart with open-source EDA, OpenLANE and Google Sky130 PDK)
- [Day 2](# How to floorplan and Introduction to Library cells)
- [Day 3](# library cell in  Magic Layout and ngspice characterization)
- [Day 4](# Pre layout Timing analysis & need of good clock tree)
- [Final steps for RTL2GDS](#Final-steps-for-RTL2GDS)

---


For the Introduction on how to get started with openlane follow
```shell
https://www.udemy.com/course/vsd-a-complete-guide-to-install-openlane-and-sky130nm-pdk/
```

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



# DAY 4 Pre layout Timing analysis & need of good clock tree.

## Things to note before routing
#### The inverter cloned from github earlier has already made the ports but still this can be verified by following the necessary steps
### 1. Input & Outut port must lie on the intersection of vertical & horizontal tracks
![image](https://user-images.githubusercontent.com/96485068/183433685-53fc5fa9-ad53-4fa3-9e34-b7db3f172367.png)

### 2.Widths of standard cell  must be odd multiple of track pitch.
![image](https://user-images.githubusercontent.com/96485068/183443638-218bc46c-7df9-4666-9b48-a6955a193eaa.png)
### 3. Height of standard cell must be odd multiple of track verticle pitch.
![image](https://user-images.githubusercontent.com/96485068/183443985-da16dae1-7e7d-4493-9ffa-7bfec12355cd.png)
### Ensuring the grid are matched according the metal pitch and offset.
![image](https://user-images.githubusercontent.com/96485068/183444218-d40bfda5-4cfa-4b5c-ba5b-21cab7d6ccf6.png)
![image](https://user-images.githubusercontent.com/96485068/183432814-2d2a32bc-edfc-4512-9645-075bf1f92600.png)
To create a input /output port.
![image](https://user-images.githubusercontent.com/96485068/183446813-3b2c5c7b-75ef-493e-9d6c-96d32bbb1f21.png)
### LEF file extracts as pin (ports) which are then usful in routing.
In the magic terminal 
```shell
write sky130_vsdinv.lef
```
Creates
![image](https://user-images.githubusercontent.com/96485068/183444786-17d22e96-1fd6-4ad3-9b50-42c98356e51f.png)
### Copy the LEF file from the mag dir to the working src of picorv32a
### Copy all the standard typical fast and slow type file of sky130 into src of picorv32a.
![image](https://user-images.githubusercontent.com/96485068/183445550-046a0991-5838-4ad8-be66-335e681400f7.png)
The inverter cell has been successfully added to the picorv32a and the same can be verified by the routing rules.
#### In magic to view layout only the LEF prompt.
```shell
expand
```
![image](https://user-images.githubusercontent.com/96485068/183449311-6b15d298-dd69-4eeb-afd9-62794e33ef2d.png)

Invoke the openlane run the same synthesis on existing file created before by
```shell
docker
./flow.tcl interactive
package require openlane 0.9
prep -design picorv32a -tag <workingfolder> -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

```
![image](https://user-images.githubusercontent.com/96485068/183448595-ea9cb64c-b8df-4567-aa17-e925052ad64d.png)
To run floorplan, placement,routing and cts follow the below hint.
![image](https://user-images.githubusercontent.com/96485068/183447903-77163287-faec-4ff3-a8a4-ae99b61d3303.png)
![image](https://user-images.githubusercontent.com/96485068/183449894-7c3c53bb-62de-4a61-ae91-e5c1893b2a8c.png)
### STA analysis
In the openlane , open openroad to run the sta with real clock
and edit by adding higher order buffer for less slack
```shell
replace_cell <cell number> <celltype_>
```
example 
```shell
replace_cell _13361_ sky130_fd_sc_hd__buf_16
```
replaces buffer of any size to 16
report

slack can be reduced by increasing the area and by changing the same in the base.sdc file
![image](https://user-images.githubusercontent.com/96485068/183451316-df94483a-f608-413d-aaa4-c6a4ee48030f.png)



# Day 5 Final steps for RTL2GDS
The routed files and magic layout are
![image](https://user-images.githubusercontent.com/96485068/183453223-04fe19ae-fc5f-4bd1-b06d-54c8f4153772.png)

Earlier back in 2020 the openlane had the following steps
```shell
package require openlane 0.9
prep -design <design> -tag <tag> -overwrtie
run_synthesis
run_floorplan
run_placement
run_cts
gen_pdn
run_routing
```

New version steps, are as follows
```shell
package require openlane 0.9
prep -design <design> -tag <tag> -overwrtie
run_synthesis
init_floorplan
place_io
global_placement_or
detailed_placement
tap_decap_or
detailed_placement
run_cts
gen_pdn
run_routing
```



















# References
  - RISC-V: https://riscv.org/
  - VLSI System Design: https://www.vlsisystemdesign.com/
  - OpenLANE: https://github.com/The-OpenROAD-Project/OpenLane

# Acknowledgement
  - [Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.
  - [Nickson Jose](https://github.com/nickson-jose)
