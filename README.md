# SRAM

This reposistory will give brief idea on 1024 X 32 SRAM IP Design using OpenRAM compiler .


# Table of contents

- [Required Specifications](#required-specifications)
- [6T-CELL](#6t-cell)
- [Modes of operation of sram](#modes-of-operation-of-sram)
  - [**1. Hold mode :**](#1-hold-mode-)
    - [SNM curve](#snm-curve)
  - [**2. Read mode :**](#2-read-mode-)
    - [Precharge circuit :](#precharge-circuit-)
    - [Sense amplifier :](#sense-amplifier-)
  - [**3. Write mode :**](#3-write-mode-)
    - [Write Driver :](#write-driver-)
- [IP usage](#ip-usage)
  - [Ngspice for Simulation](#ngspice-for-simulation)
  - [Steps to use Ngspice](#steps-to-use-ngspice)
  - [For Simulation of this IP](#for-simulation-of-this-ip)
    - [To Run Simulation](#to-run-simulation)
- [Future work](#future-work)
- [Author](#author)
- [Acknowledgements](#acknowledgements)
- [Contact Information](#contact-information)

## Required Specifications

 * Size : 4KB (1024 x 32)
 * Power Supply : 1.8V
 * Access time < 2.5ns 
 
## 6T-CELL 

<img align="center" width="1000"  src="/Circuits/6tsramcell.png">

## Modes of operation of sram 

### **1. Hold mode :** 
WL=0 ==> Access transistors M1,M6 will be off . The circuits preserves one of two stable operating points . Therefore, data is held .
#### SNM curve 

<img align="center" width="1000"  src="/Waveforms/SNM_CURVE.png">

Calculated SNM value for above curve is 0.59 .

### **2. Read mode :**
WL=1 ==> Aceess transistors will be on.
For Read '1': Q = 1 and Q = 0 Bit lines are precharged to Vdd .In this operation M2 , M3 off . Voltage of BL bar starts decreasing. Both bitlines are connected to data
read circuitry and small difference is detected and amplified as logic'1'.
Similarly read '0' operation is done .

Note : Condition for cell stability in read operation  is Kpdn > Kaccess  i.e CR (CELL RATIO) should be grater than 1 .

#### Precharge circuit :
<img align="center" width="1000"  src="/Circuits/precharge.png">

Simulated waveform :

<img align="center" width="1000"  src="/Waveforms/precharge_circuit(pre,bl.br).png">

#### Sense amplifier :  

Here sen is active-0 read enable signal .

<img align="center" width="1000"  src="/Circuits/sense_amp.png">

Simulated waveforms :

<img align="center" width="1000"  src="/Waveforms/sense_inputs.png">
<img align="center" width="1000"  src="/Waveforms/sense_out.png">

### **3. Write mode :**
WL=1 ==> Aceess transistors will be on .
Write '0 ': Initially Q=1 and Q = 0. Here BL is forced to 0 and this causes M4 and M6 transistors conduct .
When Q less than Vth of M1 then M1 gets off. Qb becomes high as PMOS(M2) connected toVdd will be ON and Q becomes low i.e logic'0'. That i.e write operation is done by forcing bitlines bl and br , which is done using write driver circuit .

Note : Condition for cell write completion is Kaccess > Kpup . 

#### Write Driver :

Here we is active-0 write enable signal .

<img align="center" width="1000"  src="/Circuits/writedrivers_schematic.png">

Simulated waveforms:

<img align="center" width="1000"  src="/Waveforms/writedriver_inputs.png">
<img align="center" width="1000"  src="/Waveforms/writedriver_out_bl.png">
<img align="center" width="1000"  src="/Waveforms/writedriver_output_br.png">

## IP usage 
The user of this IP has to install Ngspice (Open source Circuit Simulator)
### Ngspice for Simulation
**For Ubuntu **
Open terminal and type the following command to install Ngspice 
```
$ sudo apt-get install -y ngspice
```
### Steps to use Ngspice

To enter the Ngspice Shell, open the terminal & type :
```
$ ngspice
```
To simulate a netlist, type :
```
ngspice 1 ->  source <filename>.cir
```

You can exit from the Ngspice Shell by typing :
```
ngspice 1 ->  exit
```
or
 
```
ngspice 1 ->  quit
```

There are several waveforms that need to be obtained to observe the performance of the Bandgap reference circuit .

### For Simulation of this IP

To clone the Repository and download the Netlist files for Simulation, enter the following commands in your terminal .

```
$  sudo apt install -y git
$ git clone https://github.com/aishwaryapenumarthi/SRAM.git
$ cd SRAM/Spice_lib
```
#### To Run Simulation 
**Example :** To run simulations of trigate circut , type the below command 
```
$ ngspice tri_gate.cir.out
```
Which gives you following waveforms :

<img align="center" width="1000"  src="/Waveforms/trigate_inputs.png">
<img align="center" width="1000"  src="/Waveforms/trigate_output.png">

## Future work

1.Magic layouts of above subcircuits are need to be done . 

2.OpenRam compiler will be used , which aids memory design . For Openram compiler, spice netlist, magic files, and gds for each and every block (i.e subcircuit) must be provided . Therefore, .mag, .gds files of all subcircuits should be generated .

3.The setup script and technology directory for osu018  has to be created . 

## Author

 * Penumarthi Aishwarya
 
## Acknowledgements

  * Kunal Ghosh Director VSD Corp.Pvt.ltd
  * Philipp Guhring Software Architect at LibreSilicon Association
  
## Contact Information 

- PENUMARTHI AISHWARYA
 M.tech Embedded Systems, NIT Jamshedpur
  aishwarya.penumarthi@gmail.com
- KUNAL GHOSH 
 Director, VSD Corp. Pvt. Ltd. 
  kunalpghosh@gmail.com
- PHILIPP GÜHRING 
Software Architect at LibreSilicon Association
  pg@futureware.at

 
