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
## Introduction

- Temperatue independent voltage reference circuit widely used in Integrated Cicuits.
- Produces cinstant voltage regardless of power supply variation, temp. changes and circuit loading.
- Output voltage of 1.2v (close to the band gap enery of silicon at 0 deg kelvin)
- All applications starting from analog, digital, mixed mode, RF and SOCs

![Screenshot from 2021-10-21 09-16-02](https://user-images.githubusercontent.com/80625515/138239326-21775953-80a9-4241-9fd3-71154af78744.png)


### Why BGR

1. A battery is unsuitable for use as a reference voltage source.
- voltage drops over time
2. A typical power supply is also not suitable.
- noisy output and residual ripple
3. A voltage referene IC used buried Zener Diode.
- Discrete design required additional componentsand high frequency filtering circuits due to higher thermal noise.
- low voltage Zener diode is not available
4. A Bandgap reference which can be integrated in bulk CMOS, Bi-CMOS or Bipolar technologies without the use of external cmponents.

### Applications of BGR

- Low dropoutreglators (LDO)
- DC-to-DC buck converters
- Analog-to-Digital Converter (ADC)
- Digital-to-Analog Converter (DAC)
- 
![Screenshot from 2021-10-21 09-29-39](https://user-images.githubusercontent.com/80625515/138239732-8cd44bbd-610a-4ef4-847a-5a2aff7a2e3e.png)
![Screenshot from 2021-10-21 09-30-07](https://user-images.githubusercontent.com/80625515/138239917-b399727b-f27a-4bf2-878d-3bd0d8fddaff.png)


## Types of BGR

### Architecture wise BGR can be designed in two ways,

1. Using self-biased current mirror
2. Using Operational- Amplifier

### Application wise BGR can be categorized as,

1. Low-voltage BGR
2. Low-power BGR
3. High-PSRR and low-noise BGR
4. Curvature compressed BGR

## Self-Biased Current Mirror Based BGR

<bgr-circuit-diagram>

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

- CTAT voltage generation circuit
- PTAT voltage genertaion circuit
- Self-biased current mirror cirrcuit
- Reference branch circuit
- Start-up circuit

## CTAT Voltage Generation

It is the most important component of BGR. 



## PTAT Voltage Generation



### Design of R1 Resistance 

- R1 completely depends upon the power consumption and silicon area budget.
- R1 = Vt 1n (N)/I
- Resistance decreases, so area decreases with the increase of circuit current.
- Resistance increases, so area increases with the decrease of circuit current.
- Also the resistance value depends on number of BJT used in the branch 2.
i.e, for 10uA current with N=8, R1 calculated to be 5.4k Ohm

## Self-bias Current Mirror

### Current Mirror & its Issues

The objective of reference generation is to establish DC voltage or current,
- Independent of supply
- Independent of process and
- Well-defined behavior with temperature

#### Issues:

- Output current of this circuit quite sensitive to VDD
- For a less sensitive solution, the circuit must bias itself, i.e Iref must be some how derived from Iout

### Self-Biased Current Mirror & its Issues 
- The idea of self bias is that if Iout is to be ultimate independent of the Iref can be a replica of iout.
- MP1 and MP2 copy Iout and define Iref
Since each diode coonnected device feed from a current source, Iout and Iref arerelative independent of Vdd.

#### Issues:

- The circuit is governed by one equation; Iout=K x Iref, hence can support any amount of current.
- To uniquely define the currents, another constraint must be added in the circuit

- In the above circuit Rs defines current uniquely
- Both Iout and Iref are very little dependent on Vdd.
- Loop Gain always less than one, by-default the circuit is stable.

- An important issue in supply-independent biasing is the existence of degenerate bias point.
- Start-up problem when supply turned on.

### Summary of Self-Biased Current Mirror

#### Advantages 

- Most simplified topology
- Simple to design
- By-default the circuit is stable

#### Limitations

- More prone to supply volage variation
- Cascode structue can be used to solve the issue
- Voltage head room issues
- Start-up issue

## Reference Branch

- Current I3 is same as I1 & I2
- Voltage across Q3 is CTAT type
- Voltage across R2 is PTAT type
- Vref is the addition of CTAT & PTAT voltages
- R2 = alpha(R1)

### Design of R2 Resistance 

- Tempco. of Vref should be zero
- As all the values known, alpha ca be calculated easily.
- R2 = alpha(R1)

## Start-up

#### Need for Start-up Circuit

- If the channel length modulation is negligible, the current hardly depends on supply voltage.
- The main issue in supply independent biasing is the existence of degenerate bias points.
- There are two stable operating points
    - Iin = Iout = 0A (undesired operating point)
    - desired operating points
- Must keep the circuit out of the undesired point when the supply is turned on.
- Must not interface with the circuit once it reaches the desired operating point.


<startup-cir>

- Initially circuit current at Zero
- Net2 follows VDD
- When net2 voltage, one Vt more than net6 voltage, current will flow through MP5 and net1 node up the voltage
