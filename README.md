### Advanced-Physical-Design-using-OpenLANE-Sky130
My takeaways from the 5 days workshop
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





