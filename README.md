# Physical-Verification-using-SKY130
## 1.Project Overview  
This repository documents my progress in the SKY130 Physical Verification Lab, focusing on the design and verification of a CMOS inverter using open-source VLSI tools.
The lab uses the SkyWater 130 nm Process Design Kit (PDK) to understand the basic design flow, schematic creation, circuit simulation, layout design, Design Rule Checking (DRC), and Layout Versus Schematic (LVS) verification.
## 2.Objectives
- Understand the SKY130 PDK and CMOS inverter operation.  
- Set up and verify the required open-source EDA tools.  
- Create a CMOS inverter schematic using Xschem.  
- Perform pre-layout simulation using Ngspice.  
- Design the inverter layout using Magic.  
- Perform DRC and LVS checks.  
- Study post-layout simulation and parasitic extraction. 
## 3. Tools and Environment
- **Operating System:** Ubuntu/Linux  
- **Technology:** SkyWater SKY130A  
- **Schematic Editor:** xschem  
- **Layout Editor:** Magic  
- **Circuit Simulator:** Ngspice  
- **Verification Tool:** Netgen 

## Getting the tools
To adequately utilize the open source skywater130 pdk and understand the design flow, we first require to install all the tools, which are  
open_pdk  
- magic  
- ngspice  
- xscheme  
- netgen  
<img width="1023" height="541" alt="toolchains" src="https://github.com/user-attachments/assets/33398e76-fa58-4ff8-8043-f18d27026a55" />

## Magic  
Magic is an open-source VLSI layout tool.  
Install steps:
```
$  git clone git://opencircuitdesign.com/magic
$  cd magic
$	 ./configure
$  make
$  sudo make install
```
## Netgen  
Netgen is a tool for comparing netlists, a process known as LVS, which stands for "Layout vs. Schematic"   
Install steps:  
```
$  git clone git://opencircuitdesign.com/netgen
$  cd netgen
$	./configure
$  make
$  sudo make install
```
## Xschem  
Xschem is a schematic capture program  
Install steps   
```
$  git clone https://github.com/StefanSchippers/xschem.git xschem_git
$	./configure
$  make
$  sudo make install
```
## Ngspice
ngspice is the open-source spice simulator for electric and electronic circuits.  
Install steps:   
After downloading the tarball from  
https://sourceforge.net/projects/ngspice/files/ to a local directory, unpack it using:  
```
 $ tar -zxvf ngspice-37.tar.gz
 $ cd ngspice-37
 $ mkdir release
 $ cd release
 $ ../configure  --with-x --with-readline=yes --disable-debug
 $ make
 $ sudo make install
```
Please note that to view the simulation graphs of ngspice, xterm is required and can be installed using.   
```
$ sudo apt-get update
$ sudo apt-get install xterm
```
## open_pdk
Open_PDKs is distributed with files that support the Google/SkyWater sky130 open process description https://github.com/google/skywater-pdk. Open_PDKs will set up an environment for using the SkyWater sky130 process with open-source EDA tools and tool flows such as magic, openlane, netgen,etc.   
Install steps:   
```
$  git clone git://opencircuitdesign.com/open_pdks
$  open_pdks
$	./configure --enable-sky130-pdk
$  make
$  sudo make install
```
Now that we have all the necessary tools installed let's understand the design flow!   
## Understanding the design flow
## Verifiying the open_pdk installation
Checking if Magic works

<img width="1920" height="1080" alt="Magic vlsi layout tool" src="https://github.com/user-attachments/assets/dee1cfb3-1491-4d01-9544-f93b30296397" />


Checking if Xschem works

<img width="1920" height="1080" alt="Xschem_launch" src="https://github.com/user-attachments/assets/fa911b45-44d3-4512-b2d1-3c42ffcda9d9" />

## Creating inverter schematic using xschem
An initial schematic is made by placing components from the open_pdk library
The required changes to the properties of the device can be made here and will automatically reflect in the layout  
<img width="1920" height="1080" alt="Screenshot 2026-10-27 161059" src="https://github.com/user-attachments/assets/d82bde59-0f9b-4338-9341-00962fc483e5" />

## Cmos Inverter Symbol      
Convert the schematic to a symbol. A custom symbol is created in Xschem for the CMOS inverter subcircuit, with VDD, VIN, VOUT, and VSS pins for testbench integration.           
<img width="1920" height="1080" alt="Screenshot 2026-10-28 184043" src="https://github.com/user-attachments/assets/3eeb203e-5687-47da-bb0e-289026403f1d" />


Using the symbol, we can create an independent test bench to simulate the circuit.   
<img width="1920" height="1080" alt="Screenshot 2026-10-27 164553" src="https://github.com/user-attachments/assets/16fffcba-d832-4b38-827e-cde865135c04" />

## Creating and simulating testbench Schematic   
The circuit can be simulated in ngspice.  
<img width="872" height="445" alt="Schematic_simulation(ip vs op)" src="https://github.com/user-attachments/assets/a6c6f7f1-b2ab-4e82-99aa-ad5f20525b68" />

## Creating inverter layout in Magic and exporting its netlist   
The original schematic can be used to export a netlist, which can be imported into magic to create the layout.    
- Layout imported from spice file: file -> import spice
<img width="662" height="383" alt="spice2mag" src="https://github.com/user-attachments/assets/0d95102b-18c0-414c-bc3c-354376e6c7e9" />

- Layout setup
<img width="1672" height="941" alt="layout setup" src="https://github.com/user-attachments/assets/9622ceaa-eebc-423d-aa60-21a79af3a76e" />

- Inverter layout
<img width="1672" height="941" alt="inverter layout" src="https://github.com/user-attachments/assets/98cc99e2-fe42-4e00-b9ef-2d50cb017d54" />

             
## DRC - Design Rule Check               
Inside the Magic console:      
```     
$ drc check         
$ drc count                 
$ drc why          
```
Result:
```
$ Drc errors
```
CMOS inverter physical layout in Magic using the Sky130A technology, with DRC verification showing zero errors.  

 <img width="1681" height="936" alt="TkCon Console Over IC Layout (1)" src="https://github.com/user-attachments/assets/e3dbbdfd-bb1d-4087-9a5c-777737195f5b" />
 
 ## Extraction from Magic

Inside the Magic Console:

```bash
extract all
extract2spice lvs
extract2spice
```
## LVS — Layout vs Schematic
LVS is performed using Netgen to verify that the physical layout matches the original circuit schematic. For this comparison, the SPICE netlist generated from Xschem is compared with the SPICE netlist extracted from the Magic layout to check whether the devices and electrical connections match.   
``` bash
cd ../netgen
netgen -batch lvs "../mag/inverter.spice inverter" "../xschem/inverter.spice inverter"
```
<img width="1671" height="840" alt="lvs" src="https://github.com/user-attachments/assets/2e62da1f-616a-4806-b80b-1fb4774eb1ac" />

