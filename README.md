# SRAM

This reposistory will give brief idea on 1024 X 32 SRAM IP Design using OpenRAM compiler .


# Table of contents

- [SRAM](#sram)
- [Table of contents](#table-of-contents)
  - [Desired Specifications](#desired-specifications)
  - [6T-CELL](#6t-cell)
  - [Modes of operation of sram](#modes-of-operation-of-sram)
    - [**1. Hold mode :**](#1-hold-mode-)
    - [**2. Read mode :**](#2-read-mode-)
    - [**3. Write mode :**](#3-write-mode-)
  - [Block diagram for for 1-bit SRAM](#block-diagram-for-for-1-bit-sram)
    - [Simulated waveforms :](#simulated-waveforms-)
  - [IP usage](#ip-usage)
    - [Ngspice for Simulation](#ngspice-for-simulation)
    - [Steps to use Ngspice](#steps-to-use-ngspice)
    - [For Simulation of this IP](#for-simulation-of-this-ip)
    - [Layout using Magic](#layout-using-magic)
    - [To View Magic Layouts](#to-view-magic-layouts)
    - [Layouts](#layouts)
  - [Future work](#future-work)
  - [Author](#author)
  - [Acknowledgements](#acknowledgements)
  - [Contact Information](#contact-information)
## Desired Specifications

 * Size : 4KB (1024 x 32)
 * Power Supply : 1.8V
 * Access time < 2.5ns 
 
 For more details check [this](mem4kBytesor32kbitsSpec.pdf)
 
## 6T-CELL 

<img align="center" width="900"  src="/Circuits/6tsramcell.png">

## Modes of operation of sram 

### **1. Hold mode :** 
WL=0 ==> Access transistors M1,M6 will be off . The circuits preserves one of two stable operating points . Therefore, data is held .
#### SNM curve 

<img align="center" width="900"  src="/Prelayout_Simulations/SNM_CURVE.png">

Calculated  SNM value for above curve is  0.59 .

### **2. Read mode :**
WL=1 ==> Aceess transistors will be on.
For Read '1': Q = 1 and Q = 0 Bit lines are precharged to Vdd .In this operation M2 , M3 off . Voltage of BL bar starts decreasing. Both bitlines are connected to data
read circuitry and small difference is detected and amplified as logic'1'.
Similarly read '0' operation is done .

**Note**  : Condition for cell stability in read operation  is Kpdn > Kaccess  i.e CR (CELL RATIO) should be grater than 1 .

#### Precharge circuit :

<img align="center" width="900"  src="/Circuits/precharge.png">

##### Simulated waveforms :

<img align="center" width="900"  src="/Prelayout_Simulations/precharge_circuit(pre,bl,br).png">

#### Sense amplifier :  

Here sen is active-0 read enable signal .

<img align="center" width="900"  src="/Circuits/sense_amp.png">

##### Simulated waveforms :

<img align="center" width="900"  src="/Prelayout_Simulations/sense_inputs.png">
<img align="center" width="900"  src="/Prelayout_Simulations/sense_out.png">

### **3. Write mode :**
WL=1 ==> Access transistors will be on .
Write '0 ': Initially Q=1 and Q = 0. Here BL is forced to 0 and this causes M4 and M6 transistors conduct .
When Q less than Vth of M1 then M1 gets off. Qb becomes high as PMOS(M2) connected toVdd will be ON and Q becomes low i.e logic'0'. That i.e write operation is done by forcing bitlines bl and br , which is done using write driver circuit .

**Note :** Condition for cell write completion is Kaccess > Kpup . 

#### Write Driver :

Here we is active-0 write enable signal .

<img align="center" width="900"  src="/Circuits/writedrivers_schematic.png">

##### Simulated waveforms:

<img align="center" width="900"  src="/Prelayout_Simulations/writedriver_inputs.png">
<img align="center" width="900"  src="/Prelayout_Simulations/writedriver_out_bl.png">
<img align="center" width="900"  src="/Prelayout_Simulations/writedriver_output_br.png">


## Block diagram for for 1-bit SRAM

<img align="center" width="900"  src="/Circuits/block diagram.JPG">

### Simulated waveforms : 
<img align="center" width="900"  src="/Prelayout_Simulations/Integrated_circuit_in_out.png">
<img align="center" width="900"  src="/Prelayout_Simulations/bl_br_integrated_circuit.png">



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
ngspice 1 ->  source <filename>.cir.out
```

You can exit from the Ngspice Shell by typing : 
```
ngspice 1 ->  quit
```
### For Simulation of this IP

To clone the Repository and download the Netlist files for Simulation, enter the following commands in your terminal .

```
$  sudo apt install -y git
$ git clone https://github.com/aishwaryapenumarthi/SRAM.git
$ cd SRAM/Postlayout_spice_files
```
#### To Run Simulation 
**Example :** To run simulations of trigate circuit , type the below command 
```
$ ngspice tristate.cir.out
```
Which gives you following waveforms :

<img align="center" width="900"  src="/Prelayout_Simulations/tristate_inputs.png">
<img align="center" width="900"  src="/Prelayout_Simulations/tristate_output.png">

### Layout using Magic

*For ubuntu linux*

Download Magic from this [link](http://opencircuitdesign.com/magic/)


```html
Open Terminal and type below commands. 
cd Downloads/
tar xzf magic-7.5.232.tgz
cd magic-7.5.232/
sudo apt-get install m4
sudo apt-get install tcl-dev
sudo apt-get install tk-dev
sudo apt-get install blt
sudo apt-get install freeglut3
sudo apt-get install libglut3
sudo apt-get install libgl1-mesa-dev
sudo apt-get install libglu1-mesa-dev
./configure
sudo apt-get install csh
sudo make
sudo make install
```
Or

Install Qflow & It will install all necessary open source tools including magic.

Download qflow from this [link](http://opencircuitdesign.com/qflow/)

```html
Open Terminal and type below commands. 
cd Downloads/
tar xzf qflow-1.4.83.tgz
sudo apt-get update
sudo apt-get install qflow
```


### To View Magic Layouts

1. Open the repository and change directory to `cd VSDRAM/mag_libs`

2. To view the layout, type on terminal : `magic -T sample6m.tech <filename>.mag` 

3. Examples : `magic -T sample6m.tech precharge.mag` , `magic -T sample6m.tech sense.mag`

3. To extract the spice netlist : Go to Tkcon window and type these commands.

`extract all`
`ext2spice cthresh 0 rthresh 0`
`ext2spice`

### Layouts 

#### 6T SRAM CELL

<img align="center" width="900"  src="/LAYOUT/sramcell.png">

For postlayout simulated waveform (read operation) click [here](/Postlayout_Simulations/sramcell_read.png)

For postlayout simulated waveform (write operation) click [here](/Postlayout_Simulations/sramcell_write.png)

#### Precharge 

<img align="center" width="900"  src="/LAYOUT/precharge.png">

For postlayout simulated waveform click [here](/Postlayout_Simulations/precharge.png)

#### Sense 

<img align="center" width="900"  src="/LAYOUT/sense.png">

For postlayout simulated waveform click [here](/Postlayout_Simulations/sense.png)

#### Trigate 

<img align="center" width="900"  src="/LAYOUT/tristate.png">

For postlayout simulated waveform click [here](/Postlayout_Simulations/tristate.png)

#### Write Driver

<img align="center" width="900"  src="/LAYOUT/writedriver.png">

For postlayout simulated waveform click [here](/Postlayout_Simulations/writedriver.png)

#### D-FLIP FLOP

<img align="center" width="900"  src="/LAYOUT/dflipflop.png">

For postlayout simulated waveform click [here](/Postlayout_Simulations/dflipflop.png)

#### Integrated circuit

<img align="center" width="900"  src="/LAYOUT/Integrated.png">

For postlayout simulated waveform click [here](/Postlayout_Simulations/Integrated.png)
 
## Future work

The setup script and technology directory for osu018  has to be created in OpenRAM compiler.

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

 
