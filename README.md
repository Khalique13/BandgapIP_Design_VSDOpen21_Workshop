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

## Acknowledgement
- Kunal Ghosh, Co-founder, VSD
- Anagha Ghosh, VSD
- Santunu Sarangi, Asst. Professor

## Author
- Mohammad Khalique Khan, Aliah University
