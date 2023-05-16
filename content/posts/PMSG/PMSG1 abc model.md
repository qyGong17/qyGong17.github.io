---
title: "PMSG1 model in abc reference frame"
date: 2023-05-15
draft: False
math: katex
description: Brief introduction of operation and equations in abc reference frame. 
tags: ["PMSG control"]
---


# A very brief introcuction
For an ideal PMSG: 
- The magnets in the rotor generates a constant magnetic field, $\psi_{r}$. (When the rotor is still)
- The three-phase AC current in the stator windings generates a rotating magnetic field (RMF), with speed of $60f$ RPM (revolutions per minute). $f$ is the frequency of the three-phase supply, and 60 is from seconds in a minute. 
- In steady state, the rotor is revolving at the **synchronous speed**, $\frac{60f}{P}$ RPM, where $P$ is the number of pole pairs. 


About $P$ and synchrounous speed: [How does the number of poles in an induction motor determine its speed and torque? - Electrical Engineering Stack Exchange](https://electronics.stackexchange.com/questions/241418/how-does-the-number-of-poles-in-an-induction-motor-determine-its-speed-and-torqu)

During a half cycle of the supply current, the polarity of the magnetic field of the stator is reversed, so the rotor revolves from one pole to the other pole in the pair, therefore, 
- more pole pairs -> smaller angle for each pole pair -> less angle the rotor revolves in one cycle -> lower synchrounous speed

(I know it is really brief because I didn't know much about motors. )


# PMSG model in abc frame
The dynamics of the PMSG is usually described by a set of equations: stator voltage, stator flux, Torque, and rotor speed. 
(Similar to those of the DFIG.)

## Stator voltage

$$
\left[\begin{matrix}
u_A \\\\\\ u_B \\\\\\ u_C
\end{matrix}\right] = 
\left[\begin{matrix}
R_s & 0 & 0 \\\\\\ 0 & R_s & 0 \\\\\\ 0 & 0 & R_s
\end{matrix}\right]
\left[\begin{matrix}
i_A \\\\\\ i_B \\\\\\ i_C
\end{matrix}\right] + 
\frac{d}{dt}\left[\begin{matrix}
\psi_A \\\\\\ \psi_B \\\\\\ \psi_C
\end{matrix}\right]
$$

## Stator flux


## Torque

$$
\frac{J}{n_p}\frac{d\omega_r}{dt}=T_e-T_L
$$

## Rotor speed

testing a new post. 

Still, kind of weird





