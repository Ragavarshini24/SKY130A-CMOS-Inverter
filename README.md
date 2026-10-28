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
<img width="763" height="411" alt="Inverter_schematic" src="https://github.com/user-attachments/assets/0e66cf4c-e29d-4b37-9a94-36552a44e469" />

Convert the schematic to a symbol.Using the symbol, we can create an independent test bench to simulate the circuit.   
<img width="803" height="387" alt="Inverter_schem" src="https://github.com/user-attachments/assets/4b378a55-c9ac-46e2-b059-3b6363c36d07" />
## Creating and simulating testbench Schematic   
The circuit can be simulated in ngspice.  
<img width="872" height="445" alt="Schematic_simulation(ip vs op)" src="https://github.com/user-attachments/assets/a6c6f7f1-b2ab-4e82-99aa-ad5f20525b68" />

## Creating inverter layout in Magic and exporting its netlist   
The original schematic can be used to export a netlist, which can be imported into magic to create the layout.    
- Layout imported from spice file: file -> import spice
<img width="662" height="383" alt="spice2mag" src="https://github.com/user-attachments/assets/0d95102b-18c0-414c-bc3c-354376e6c7e9" />

- Layout after connecting pins
<img width="572" height="301" alt="final_layout" src="https://github.com/user-attachments/assets/9806fe8e-2f1a-4dab-976f-7410262add03" />

- Inverter layout
<img width="794" height="442" alt="inv_layout" src="https://github.com/user-attachments/assets/a743ce14-450c-46fb-b6db-a0bfcd207f81" />                                           
## DRC - Design Rule Check            
Inside the Magic console: 
```

$  drc check     
$  drc count     
$  drc why

```




