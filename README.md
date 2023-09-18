# Day 1 - Inception of OpenSource EDA ,OPENLANE and SKY130 PDK

![image](https://github.com/vivek8904/pes_pd/assets/130572147/3deab4a6-6194-4cc9-80b5-911c3dcf9ab1)
- Digital ASIC Design - 
- PDK - 
- Open PDK - 
- Open world digital asic design

## Simplified RTL to GDS flow
![image](https://github.com/vivek8904/pes_pd/assets/130572147/cdfbfba6-8703-4e40-8158-dfc29d9a793f)

- Synthesis
- Floor and Power Planning
- Placement
- Clock Tree Synthesis
Routing
Sign Off

## Introduction to OpenLane and Strive Chipsets

Strive SoC Family
**OpenLane ASIC FLow**
![image](https://github.com/vivek8904/pes_pd/assets/130572147/72004c00-fa73-4fc4-aeed-5249ff95ce36)
OpenLane regression testing
Design For Test
Physical Implementation
Logic Equivalence Check
Dealing with antenna rules and regulations
STA

## Labwork
![image](https://github.com/vivek8904/pes_pd/assets/130572147/5605207e-1a7e-4e9b-ab7e-74cb14c0176b)

![image](https://github.com/vivek8904/pes_pd/assets/130572147/23934c64-6a6d-4acf-875a-c465dd651c7e)

![image](https://github.com/vivek8904/pes_pd/assets/130572147/1abcfd46-2e4d-40fa-b747-a971cfe6f737)
![image](https://github.com/vivek8904/pes_pd/assets/130572147/240fd32d-d1b3-4efe-87be-d5b7e7b14acd)
![image](https://github.com/vivek8904/pes_pd/assets/130572147/60a4fef3-2299-489d-9f1c-be556cfda84a)

# Day 2-Chip Foorplanning and Synthesis

*** run_floorplan ***
![image](https://github.com/vivek8904/pes_pd/assets/130572147/817cdee3-fa38-4c43-99ec-0de16fe6d599)

## Library Binding and Placement
![image](https://github.com/vivek8904/pes_pd/assets/130572147/d28a2dac-7fa1-497b-9665-eaa46c474367)

![image](https://github.com/vivek8904/pes_pd/assets/130572147/74de4111-04e5-42aa-b1b5-14c89f90a8b1)

## Cell Design Flow


# day 3 - Design Library Cell

## SPICE Deck creation for CMOS Inverter

SPICE deck contains the information of netlist such as:
- Connectivity Information
- Component values
- 'Nodes' identified
- 'Node' names

![image](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/fde8c66e-6547-49a2-bdad-478c812d5419)

### [CMOS_INVERTER.cir]()

```
*** MODEL DESCRIPTIONS ***
*** NETLIST DESCRIPTION ***
M1 out in vdd vdd pmos W=0.375u L=0.25u
M2 out in 0 0 nmos W=0.375u L=0.25u

cload out 0 10f

Vdd vdd 0 2.5
Vin in 0 2.5
*** SIMULATION Commands ***

.op
.dc Vin 0 2.5 0.05
*** include tsmc_025um_model.mod ***
.LIB "tsmc_025um_models.mod" CMOS_MODELS
.end
```

SPICE Simulation steps
```
cd <folder where the .cir file is present>
source CMOS_INVERTER.cir
run
setplot
dc1
display
plot out vs in
```

Observe the output. It should be symmetric ie., the threshold voltage should be at vdd/2 if it isnt, try to increase the PMOS width and run the simulation again. One of the important parameters tthat defines the **ROBUSTNESS** of the CMOS is ```Switching Threshold (Vm)``` @Vm : Vin = Vout

## Fabrication Process for a CMOS Inverter

Fabrication of CMOS Inverter is a 16-Mask process

### 1. Selecting the substrate 

- P-Type substrate with resistivity around (5-50 ohm) doping level (10^15 cm^-3) and orientation (100).
- Note that substrate doping should be less than well doping (used to fabricate NMOS and PMOS)

### 2. Create active resistance

This step creates pockets for NMOS and PMOS
1. Grow SiO2(~40nm) on Psub
2. deposit ~80nm Si3N4 on SiO2
3. deposit 1um layer of photoresist(used to define regions)
4. photolithography
5. etch out Si3N4 and SiO2 using a suitable solvent
6. Place the obtained structure in oxidartion furnace due to which field oxide is grown.This process is called ```LOCOS``` that is ```Local oxidation of silicon```
7. Etch out Si3N4 using hot phosphoric acid

### 3.NWel and PWel formation

- Apply photoresist, apply mask that covers NMOS
- Expose to UV, Wash, remove mask, appl boron(p-type) using Ion Implantation at an energy of 200Kev(for diffusion)
- repeat it for the other half using phosphorous @400Kev because phosphorous is heavier
- Wells have been created but the depth is low. Therefore subject it to high temperature furnace which increases the well depth.

### 4. Formation of Gate

- We repeat the step 3 but at low energy with p-type implant as boron @60Kev and n-type implant as Arsenic.
- Due to this The SiO2 is damaged as the dopants penetrate through it.
- Therefore original SiO2 is etched out using dilute HF solution and regrown to give high quality oxide(~10 nm thin)
- Finally for the gate to form, apply N-type ion implants for low gate resistance.
- Now mask on small width of Nwell and PWell above SiO2  and perform photolithography
- Gate Formation is Done

### 5. Lighlt Doped Drain Formation(LDD Formation)

- On the surface of SiO2 corresponding to NWell, apply photoresist, mask it, put phosphorous to make N-Implant on p-well(N-)
- Similarly do it for the other side using boron that forms (p-) implant
- This LDD has to be protected from further process
- so, Deposit 0.1um thick SiO2 on full structure and etch out using plasma anisotropic etching that results in formation of side wall spacers..

### 6. Source and Drain Formation

- Mask Nwell structure, deposit arsenic @75KeV that forms an N+ implant on Pwell
- use boron for P+ implant formation on Nwell
- Subject it to high temperature furnace that results in required thickness of N+,P+,N-,P- implants.

### 7. Steps to form contacts and interconnects

- Etch thin SiO2 oxide in HF solution
- Deposit Titanium of wafer surface using sputtering all over the structure
- Wafer heated at 600-700 degree in ambient N2 environment for 60 sec that reults in low resistance TiSi2 where the gate of both MOS is present.
- At the other places, TiN is formed that's used for local communication
- Etch off TiN on and half around gate structure of both MOS using RCA Cleaning

### 8. Higher level metal formation

- On the resulted structure, deposit a thick layer of (1um) SiO2 doped with P/B known as phosphoborosilicate glass
- To make the added surface plain, use CMP (Chemical Metal Polishing)
- For the creation of contact pins, proper holes with contacts have to be made
- This can be done using Al, W and TiN layer depositions.
- Deposit a layer of Si3N4 that acts as dielectric to protect the chip.

### 9. Final STructure

 ![image](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/0e355a75-55ff-4723-96ae-4abd5845697c)

## Inverter Layout using Magic

```
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
magic -T sky130A.tech sky130_inv.mag
```

## Exploring the Layout displayed by MAGIC

Select the specific layer/device by hovering over the object and pressing, s, iteratively, until you traverse the hierarchy to the specified object:

![Screenshot from 2023-09-12 18-15-54](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/1a918a4c-da78-4c9f-b553-e080ddd3e7e7)

- select a region from the layout, go to the console and type ```what``` to display the information of selected area
- To select a region, place ```cursor``` on that point and  press```s```. More the number of times you press ```s```, higher the abstraction selected.

![image](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/fdd5bf6b-3483-4471-9b68-d98fa0b80af3)

refer to [inverter](https://github.com/nickson-jose/vsdstdcelldesign) to create layout for CMOS Inverter

### DRC Check

To check for DRC Errors, select a region (left click for starting point, right click at end point) and see the DRC column at the top that shows how many DRC errors are present.The Details of DRC Errors will be printed on the console.

![image](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/eebc0109-4408-40fa-a18e-ead67419cfa7)

For more information on DRC errors plase refer to: [DRC_Erros](https://skywater-pdk--136.org.readthedocs.build/en/136/)
For more information on how to fix these DRC errors using Magic please refer to: [fix_DRC](http://opencircuitdesign.com/magic/)


## Extracting PEX to SPICE with MAGIC

Select Full inverter layout. Then

![image](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/36c93dc8-6c1e-4ac4-9eac-f2c7a001b82a)

![image](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/e58613be-86ee-4248-8298-ef002274429b)

The above file has details of inverter netlist but the sources and their values are not specified. So we have to modify the file.

- Grid size from the layout is 0.01u
- specify the library for MOS
- create VDD, VSS, Input pulse Va
- specify the type of analysis to be done

### Grid Size

![image](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/1fd94afe-3a40-4269-92ed-d7c46f248417)

## Modified Spice netlist

![Screenshot from 2023-09-14 17-25-22](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/21be1cbb-da31-4409-bf53-a3a80f11ac97)

To run the spice netlist, run ```ngspice sky130_inv.spice``` and ```plot y vs time a```

![image](https://github.com/vivek8904/pes_pd/assets/130572147/87377a0b-47dc-4b8f-ab14-f4496f85ae16)

- Rise Transition : 0.0395ns
- Fall transition : 0.0282ns
- Cell Rise delay : 0.03598ns
- Cell fall delay : 0.0483ns








