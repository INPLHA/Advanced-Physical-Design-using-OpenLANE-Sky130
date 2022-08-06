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



