---
title: "SIwave1 New Project and DC IR analysis"
date: 2023-06-01
draft: False
math: katex
summary: 
tags: ["SIwave"]
---



Using `Ansys SIwave 2020R2`. 

This post introduces a basic IR-drop analysis on the power planes using SIwave 2020R2. 

- Analyze the voltage drop of one power rail, from source to chip. 
- All the RLC components are deactivated except for those connected in series to the power rails. 

SIwave Documentation: [Computing DC IR Simulations](https://ansyshelp.ansys.com/account/secured?returnurl=/Views/Secured/Electronics/v202/en/home.htm%23../Subsystems/SIwave/Content/ComputingDCSimulations.htm%3FTocPath%3DSIwave%7CSIwave%2520Help%7CRunning%2520Simulations%7CSIwave%2520Simulations%7CComputing%2520DC%2520IR%2520Simulations%7C_____0)

# New Project

## Import ODB++

![](/images/img_2023-05-28-2.png)

![](/images/img_2023-05-28-1.png)

default setting for nets. 
![](/images/img_2023-05-28-3.png)

# The Workflow Wizard
 Open the `Workflow Wizard` from this button on the upper-left. 

![](/images/img_2023-05-28-4.png)

## Stackup
Delete the `smt` layers above `top` and below `bottom` (they have 0 thickness and would cause simulation error). 

(For IR-analysis, the dielectric material is not important, so use default.)

![](/images/img_2023-05-28-5.png)

## Padstack
Mainly, the via plating setting. 
Default: 

![](/images/img_2023-05-28-6.png)

Change it to 1mil (or something else, according to the manufacturer). 
![](/images/img_2023-05-28-7.png)

## Verify Circuit Element Parameters
Deactivate all the RLC elements. 


## Power/Ground Net and Sanitize Layout
Default. Here are the documentations. 
![](/images/img_2023-05-28-8.png)

[Identifying Power and Ground Nets](https://ansyshelp.ansys.com/account/secured?returnurl=/Views/Secured/Electronics/v202/en/home.htm%23../Subsystems/SIwave/Content/IdentifyingPowerandGroundNets.htm%3FTocPath%3DSIwave%7CSIwave%2520Help%7CSimulation%2520Setup%7C_____1)

>  [Sanitize Layout](https://ansyshelp.ansys.com/account/secured?returnurl=/Views/Secured/Electronics/v202/en/home.htm%23../Subsystems/SIwave/Content/SanitizeLayout.htm%3FTocPath%3DSIwave%7CSIwave%2520Help%7CSimulation%2520Setup%7C_____5) is an operation for cleaning power and ground nets in order to fix certain alignment problems as well as complexities that may slow down simulation. It works by uniting the planes and traces for each net to be cleaned. Once the united planes are formed, portions that display trace-like properties are converted into traces.


## Connect the two planes
The `VCCINT` plane is smulated. The resistor in series is for current sensing, connecting the two planes, `VCCINT` and `VCCINT_FPGA`. 

![](/images/img_2023-05-28-22.png)

Add an ideal resistor to connect `VCCINT` and `VCCINT_FPGA` in the simulation. 

![](/images/img_2023-05-28-10.png)

![](/images/img_2023-05-28-11.png)

![](/images/img_2023-05-28-12.png)

![](/images/img_2023-05-28-13.png)


## Configure DC-IR Drop

- `VCCINT` and `VCCINT_FPGA` are both selected. 
- U21 is the power supply, set it as `voltage source`. 
- U1 is the FPGA chip, set it as `current source` (also `Distributed Current` so that each power supply pin is simulated as a small load). 

![](/images/img_2023-05-28-14.png)

Follow `Configure--Validate--Simulate` to start IR simulation. 

![](/images/img_2023-05-28-15.png)

![](/images/img_2023-05-28-16.png)


### About Node to Ground

From documentation: mainly, select `Negative` for voltage source and `Neither` for everything else. 

> Use the **Node to Ground** drop-down menu to select **Neither**, **Negative**, or **Positive**.

- If you select **Negative** on a voltage source, the voltage display on its ground pin will be 0 V.
- If you select **Positive** on a voltage source, the voltage display on its ground pin will be negative.
Consider this as being the 0 node in a SPICE simulation. This is the node to which you want all voltages referenced. Since SIwave includes the return paths of all planes (including the ground), we cannot assume that all of the sources reference that 0 node. Thus we have to pick one to be the absolute 0 node reference. So if we put the 0 node on the positive side of a voltage plane, the "ground" or return path would show as negative.

# Displaying Results
Plot Current/Voltages

![](/images/img_2023-05-28-17.png)

This example displays voltage level. 
- Select certain layers for display
- Display other stuff such as current density (J) or power (P). 
![](/images/img_2023-05-28-18.png)

View-turn off `Display Mesh`, for a cleaner look. 
![](/images/img_2023-05-28-19.png)

Display only layers 1,3,4,9,15,16: 
![](/images/img_2023-05-28-20.png)

The results: 
- The red one is `VCCINT`, the source, at almost 1V
- Between `VCCINT` and `VCCINT_FPGA` (the green one), is the voltage drop on the current sensing resistor (`0.005R*20A=0.1V`)
- The total voltage drop from source to load is around 0.3 V. But in reality, the voltage feedback trace will compensate for the voltage drop, so that the load voltage is 1V (and the source voltage is about 1.3V). 

![](/images/img_2023-05-28-21.png)