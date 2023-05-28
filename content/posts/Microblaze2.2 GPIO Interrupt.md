---
title: "Microblaze2.2 GPIO Interrupt"
date: 2023-05-26
draft: False
math: katex
summary: 
tags: ["microblaze"]
---







# Hardware Platform
Same hardware platform as in [Microblaze2.1 JTAG-UART | QY's Notes](https://qygong17.github.io/posts/microblaze2.1-jtag-uart/)

The `key_gpio` binds to a button on the board. 

# Configurations
The codes are modified from the interrupt example: `xgpio_intr_tapp_example.c`. (I found these examples a bit too complicated for a beginner like me.) 


Basically: 
- Initialize the `GPIO device`
- Initialize the `interrupt device`
- Bind the GPIO interrupt handler function
- Enable interrupt (both in the GPIO device and the interrupt device)
- Wait for the interrupt

# Debug
Every time the key is pressed, the LEDs are toggled, and a message is printed through `JTAG UART` . 

![](/images/img_2023-05-26.png)


# Sample Code
```C
#include <stdio.h>
#include "platform.h"
#include "xil_printf.h"
#include "xgpio.h"
#include "xintc.h"
#include "xil_exception.h"

// use two GPIO modules, both with only channel 1
#define GPIO_CHANNEL1 1
#define DIR_OUTPUT 	0
#define DIR_INPUT 	0x0001
#define INTC_DELAY 2000000

// the width of key_gpio is 1,
// so only the first bit is used.
#define KEY_GPIO_BIT 0x0001

void gpio_handler(void *CallBackRef);
void init_gpio();
void init_gpio_intr();

int Led_on;
int IntrCnt;
XGpio led_gpio;
XGpio key_gpio;
XIntc intc;

int main()
{

    init_platform();

    init_gpio(); // initialize key and led gpio
    init_gpio_intr(); // initialize interrupt

    Led_on = 0;
    IntrCnt = 0;
    print("Receiving key interrupts.\n\r");
    while(1){

    }
    //cleanup_platform();
    return 0;
}


void init_gpio()
{
	int status;

	// XPAR_GPIO_LED_DEVICE_ID: see xparameters.h
	// initialize led_gpio as all output
	status = XGpio_Initialize(&led_gpio, XPAR_GPIO_LED_DEVICE_ID);
	if(status != XST_SUCCESS) //if status!=0
	{
		printf("Gpio LED Initialization Failed\r\n");
		return XST_FAILURE; //return 1
	}

	XGpio_SetDataDirection(&led_gpio, GPIO_CHANNEL1, DIR_OUTPUT); //all bits as output

	//initialize key_gpio as input
	status = XGpio_Initialize(&key_gpio, XPAR_GPIO_KEY_DEVICE_ID);
	if(status != XST_SUCCESS) //if status!=0
	{
		printf("Gpio KEY Initialization Failed\r\n");
		return XST_FAILURE; //return 1
	}

	XGpio_SetDataDirection(&key_gpio, GPIO_CHANNEL1, DIR_INPUT); //set the last bit as input

}


void init_gpio_intr()
{

	int status;

	//initialize the interrupt controller, intc
	status = XIntc_Initialize(&intc, XPAR_MICROBLAZE_0_AXI_INTC_DEVICE_ID);
	if(status != XST_SUCCESS) //if status!=0
	{
		printf("INTC Initialization Failed\r\n");
		return XST_FAILURE; //return 1
	}

	/* Hook up interrupt service routine */
	XIntc_Connect(&intc, XPAR_INTC_0_GPIO_0_VEC_ID,
		      (Xil_ExceptionHandler)gpio_handler, &key_gpio);

	/* Enable the interrupt vector at the interrupt controller */
	XIntc_Enable(&intc, XPAR_INTC_0_GPIO_0_VEC_ID);

	/*
	 * Start the interrupt controller such that interrupts are recognized
	 * and handled by the processor
	 */
	status = XIntc_Start(&intc, XIN_REAL_MODE);
	if (status != XST_SUCCESS) {
		printf("INTC start Failed\r\n");
		return XST_FAILURE; //return 1
	}
	//enable GPIO interrupt for the first bit in key_gpio
	XGpio_InterruptEnable(&key_gpio, KEY_GPIO_BIT);
	XGpio_InterruptGlobalEnable(&key_gpio);

	/*
	 * Initialize the exception table and register the interrupt
	 * controller handler with the exception table
	 */
	Xil_ExceptionInit();

	Xil_ExceptionRegisterHandler(XIL_EXCEPTION_ID_INT,
			 (Xil_ExceptionHandler)XIntc_InterruptHandler, &intc);

	/* Enable non-critical exceptions */
	Xil_ExceptionEnable();

}


void gpio_handler(void *CallBackRef)
{
	XGpio *GpioPtr = (XGpio *)CallBackRef;
	int delay;

	IntrCnt ++;

	// disable the gpio interrupt for a while
	// a very simple key debounce
	XGpio_InterruptDisable(&key_gpio, KEY_GPIO_BIT);
	for(delay=0;delay < INTC_DELAY; delay++); //introduce some delay
	XGpio_InterruptEnable(&key_gpio, KEY_GPIO_BIT);

	// toggle the LEDs every time gpio interrupt is triggered
	if(Led_on)
	{
		Led_on = 0;
		//GPIO output 0
		XGpio_DiscreteWrite(&led_gpio, GPIO_CHANNEL1, 0x00);
	}
	else
	{
		Led_on = 1;
		//GPIO output 1
		XGpio_DiscreteWrite(&led_gpio, GPIO_CHANNEL1, 0x0F);
	}

	//should use xil_printf() instead of print()
	xil_printf("Key pressed count: %d.\n\r", IntrCnt);
	/* Clear the Interrupt */
	XGpio_InterruptClear(GpioPtr, KEY_GPIO_BIT);
}

```


