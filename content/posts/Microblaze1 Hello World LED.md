---
title: "Microblaze1 Hello World LED"
date: 2023-05-20
draft: False
math: katex
summary: 
tags: ["microblaze"]
---



# 1 Introduction
This post is a step-by-step guide implementing a blinking LED project with MicroBlaze:
- Create a minimal MicroBlaze project
- Sample code for blinking LED (based on a Xilinx demo)
- Implementation on the demo board

I am new to FPGA and MicroBlaze, and I learned these all from the tutorials I found. References are at the end. 

I used: 
- Device: xc7a35tfgg484-2, (an Artix-7 Mini board made by a Chinese company, Wildfire)
- Vivado 2018.3 

# 2 In Vivado
New RTL project and select the correct device. 

## 2.1 New Block Design
IP integrator-Create Block Design.
![](/images/img_2023-05-20-2.png)

## 2.2 Add MicroBlaze IP
![](/images/img_2023-05-20-4.png)

Run Block Automation: 
- Choose `New Clocking Wizard`, because the board has a 50 MHz input clock, and the microblaze core seems to need a 100 MHz clock. 
- Other setting default. 
![](/images/img_2023-05-20-5.png)

After block automation, a few other IPs are added and connected:
![](/images/img_2023-05-20-7.png)


## 2.3 Clocking wizard 
![](/images/img_2023-05-20-6.png)
- `Input Clock-Source-single ended clock capable` , also, correct frequency, `50 MHz`. 
- `Output Clocks-Reset Type-select Active Low`, to matching the schematics below (`KEY1` is Low when the button is pushed): 
![](/images/img_2023-05-20-8.png)

