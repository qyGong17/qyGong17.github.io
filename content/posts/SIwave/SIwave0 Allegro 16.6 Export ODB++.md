---
title: "SIwave0 Allegro 16.6 Export ODB++"
date: 2023-05-23
draft: False
math: katex
summary: Export an Allegro 16.6 PCB file to ODB++, to be imported in SIwave.
tags: ["SIwave","Allegro"]
---



Recently, I learned some basic simulations with SIwave, mainly:
- DC-IR analysis
- S-paramater extraction

I will first record the steps to perform these simulations for future reference. Then, maybe, test some of the signal integrity stuff on a simple board (via, capacitor, impedance control and so on). 

I am using:
- Allegro 16.6
- Ansys SIwave 2020R2

To import to SIwave: `.brd` file-->ODB++ file-->SIwave project.

This post covers: `.brd` file-->ODB++ file

1. Install `ODB++ Inside`. 
	1. I have version `11.4` on my computer, which works for `Allegro 16.6`. But I can't find any older version on the official site for `Allegro 16.6`. So no download link will be provided here.  `ODB++_Inside_Cadence_Allegro_114_Windows_64_SA_Setup`
	2. Official site (`requires login` and only support `Allegro 17.x`): [ODB++ Inside for Cadence Allegro (Windows 64 bit) - ODB++Design (odbplusplus.com)](https://odbplusplus.com/design/download/odb-inside-for-cadence-allegro-windows-64-bit/)
2. Reboot PC to update environment variables. 
3. Put the `.brd` PCB file in a path containing `no Chinese characters` (I don't know about other languages)
4. Open PCB file. In `Allegro 16.6`, File-Export-ODB++ Inside
5. Prompts: `Do you want to extract net impedance average?` Yes, (Though I didn't see the difference)
6. After a while, ODB++ GUI pops up (images below):
	1. Input path, default, points to the `.brd` PCB file
	2. Output path, default, the path of the `.brd` file
	3. model name: enter a file name, such as `odb0523`
	4. Open ODB++ Viewer: `No`
	5. Create Archive: `.tgz`, the output would be a single compressed file, which is nicer than a folder of files. 
7. Click `Next`, and the `odb0523.tgz` would be in the selected directory as in step `6.2`, ready to be imported to SIwave. 

![](/images/img_2023-05-23-2.png)





