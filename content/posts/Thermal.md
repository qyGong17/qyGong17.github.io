---
title: "Thermal resistance and thermal characterisation parameter"
date: 2023-06-05
draft: False
math: katex
summary: A (bit simplistic) summary of thermal resistance, thermal characterisation parameter and how to use them to estimate junction temperature. 
tags: ["hardware"]
---

A (bit simplistic) summary of thermal resistance, thermal characterisation parameter and how to use them to estimate junction temperature. 

## Summary
- `Thermal resistance` of certain path (such as junction-case top) = (temperature difference ) / ( actual power conducted through this path ) 
- `Characterisation parameter` between junction and case top =  ( temperature difference ) / ( `total` power dissipated ) 
- `Characterisation parameter` by definition is not `thermal resistance`, therefore a different letter `Ψ` is used. 
- `Characterisation parameter` is more suitable for estimating junction temperature in applications (chips mounted on a circuit board), while `thermal resistance` could be used to compare the thermal performance of different packages. 


## Formulae
For example, estimating junction temperature using `junction-case top` parameters. 

$P_T$ is the total dissipated power, and $P_{j-ctop}$ is the portion of power conducted through the `junction-case top` path. 

Using `characterisation parameter`: 

$$T_{j}=T_{ctop}+P_{T}\psi_{j-ctop}$$

Using `thermal resistance`

$$T_{j}=T_{ctop}+P_{j-ctop}R_{\theta jc}$$

- Pay attention to the test condition (such as PCB size, number of vias)



## Resource

[Semiconductor and IC Package Thermal Metrics (Rev. C) (ti.com)](https://www.ti.com/lit/an/spra953c/spra953c.pdf)

[How to Evaluate Junction Temperature Properly with Thermal Metrics (Rev. B)](https://www.ti.com/lit/an/slua844b/slua844b.pdf)

> The complexity of the thermal conduction path in an actual system determines that junction temperature cannot be simply estimated by the total dissipation power and thermal resistance parameters.