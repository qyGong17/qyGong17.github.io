---
title: "SIwave2 S-parameter Extraction"
date: 2023-06-04
draft: False
math: katex
summary: This post introduces how to extract the s-parameter of a pair of PCB traces using SIwave 2020R2.
tags: ["SIwave"]
---



Using `Ansys SIwave 2020R2`. 

This post introduces how to extract the s-parameter of a pair of PCB traces using SIwave 2020R2. 

# Resource
From SIwave documentation: 
- [Adding a Port - Simulation Setup](https://ansyshelp.ansys.com/account/secured?returnurl=/Views/Secured/Electronics/v202/en/home.htm%23../Subsystems/SIwave/Content/AddingaPort.htm)
- [Computing SYZ Parameters using SIwave](https://ansyshelp.ansys.com/account/secured?returnurl=/Views/Secured/Electronics/v202/en/home.htm%23../Subsystems/SIwave/Content/ComputingSYZParametersinSIwave.htm)

# Sample Board
The schematic: 
Focus on the `RX` pair with a pair of AC-coupling capacitors. 
![](/images/img_2023-05-29.png)

The PCB layout: 
- Trace length around 4100 mils
- Trace width/spacing=4.2mil/6.3mil
![](/images/img_2023-05-29-1.png)

The 8-layer stackup in Allegro. 
![](/images/img_2023-05-29-2.png)


# Importing to SIwave

- [SIwave0 Allegro 16.6 Export ODB++ | QY's Notes](https://qygong17.github.io/posts/siwave/siwave0-allegro-16.6-export-odb++/)
- [SIwave1 New Project and DC IR analysis | QY's Notes](https://qygong17.github.io/posts/siwave/siwave1-new-project-and-dc-ir-analysis/)

# Stackup
Dielectric material: [TU-768 - Taiwan Union Technology Corporation](https://www.tuc.com.tw/en-us/products-detail/id/24)

> Use `select all` on the bottom-left corner to select and edit all conductor/dielectric layers. 

For example, 
- Medal: `copper`, (also, dielectric fill: `FR-4`)
- Dielectric: `FR-4`
![](/images/img_2023-05-29-3.png)

# Assign Capacitor Model
Tools-S-param Model Assignment, use `filter` to narrow down the model list. 

![](/images/img_2023-05-29-5.png)

# Adding Ports
Tools-Generated Port: 
![](/images/img_2023-05-29-4.png)

For the RX pair in the sample board, `J1---cap---J2`, for each trace, 2 ports are needed at `J1` and `J2`: 

![](/images/img_2023-05-29-6.png)

Check the generated port and make sure that the negative net `-Net` is GND and is near the positive net `+Net`. 
![](/images/img_2023-05-29-7.png)


# Compute SYZ Parameters
Simulation-Compute SYZ Parameters: 
- Set the frequency range
- `Export Touchstone file`, this will generate an `.snp` file, where `n` is the total number of ports. In this case, an `.s4p` file. 
![](/images/img_2023-05-29-8.png)


# Display Results
Select the SYZ simulation-right click-Compute Differential S-parameters: 
![](/images/img_2023-05-29-9.png)

For example, insertion loss: 
- `+Port` and `-Port` pair: `RX_P` and `RX_N` ports at J1 (or J2)
- Plot: from diff. port `DIFFP0:Diff` to `DIFFP1:Diff`
![](/images/img_2023-05-29-10.png)

Also, the `.snp` file could be used in other software such as ADS: [like in this youtube video](https://www.youtube.com/watch?v=f45I5R8iNwQ)

# SnP File
In an `.s4p`, at each frequency, there are `1 stimulus and 16 S-parameters`, each S-parameter consists of real and imaginary parts, so 33 parameters at each frequency point. 

Such as: 
```
! S-parameters
# Hz S MA R 50.000000
5.000000000000e+06 7.415729611535e-03 -9.049652032862e+00 2.905053518013e-03 7.206460994824e+01 9.921850136786e-01 -1.314852190849e+00 1.330022519835e-03 -1.285113705145e+02
                   2.905053518013e-03 7.206460994824e+01 7.414583662428e-03 -9.086051281811e+00 1.330024196348e-03 -1.285117281406e+02 9.921868920609e-01 -1.314594245442e+00
                   9.921850136786e-01 -1.314852190849e+00 1.330024196348e-03 -1.285117281406e+02 7.547298314186e-03 -8.975382166206e+00 2.901011285899e-03 7.215890285145e+01
                   1.330022519835e-03 -1.285113705145e+02 9.921868920609e-01 -1.314594245442e+00 2.901011285899e-03 7.215890285145e+01 7.545921799068e-03 -9.010637065165e+00
```

According to [SnP File Format](https://rfmw.em.keysight.com/wireless/helpfiles/N1930B/FilePrint/SnP_File_Format.htm#:~:text=SnP%20files%20contain%20header%20information,(records)%20in%20each%20file.)
```
*.s1p Files
Each record contains 1 stimulus value and 1 S-parameter (total of 3 values).

Stim  Real (Sxx)  Imag(Sxx)

*.s2p Files
Each record contains 1 stimulus value and 4 S-parameters (total of 9 values)

Stim  Real (S11)  Imag(S11)  Real(S21)  Imag(S21)  Real(S12)  Imag(S12)  Real(S22)  Imag(S22)

*.s3p Files
Each record contains 1 stimulus value and 9 S-parameters (total of 19 values)

Stim   Real (S11)  Imag(S11)  Real(S12)  Imag(S12)  Real(S13)  Imag(S13)

          Real (S21)  Imag(S21)  Real(S22)  Imag(S22)  Real(S23)  Imag(S23)

          Real (S31)  Imag(S31)  Real(S32)  Imag(S32)  Real(S33)  Imag(S33)

*.s4p Files
Each record contains 1 stimulus value and 16 S-parameters (total of 33 values)

Stim   Real (S11)  Imag(S11)  Real(S12)  Imag(S12)  Real(S13)  Imag(S13)  Real(S14)  Imag(S14)

          Real (S21)  Imag(S21)  Real(S22)  Imag(S22)  Real(S23)  Imag(S23)  Real(S24)  Imag(S24)

          Real (S31)  Imag(S31)  Real(S32)  Imag(S32)  Real(S33)  Imag(S33)  Real(S34)  Imag(S34)

          Real (S41)  Imag(S41)  Real(S42)  Imag(S42)  Real(S43)  Imag(S43)  Real(S44)  Imag(S44)

This pattern continues for N ports involved in the measurement.
```



