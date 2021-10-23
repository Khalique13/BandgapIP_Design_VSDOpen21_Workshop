# Bandgap IP design using Sky130nm PDKs 

![image](https://user-images.githubusercontent.com/80625515/138239041-24971c6d-a864-47be-9bf5-1d2d2a1e4b9c.png)

## Table of Contents 

- [Introduction](#introduction)
- [Types of BGR](#types-of-bgr)
- [Self-Biased Current Mirror Based BGR](#self-biased-current-mirror-based-bgr)
- [Self-bias Current Mirror](#self-bias-current-mirror)
- [CTAT Voltage Generation](#ctat-voltage-generation)
- [PTAT Voltage Generation](#ptat-voltage-generation)
- [Reference Branch](#reference-branch)
- [Start-up](#start-up)
- [Bandgap Referene Circuit](#bandgap-reference-circuit)
- [Analog Design Flow](#analog-design-fow)
    - [Schematic design](#schematic-design)
    - [Pre-layout Simulation](#pre-layout-simulation)
    - [Layout](#layout)
- [Acknowledgement](#acknowledgement)
- [Author](#author)


## Introduction

- Temperature-independent voltage reference circuit widely used in Integrated Circuits.
- Produces constant voltage regardless of power supply variation, temp. changes and circuit loading.
- Output voltage of 1.2v (close to the bandgap energy of silicon at 0 deg kelvin)
- All applications starting from analog, digital, mixed-mode, RF, and SOCs

![Screenshot from 2021-10-21 09-16-02](https://user-images.githubusercontent.com/80625515/138239326-21775953-80a9-4241-9fd3-71154af78744.png)


### Why BGR

- A battery is unsuitable for use as a reference voltage source.
    - voltage drops over time
- A typical power supply is also not suitable.
    - noisy output and residual ripple
- A voltage reference IC used buried Zener Diode.
    - Discrete design required additional components and high-frequency filtering circuits due to higher thermal noise.
    - low voltage Zener diode is not available
- A Bandgap reference that can be integrated into bulk CMOS, Bi-CMOS, or Bipolar technologies without the use of external components.

### Applications of BGR

- Low dropout reglators (LDO)
- DC-to-DC buck converters
- Analog-to-Digital Converter (ADC)
- Digital-to-Analog Converter (DAC)
![Screenshot from 2021-10-21 09-29-39](https://user-images.githubusercontent.com/80625515/138239732-8cd44bbd-610a-4ef4-847a-5a2aff7a2e3e.png)
![Screenshot from 2021-10-21 09-30-07](https://user-images.githubusercontent.com/80625515/138239917-b399727b-f27a-4bf2-878d-3bd0d8fddaff.png)


## Types of BGR

### Architecture wise BGR can be designed in two ways,

1. Using a self-biased current mirror
2. Using Operational- Amplifier

### Application wise BGR can be categorized as,

1. Low-voltage BGR
2. Low-power BGR
3. High-PSRR and low-noise BGR
4. Curvature compressed BGR

## Self-Biased Current Mirror Based BGR

![Screenshot from 2021-10-21 14-26-29](https://user-images.githubusercontent.com/80625515/138246546-afb36741-1354-4f57-8860-4a7177c3e82b.png)

### Advantages

- Simplest topology
- Easy to design
- Always stable

### Limitations

- Low power supply rejection ratio (PSRR)
- Cascode design needed to reduce PSRR
- Voltage head-room issue
- Need start-up circuit



## Components of BGR
![Screenshot from 2021-10-21 09-30-26](https://user-images.githubusercontent.com/80625515/138255222-5d35cce4-63e1-435d-a46a-88d4212473b9.png)

![Screenshot from 2021-10-21 09-30-46](https://user-images.githubusercontent.com/80625515/138255278-9881ef63-260c-4a08-bf70-57efde49273d.png)

- CTAT voltage generation circuit
- PTAT voltage generation circuit
- Self-biased current mirror circuit
- Reference branch circuit
- Start-up circuit

## CTAT Voltage Generation

It is the most important component of BGR. 
![Screenshot from 2021-10-21 09-48-01](https://user-images.githubusercontent.com/80625515/138251461-7478b748-547c-4971-9585-02e92d23918a.png)

![Screenshot from 2021-10-21 09-49-27](https://user-images.githubusercontent.com/80625515/138254089-fc192e5b-f62b-4bc2-9679-1b2b7324cc31.png)

![Screenshot from 2021-10-21 09-51-33](https://user-images.githubusercontent.com/80625515/138254265-812278c6-b682-4f8e-b573-36862b3c2fc4.png)


## PTAT Voltage Generation

![ptat](https://user-images.githubusercontent.com/80625515/138248730-24ebc53a-ac9c-4419-80c0-e366adb2cd2e.png)

![Screenshot from 2021-10-21 10-04-20](https://user-images.githubusercontent.com/80625515/138249213-55db4013-8d0b-4f41-b925-ea0d439cae4a.png)


### Design of R1 Resistance 

- R1 completely depends upon the power consumption and silicon area budget.
- R1 = Vt 1n (N)/I
- Resistance decreases, so area decreases with the increase of circuit current.
- Resistance increases, so area increases with the decrease of circuit current.
- Also the resistance value depends on the number of BJT used in branch 2.
i.e, for 10uA current with N=8, R1 calculated to be 5.4k Ohm

## Self-bias Current Mirror

### Current Mirror & its Issues

The objective of reference generation is to establish DC voltage or current,
- Independent of supply
- Independent of process and
- Well-defined behavior with temperature

![Screenshot from 2021-10-21 12-11-51](https://user-images.githubusercontent.com/80625515/138248202-ea25c2f0-d3df-47bb-8ddf-2c0e7cc0891d.png)

#### Issues:

- Output current of this circuit quite sensitive to VDD
- For a less sensitive solution, the circuit must bias itself, i.e Iref must be somehow derived from Iout

### Self-Biased Current Mirror & its Issues 

- The idea of self-bias is that if Iout is to be ultimate independent of the Iref can be a replica of iout.
- MP1 and MP2 copy Iout and define Iref
Since each diode-connected device feed from a current source, Iout and Iref are relative independents of Vdd.

![Screenshot from 2021-10-21 12-26-14](https://user-images.githubusercontent.com/80625515/138248033-f48c731f-77c0-4864-81bf-2751d42ab58f.png)

#### Issues:

- The circuit is governed by one equation; Iout=K x Iref hence can support any amount of current.
- To uniquely define the currents, another constraint must be added in the circuit

![Screenshot from 2021-10-21 12-27-29](https://user-images.githubusercontent.com/80625515/138247770-d12aa7d8-d66d-42fa-8142-058901e8422b.png)


- In the above circuit Rs defines current uniquely
- Both Iout and Iref are very little dependent on Vdd.
- Loop Gain is always less than one, by default the circuit is stable.
- An important issue in supply-independent biasing is the existence of degenerate bias points.
- Start-up problem when supply turned on.

### Summary of Self-Biased Current Mirror

![Screenshot from 2021-10-21 12-32-38](https://user-images.githubusercontent.com/80625515/138247447-59253e22-c6fa-43a2-8813-3c9e643a3dfb.png)


#### Advantages 

- Most simplified topology
- Simple to design
- By-default the circuit is stable

#### Limitations

- More prone to supply voltage variation
- Cascode structure can be used to solve the issue
- Voltage headroom issues
- Start-up issue

## Reference Branch

![Screenshot from 2021-10-21 12-43-06](https://user-images.githubusercontent.com/80625515/138246778-c109c899-dfe9-468f-9a36-a92c3d33ea40.png)

- Current I3 is the same as I1 & I2
- Voltage across Q3 is CTAT type
- Voltage across R2 is PTAT type
- Vref is the addition of CTAT & PTAT voltages
- R2 = alpha(R1)

### Design of R2 Resistance 

- Tempco. of Vref should be zero
- As all the values known, alpha can be calculated easily.
- R2 = alpha(R1)

## Start-up

#### Need for Start-up Circuit
    
![Screenshot from 2021-10-21 12-52-29](https://user-images.githubusercontent.com/80625515/138246129-68ef3b3c-3e79-4a7e-9af7-6a1c7ad7eb5b.png)

- If the channel length modulation is negligible, the current hardly depends on the supply voltage.
- The main issue in supply independent biasing is the existence of degenerate bias points.
- There are two stable operating points
    - Iin = Iout = 0A (undesired operating point)
    - desired operating points
- Must keep the circuit out of the undesired point when the supply is turned on.
- Must not interface with the circuit once it reaches the desired operating point.

![Screenshot from 2021-10-21 12-52-51](https://user-images.githubusercontent.com/80625515/138246278-bce3ffb9-1967-4a63-80bc-72d29a7ab5f9.png)
    
- Initially circuit current at Zero
- Net2 follows VDD
- When net2 voltage, one Vt more than net6 voltage, current will flow through MP5 and net1 node up the voltage

## Bandgap Reference Circuit

![bgr-full](https://user-images.githubusercontent.com/80625515/138543592-97ea4ddf-e5ec-4c12-8143-2f7d5b3bae50.png)

## Analog Design Fow 

![analog-design-snap](https://user-images.githubusercontent.com/80625515/138543609-8b155295-cb4c-4c33-9f16-0327a09135b6.png)

### Schematic design


#### CTAT voltage generation circuit
```
**** ctat voltage generation circuit *****

.lib "/home/khalique/BGR_VSDOpen21_Workshop/eda-technology/sky130/models/spice/models/sky130.lib.spice tt"
.include "/home/khalique/BGR_VSDOpen21_Workshop/eda-technology/sky130/models/spice/models/sky130_fd_pr__model__pnp.model.spice"

.global vdd gnd
.temp 27

*** bjt definition
xqp1	gnd	gnd	qp1	gnd	sky130_fd_pr__pnp_05v5_W3p40L3p40	m=1

*** supply voltage and current
vsup	vdd	gnd	dc	2
isup	vdd	qp1	dc 	10u
.dc	temp	-40	125	5

*** control statement
.control
run
plot v(qp1)
.endc
.end

```
#### PTAT voltage generation circuit
```
**** ptat voltage generation circuit *****

.lib "/home/khalique/BGR_VSDOpen21_Workshop/eda-technology/sky130/models/spice/models/sky130.lib.spice tt"
.include "/home/khalique/BGR_VSDOpen21_Workshop/eda-technology/sky130/models/spice/models/sky130_fd_pr__model__pnp.model.spice"

.global vdd gnd
.temp 27

*** vcvs definition
e1	net2	gnd	ra1	qp1	gain=1000
xmp1    q1	net2   	vdd	vdd     sky130_fd_pr__pfet_01v8_lvt     l=2     w=5     m=4
xmp2    q2    	net2    vdd     vdd     sky130_fd_pr__pfet_01v8_lvt     l=2     w=5     m=4
	
*** bjt definition
xqp1	gnd	gnd	qp1	vdd	sky130_fd_pr__pnp_05v5_W3p40L3p40	m=1
xqp2    gnd     gnd     qp2     vdd     sky130_fd_pr__pnp_05v5_W3p40L3p40       m=8

*** high-poly resistance definition
xra1    ra1     na1     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  	l=7.8
xra2    na1     na2     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41	l=7.8
xra3    na2     qp2     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41	l=7.8
xra4    na2     qp2     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41	l=7.8

*** voltage sources for current measurement
vid1	q1	qp1	dc	0
vid2	q2	ra1	dc	0

*** supply voltage
vsup	vdd	gnd	dc 	2
.dc	temp	-40	125	5

*** control statement
.control
run
plot v(qp1) v(ra1) v(qp2) v(net2)
plot vid1#branch vid2#branch
.endc
.end
```

#### Resistor Bank circit
```
**** RES tempco circuit *****

.lib "/home/khalique/BGR_VSDOpen21_Workshop/eda-technology/sky130/models/spice/models/sky130.lib.spice tt"
.include "/home/khalique/BGR_VSDOpen21_Workshop/eda-technology/sky130/models/spice/models/sky130_fd_pr__model__pnp.model.spice"

.global vdd gnd
.temp 27

*** resistor definition
xra1    ra1     na1     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xra2    na1     na2     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xra3    na2     qp2     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xra4    na2     qp2     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8

*** supply current
vsup	vdd	gnd	dc	2
vid     qp2     gnd     dc      0
isup	gnd	ra1	dc 	10u
.dc	temp	-40	125	1	

*** control statement
.control
run
plot v(ra1) 
plot v(ra1)/vid#branch
.endc
.end
```

#### Complete BGR Circuit

```
**** bandgap reference circuit using self-biase current mirror *****

.lib "/home/khalique/BGR_VSDOpen21_Workshop/eda-technology/sky130/models/spice/models/sky130.lib.spice tt"
.include "/home/khalique/BGR_VSDOpen21_Workshop/eda-technology/sky130/models/spice/models/sky130_fd_pr__model__pnp.model.spice"

.global vdd gnd
.temp 27

*** circuit definition ***

*** mosfet definitions self-biased current mirror and output branch
xmp1	net1	net2	vdd	vdd	sky130_fd_pr__pfet_01v8_lvt	l=2	w=5	m=4
xmp2    net2    net2    vdd     vdd     sky130_fd_pr__pfet_01v8_lvt     l=2     w=5    	m=4
xmp3    net3    net2    vdd     vdd     sky130_fd_pr__pfet_01v8_lvt     l=2     w=5     m=4
xmn1    net1    net1    q1      gnd     sky130_fd_pr__nfet_01v8_lvt     l=1     w=5     m=8
xmn2    net2    net1    q2      gnd     sky130_fd_pr__nfet_01v8_lvt     l=1     w=5     m=8

*** start-upcircuit
xmp4    net4    net2    vdd     vdd     sky130_fd_pr__pfet_01v8_lvt     l=2     w=5     m=1
xmp5    net5    net2    net4    vdd     sky130_fd_pr__pfet_01v8_lvt     l=2     w=5     m=1
xmp6    net7    net6    net2    vdd     sky130_fd_pr__pfet_01v8_lvt     l=2     w=5     m=2
xmn3    net6    net6    net8    gnd     sky130_fd_pr__nfet_01v8_lvt     l=7     w=1     m=1
xmn4    net8    net8    gnd     gnd     sky130_fd_pr__nfet_01v8_lvt     l=7     w=1     m=1

*** bjt definition
xqp1	gnd	gnd	qp1	vdd	sky130_fd_pr__pnp_05v5_W3p40L3p40	m=1
xqp2    gnd     gnd     qp2     vdd     sky130_fd_pr__pnp_05v5_W3p40L3p40       m=8
xqp3    gnd     gnd     qp3     vdd     sky130_fd_pr__pnp_05v5_W3p40L3p40       m=1

*** high-poly resistance definition
xra1	ra1	na1	vdd	sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xra2	na1     na2     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xra3    na2     qp2     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xra4    na2     qp2     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8

xrb1    vref    nb1     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb2    nb1     nb2     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb3    nb2     nb3     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb4    nb3     nb4     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb5    nb4     nb5     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb6    nb5     nb6     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb7    nb6     nb7     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb8    nb7     nb8     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb9    nb8     nb9     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb10   nb9     nb10    vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb11   nb10    nb11    vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb12   nb11    nb12    vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb13   nb12    nb13    vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb14   nb13    nb14    vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb15   nb14    nb15    vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb16   nb15    nb16    vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb17   nb16    qp3   	vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8
xrb18   nb16    qp3     vdd     sky130_fd_pr__res_high_po_1p41     w=1.41  l=7.8

*** voltage source for current measurement
vid1	q1	qp1	dc	0
vid2    q2      ra1     dc      0
vid3    net3    vref    dc      0
vid4    net7	net1	dc	0
vid5	net5	net6	dc	0

*** supply voltage
vsup	vdd	gnd	dc 	2
*.dc	vsup	0	3.3	0.3.3
.dc	temp	-40	125	5

*vsup	vdd	gnd	pulse	0	2	10n	1u	1u	1m	100u
*.tran	5n	10u

.control
run

plot v(vdd) v(net1) v(net2) v(qp1) v(ra1) v(qp2) v(vref) v(qp3)
plot vid1#branch vid2#branch vid3#branch vid4#branch vid5#branch

.endc
.end

```
### Pre-layout Simulation

#### CTAT Voltage generation simulation

![ctat-bjt-term](https://user-images.githubusercontent.com/80625515/138544033-52a45a54-b82b-4e4b-9a24-e7829e0ebd46.png)

![ctat-bjt-wave](https://user-images.githubusercontent.com/80625515/138544039-14cf0113-135c-41b2-9050-d5f756cf9a87.png)

![ctat-gen-wave](https://user-images.githubusercontent.com/80625515/138544049-f7c558f3-866a-4792-bab1-9e28b64720e7.png)

![ctat-wave](https://user-images.githubusercontent.com/80625515/138544060-66b7e9c5-86fe-4215-a477-33ae52cec0f8.png)

![ctat-var-current-term](https://user-images.githubusercontent.com/80625515/138544072-279c93ce-0220-4227-b2f6-b57e61e17b00.png)

![ctat-var-current-wave](https://user-images.githubusercontent.com/80625515/138544076-3ba109d8-e86a-48b5-b1c6-b3d605acdcf5.png)

#### PTAT Voltage generation simulation

![ptat-opamp-term](https://user-images.githubusercontent.com/80625515/138544115-53b2b16e-eb45-4e8c-b880-c2624c466357.png)

![ptat-opanp-wave](https://user-images.githubusercontent.com/80625515/138544120-342a9311-caf3-4e43-9b21-2057fcae28cb.png)

![ptat-pre-simwave](https://user-images.githubusercontent.com/80625515/138544494-dab82136-b8ea-4f49-84b3-90e054320374.png)

![ptat-volt-sen-pre-sim](https://user-images.githubusercontent.com/80625515/138544506-91ec9234-f486-4afe-a270-f336fb61e660.png)


#### Resistor temperature coefficient measurement

![res-tempco-term](https://user-images.githubusercontent.com/80625515/138544226-c538fd17-856a-4864-9bea-dab50791811a.png)

![res-tempco-wave](https://user-images.githubusercontent.com/80625515/138544230-faf6dade-157d-45e6-9400-eac21a79794e.png)

![res-cur-var-wave](https://user-images.githubusercontent.com/80625515/138544525-48d6da41-f0e9-4dd2-9019-3fae657f2809.png)

#### BGR Complete simulation

![bgr-opampterm](https://user-images.githubusercontent.com/80625515/138544355-5b7028fd-967a-446a-b42a-66bee56691b2.png)

![bgr-opamp1](https://user-images.githubusercontent.com/80625515/138544362-c0131ee3-4bd8-4bff-b21c-926a3362ff80.png)

![bgr-opamp2](https://user-images.githubusercontent.com/80625515/138544370-238457ab-0a95-4908-8fea-c443249745ed.png)

![bgr-var1](https://user-images.githubusercontent.com/80625515/138544386-45c6a9a2-29bc-48e8-bd53-dc5e242e5463.png)

![bgr-var2](https://user-images.githubusercontent.com/80625515/138544391-bdab7749-7ac6-427a-95ff-b3a3d55b03d9.png)

![bgr-ff1](https://user-images.githubusercontent.com/80625515/138544394-5806f1a5-da85-4e9f-9c75-b5814f5bf9cb.png)

![bgr-ff2](https://user-images.githubusercontent.com/80625515/138544396-c86632c3-2ca1-4a70-8119-560465959303.png)

![bgr-selfbias-cmr1](https://user-images.githubusercontent.com/80625515/138544425-078c79ad-ffd2-4e69-aba5-fa0e17a18263.png)

![bgr-selfbias-cmr2](https://user-images.githubusercontent.com/80625515/138544427-6ec23e01-fd15-4fd4-ba17-863339ff92b1.png)

![bgr-ss1](https://user-images.githubusercontent.com/80625515/138544441-ec5c2269-1d6e-4ad7-92d5-13ee274ef885.png)

![bgr-ss2](https://user-images.githubusercontent.com/80625515/138544443-702eca8f-776c-4d2c-ab61-ddbe93967386.png)

## Layout

#### Resistors

![single-resistor-lay](https://user-images.githubusercontent.com/80625515/138544949-1c6232b6-f57b-46ba-8790-114cdb276c7a.png)

![resistors-layout](https://user-images.githubusercontent.com/80625515/138544956-4396fcbd-c36e-41b3-81dc-cec174cbc1d0.png)

#### BJTs

![1pnp-lay](https://user-images.githubusercontent.com/80625515/138544977-59710b63-3037-4de9-afbe-07d5f0aa44ed.png)

![pnp-lay](https://user-images.githubusercontent.com/80625515/138544982-c24203e2-b3a7-486f-960d-f1069d055635.png)


#### Mosfets (N-channel)

![nfet-lay](https://user-images.githubusercontent.com/80625515/138544991-eb3d044f-628e-43ff-b4f9-a7347ba6fda9.png)

![nfets-lay](https://user-images.githubusercontent.com/80625515/138545038-4d030975-bd2b-4ea2-8aba-ef88721bd93f.png)

![nfet1-lay](https://user-images.githubusercontent.com/80625515/138545107-7ed543fb-a678-440f-bb56-d051e8328a47.png)

![starternfet-lay](https://user-images.githubusercontent.com/80625515/138545118-e4acc2fe-5a00-4310-8587-b0ef0f6998ad.png)


#### Mosfets (P-channel)

![pfet-lay](https://user-images.githubusercontent.com/80625515/138545012-99c7fb32-0344-4e38-ae11-0f9b2a010e59.png)

![pfets-lay](https://user-images.githubusercontent.com/80625515/138545021-09f66fba-60bb-4520-ad30-793af7425d87.png)

#### Top Level BGR Layout

![bgr-final-lay](https://user-images.githubusercontent.com/80625515/138545125-ee3572fc-5d44-4378-8ef8-6aa22eea40d3.png)


## Acknowledgement
- Kunal Ghosh, Co-founder, VSD
- Anagha Ghosh, VSD
- Santunu Sarangi, Asst. Professor

## Author
- Mohammad Khalique Khan, Aliah University
