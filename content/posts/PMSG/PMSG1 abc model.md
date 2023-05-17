---
title: "PMSG1 model in abc reference frame"
date: 2023-05-15
draft: False
math: mathjax
description: Brief introduction of operation and equations in abc reference frame. 
tags: ["PMSG control"]
---


# A very brief introcuction
For an ideal PMSG: 
- The magnets in the rotor generate a constant magnetic field, $\psi_{m}$ (When the rotor is still). 
- The three-phase AC current in the stator windings generates a rotating magnetic field (RMF), with speed of $60f$ RPM (revolutions per minute). $f$ is the frequency of the three-phase supply, and 60 is from seconds in a minute. 
- (My rough explanation: the stator winding with 3-ph AC current is like a set of magnets revolving outside the rotor, the rotor magnets, therefore, would also revolve, with its north pole locked to the south poles of the equivalent stator magnets.)
- In steady state, the rotor is revolving at the **synchronous speed**, $\frac{60f}{P}$ RPM, where $P$ is the number of pole pairs. 

About $P$ and synchrounous speed: [How does the number of poles in an induction motor determine its speed and torque? - Electrical Engineering Stack Exchange](https://electronics.stackexchange.com/questions/241418/how-does-the-number-of-poles-in-an-induction-motor-determine-its-speed-and-torqu)

During a half cycle of the supply current, the polarity of the magnetic field of the stator is reversed, so the rotor revolves from one pole to the other pole in the pair, therefore, 
- more pole pairs -> smaller angle for each pole pair -> less angle the rotor revolves in one cycle -> lower synchrounous speed

(It is really brief because I didn't know much about motors. )

# PMSG model in abc frame
The dynamics of the PMSG is usually described by a set of equations: stator voltage, stator flux, Torque, and rotor speed. 
(Similar to those of the DFIG.)

- Ideal model, harmonics, saturation, losses, etc., are all ignored.

## Stator voltage
The 3-ph voltage is supplied to the stator windings. Each winding could be regarded as a resistor and a coupled inductor. From Lenz's Law and Kirchhoff's voltage law, for each winding, $u_{x}=R_{s}i_{x}+ \frac{d \psi_{x}}{dt}$.
Write out the equations in matrix form:
$$\left[\begin{matrix}
u_A \\\ u_B \\\ u_C
\end{matrix}\right] = 
\left[\begin{matrix}
R_s & 0 & 0 \\\ 0 & R_s & 0 \\\ 0 & 0 & R_s
\end{matrix}\right]
\left[\begin{matrix}
i_A \\\ i_B \\\ i_C
\end{matrix}\right] + 
\frac{d}{dt}\left[\begin{matrix}
\psi_A \\\ \psi_B \\\ \psi_C
\end{matrix}\right]$$

## Stator flux
The total flux of each stator winding comes from the winding itself, other windings, and the rotor. 


$$\begin{align*}
\left[\begin{matrix}
\psi_{A} \\\  \psi_{B} \\\  \psi_{C} \\\ 
\end{matrix}\right] &= 
\left[\begin{matrix}
\psi_{AA}+\psi_{AB}+\psi_{AC}+\psi_{Am} \\\  \psi_{BA}+\psi_{BB}+\psi_{BC}+\psi_{Bm} \\\  \psi_{CA}+\psi_{CB}+\psi_{CC}+\psi_{Cm} \\\ 
\end{matrix}\right] \\\
&=\left[\begin{matrix}
L_{AA} & L_{AB} & L_{AC} \\\
L_{BA} & L_{BB} & L_{BC} \\\
L_{CA} & L_{CB} & L_{CC}
\end{matrix}\right]
\left[\begin{matrix}
i_{A}\\\
i_{B}\\\
i_{C}
\end{matrix}\right]+
\left[\begin{matrix}
cos \theta_{r} \\\
cos \left(\theta_{r}+ \frac{2}{3}\pi\right) \\\
cos \left(\theta_{r}- \frac{2}{3}\pi\right)
\end{matrix}\right] \psi_{m}
\end{align*}$$




## Torque



## Rotor speed

$$
\frac{J}{n_p}\frac{d\omega_r}{dt}=T_e-T_L
$$
testing a new post. 



Most of the equations still came from textbooks, as I cannot invent them. But I'll try to use my own words for the next posts. 




[Inductance - Wikipedia](https://en.wikipedia.org/wiki/Inductance)

- [[What is flux and what is an inductor]]
- [[What is mutual flux and mutual inductance]]
These are some questions to revise in later posts. 