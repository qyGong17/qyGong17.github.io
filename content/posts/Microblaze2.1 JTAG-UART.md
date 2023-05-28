---
title: "Microblaze2.1 JTAG-UART"
date: 2023-05-25
draft: False
math: katex
summary: 
tags: ["microblaze"]
---


Using `Vivado 2018.3` . 

# Block Diagram
Changes to [Microblaze1 Hello World LED | QY's Notes](https://qygong17.github.io/posts/microblaze1-hello-world-led/): 
- An AXI Interrrupt Controller
- Another AXI_GPIO IP, `gpio_key`
	- enable GPIO interrupt
	- the interrupt output is connected to AXI Interrrupt Controller 
- MDM module, enable `JTAG UART`

![](/images/img_2023-05-25-2.png)

## Microblaze
- Enable `interrupt controller`
- Increase Local Memory to `128 KB`

![](/images/img_2023-05-25.png)

## Key GPIO 
Enable interrupt

![](/images/img_2023-05-25-3.png)

# SDK 


## BSP Settings
Settings to direct `print()/xil_printf()` output to `JTAG UART` (nomarlly, these are the default, since only one UART is present here). 

![](/images/img_2023-05-25-6.png)


![](/images/img_2023-05-25-4.png)

![](/images/img_2023-05-25-5.png)

## Sample Code
Just a helloworld... 

```C
#include <stdio.h>
#include "platform.h"
#include "xil_printf.h"

int main()
{
    init_platform();
    
    print("Hello World\n\r");

    cleanup_platform();    
    return 0;
}

```

## Debug
The output is printed in the `Console`. 

![](/images/img_2023-05-25-1.png)


`JTAG UART` could be useful for debugging. However, sending stuff to it seems compicated. 
For example, in this link: [MicroBlaze bare metal JTAG UART how do I receive/input/stdin? (xilinx.com)](https://support.xilinx.com/s/question/0D52E00006hpenSSAQ/microblaze-bare-metal-jtag-uart-how-do-i-receiveinputstdin?language=en_US)

I am trying to number these posts based on the hardware platform, i.e., the block diagram in Vivado. So the `y-th` application from the `x-th` block diagram would be `x.y`. 

My end goal here is just to control an iic device with MicroBlaze, but I will firstly work on modules such as interrupt, timer and uart, because I find the AXI_IIC IP and the APIs a bit confusing and seem to require some other IPs such as the interrupt. 

[ug940-vivado-tutorial-embedded-design.pdf](https://docs.xilinx.com/v/u/2017.2-English/ug940-vivado-tutorial-embedded-design)

[MicroBlaze Processor Quick Start Guide (xilinx.com)](https://www.xilinx.com/support/documents/quick_start/microblaze-quick-start-guide.pdf)