## 2.4 Connection Automation
`Run Connection Automation`: 
![](/images/img_2023-05-20-21.png)
(I don't know how this button can be accessed from the menu...)


Input ports for `reset` and `clock` are added automatically: 
- The generated port name was `clk_100MHZ`, I changed it to `clk_50MHZ`, to avoid confusion
- Two reset ports were generated, and I merged them into one. 
![](/images/img_2023-05-20-11.png)

> `Regenerate Layout`: useful button to organize the schematics. 

![](/images/img_2023-05-20-10.png)


## 2.5 Add AXI_GPIO
Add `AXI_GPIO` IP.
I have 4 user LEDs on the board, so I selected `GPIO Width=4`, and `All Outputs`. 
![](/images/img_2023-05-20-12.png)


`Run Connection Automation` again, GPIO IP is automaticly connected to the Microblaze system, meanwhile, an output port was added to the block dragram. 
![](/images/img_2023-05-20-13.png)

## 2.6 Validate Design
![](/images/img_2023-05-20-14.png)
Successful. 

## 2.7 Create wrapper (top level design)
Automaticaly generate a top level design for the project. 
![](/images/img_2023-05-20-15.png)
The final structure: `top-mb1-all the IPs`
![](/images/img_2023-05-20-16.png)

The generated wrapper: 
```verilog
module mb1_wrapper
   (clk_50MHz,
    led_tri_o,
    reset_rtl_0);
  input clk_50MHz;
  output [3:0]led_tri_o;
  input reset_rtl_0;

  wire clk_50MHz;
  wire [3:0]led_tri_o;
  wire reset_rtl_0;

  mb1 mb1_i
       (.clk_50MHz(clk_50MHz),
        .led_tri_o(led_tri_o),
        .reset_rtl_0(reset_rtl_0));
endmodule
```

## 2.8 Define Contraints

1. Run Synthesis
2. From the top menus, `layout-I/O planning`
3. Assign correct `package pins` and `I/O standard` for reset button, (read the schematics or documentations of your board)
4. Save the constraint `.xdc` file. 

![](/images/img_2023-05-20-17.png)

generated `.xdc` file :
```
set_property PACKAGE_PIN W19 [get_ports clk_50MHz]
set_property IOSTANDARD LVCMOS33 [get_ports clk_50MHz]
set_property PACKAGE_PIN N20 [get_ports {led_tri_o[0]}]
set_property PACKAGE_PIN M20 [get_ports {led_tri_o[1]}]
set_property PACKAGE_PIN N22 [get_ports {led_tri_o[2]}]
set_property PACKAGE_PIN M22 [get_ports {led_tri_o[3]}]
set_property IOSTANDARD LVCMOS33 [get_ports {led_tri_o[3]}]
set_property IOSTANDARD LVCMOS33 [get_ports {led_tri_o[2]}]
set_property IOSTANDARD LVCMOS33 [get_ports {led_tri_o[1]}]
set_property IOSTANDARD LVCMOS33 [get_ports {led_tri_o[0]}]
set_property IOSTANDARD LVCMOS33 [get_ports reset_rtl_0]
set_property PACKAGE_PIN R16 [get_ports reset_rtl_0]
```

## 2.9 Bitstream Generation
All default settings: 
1. Implementation
2. Generate bitstream


## 2.10 Export Hardware
File-export-export Hardware with default settings. 

![](/images/img_2023-05-20-18.png)

## 2.11 Open SDK
File-Launch SDK 

# 3 In SDK
## 3.1 New Application Project
2. In SDK, New-Application Project
3. Finish, create an empty project

![](/images/img_2023-05-20-19.png)

## 3.2 Add Sample Code
Add `main.c` at `src`  directory.
![](/images/img_2023-05-20-22.png)

main.c:
```c
#include <stdio.h>
#include "xil_printf.h"
#include "xgpio.h"
#include "xparameters.h" //for device id

XGpio led_gpio;

#define LED_DELAY 	5000000
#define LED_CHANNEL 1
#define DIR_OUTPUT 	0

int main()
{
	volatile int counter;
	int status;

	// XPAR_GPIO_LED_DEVICE_ID: see xparameters.h
	// GPIO_LED is the name of the AXI_GPIO IP from Vivado
	status = XGpio_Initialize(&led_gpio, XPAR_GPIO_LED_DEVICE_ID);
	if(status != XST_SUCCESS) //if status!=0
	{
		//comment out xil_printf, because the RAM was small, 8KB
		//xil_printf("Gpio Initialization Failed\r\n");
		return XST_FAILURE; //return 1
	}

	XGpio_SetDataDirection(&led_gpio, LED_CHANNEL, DIR_OUTPUT); //all bits as output

	while(1)
	{
		// Set the GPIO bits to High
		XGpio_DiscreteWrite(&led_gpio, LED_CHANNEL, 0x0F);

		for(counter=0; counter<LED_DELAY;counter++); //delay

		// Set the GPIO bits to Low
		XGpio_DiscreteWrite(&led_gpio, LED_CHANNEL, 0x00);

		for(counter=0; counter<LED_DELAY;counter++); //delay
	}

    return XST_SUCCESS;
}

```

- For initialization of GPIO, refer to `xgpio_example.c`: [gpio: xgpio_example.c File Reference](https://xilinx.github.io/embeddedsw.github.io/gpio/doc/html/api/xgpio__example_8c.html#a840291bc02cba5474a4cb46a9b9566fe)
- For device id, look for `GPIO_LED` (the name of `AXI_GPIO` ip from Vivado Block Design) in `xparameters.h`:
```c
#define XPAR_GPIO_LED_DEVICE_ID 0
#define XPAR_GPIO_0_DEVICE_ID XPAR_GPIO_LED_DEVICE_ID
```

## 3.3 Debug

### 3.3.1 Run Configuration 

![](/images/img_2023-05-20-23.png)

![](/images/img_2023-05-20-24.png)

### 3.3.2 Build Project
Project-Build All

### 3.3.3 Program FPGA

Right click Project `led`-Run as-Launch on Hardware. 

After a few seconds, the LEDs should be blinking. 
![](/images/img_2023-05-20-25.png)

Basically, the SDK is just like other IDEs for ARM or TI DSP, though I haven't used these in a while, either. 

# 4 Links
Maybe I should have used a newer Vivado version: 
> Starting in the **2019.2 release**, the Xilinx SDK development environment is unified into an all-in-one Vitis™ unified software platform.

1. Most of the steps are from this video: [Digilent Nexys: Microblaze and GPIO Design Implementation - YouTube](https://www.youtube.com/watch?v=nxPOGmTDLns)
2. Another toturial: [MicroBlaze Quickstart Video](https://www.xilinx.com/video/software/microblaze-quickstart-video.html)
3. volatile int: [declaration - Why is volatile needed in C? - Stack Overflow](https://stackoverflow.com/questions/246127/why-is-volatile-needed-in-c/)
	1. `volatile` tells the compiler not to optimize anything that has to do with the `volatile` variable.
4. [MicroBlaze - section .text will not fit in region (xilinx.com)](https://support.xilinx.com/s/question/0D52E00006hpZUBSA2/microblaze-section-text-will-not-fit-in-region?language=en_US)
5. [Step By Step Guide To Xilinx SDK Project Migration To Vitis](https://support.xilinx.com/s/article/1050061?language=en_US#:~:text=Starting%20in%20the%202019.2%20release,Vitis%E2%84%A2%20unified%20software%20platform.)
6. [gpio: xgpio_example.c File Reference](https://xilinx.github.io/embeddedsw.github.io/gpio/doc/html/api/xgpio__example_8c.html#a840291bc02cba5474a4cb46a9b9566fe)
	1. search for `xgpio_example.c` inside the Vivado installation directory. 