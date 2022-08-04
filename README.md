### Advanced-Physical-Design-using-OpenLANE-Sky130
My takeaways from the 5 days workshop
## DAY 1 Kickstart with open-source EDA, OpenLANE and Google Sky130 PDK
>Invoking the Openlane 
 cd openlane_working_dir/openlane
 docker                   call off(docker run it -v $(pwd):/openLANE_flow -v $PDK_ROOT: $PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER): $(id -g $USER) openlane:rc2)
 bash-4.2$ pwd
 ./flow.tcl -interactive
 OPENLANE
 %package require openlane 0.9
 %prep -design picorv32a
 (to create run1 ) %prep -design picorv32a -tag first _run1
 run_synthesis
 run_floorplan
 run_placement
 ! [less floorplan.tcl]()
 
