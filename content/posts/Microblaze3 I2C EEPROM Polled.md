---
title: "Microblaze3 I2C EEPROM Polled"
date: 2023-05-31
draft: False
math: katex
summary: AXI_IIC IP and EEPROM operation.
tags: ["microblaze"]
---





Using `Vivado 2018.3` . 

# I2C EEPROM operation

I have `xx24C64` on my board:
- 8 KB (64 Kb) space
- Address range 0~0x1FFF
- Device address `7’b101_0011`, `0x53` (on my board, A2=0, A1=A0=1)

Datasheet: [CAT24C64 - 64 Kb I2C CMOS Serial EEPROM (onsemi.com)](https://www.onsemi.com/pdf/datasheet/cat24c64-d.pdf)

## Byte Write
![](/images/img_2023-05-31.png)

## Selective Read
![](/images/img_2023-05-31-1.png)


# Vivado Block Design
Except for the  `AXI_IIC` IP, other settings are the same as the block design in [Microblaze2.1 JTAG-UART | QY's Notes](https://qygong17.github.io/posts/microblaze2.1-jtag-uart/).

![](/images/img_2023-05-31-3.png)


# I2C Polled-mode

From xiic documentation, [iic: Main Page](https://xilinx.github.io/embeddedsw.github.io/iic/doc/html/api/index.html):
> This driver does not provide a polled mode of operation primarily because polled mode which is non-blocking is difficult with the amount of interaction with the hardware that is necessary.

So, at first, I thought interrupt was necessary... Until I find this example: `xiic_low_level_eeprom_example.c`

Since I only wanted to configure an I2C slave device, polled-mode with blocking is fine. 
Basically, I just need `XIic_Recv()` and `XIic_Send()`. 

And the job was done. I guess I won't be using microblaze in a while...

# Resource
- 24C64 Datasheet: [CAT24C64 - 64 Kb I2C CMOS Serial EEPROM (onsemi.com)](https://www.onsemi.com/pdf/datasheet/cat24c64-d.pdf)
- Xilinx Github : [iic: xiic\_l.c File Reference](https://xilinx.github.io/embeddedsw.github.io/iic/doc/html/api/xiic__l_8c.html)
- [AXI_IIC读写操作_axi iic_丁点的沙砾的博客-CSDN博客](https://blog.csdn.net/qq_32134427/article/details/109366542)
- [IIC Protocol and Programming Sequence (xilinx.com)](https://support.xilinx.com/s/article/1072248?language=en_US)
- [Xilinx FPGA Microblaze AXI_IIC使用方法及心得_axi iic_NjustMEMS_ZJ的博客-CSDN博客](https://blog.csdn.net/u013098336/article/details/99694145)

# Sample Code
Run result: write 0x67, read 0x67, pass. 
![](/images/img_2023-05-31-2.png)

```C
#include <stdio.h>
#include "platform.h"
#include "xil_printf.h"
#include "xiic_l.h"
#include "sleep.h"

#define IIC_BASE_ADDRESS	XPAR_IIC_0_BASEADDR

#define EEPROM_ADDRESS  0x53

int eepromWriteByte(u32 IicBaseAddress,
				u8 deviceAddress,
				int romAddr,
				u8 dataByte);

u8 eepromReadByte(u32 IicBaseAddress,
				u8 deviceAddress,
				int romAddr);

int main()
{

	u8 byteSend;
	u8 byteRecv;
	int eepromAddr;

    init_platform();

    eepromAddr = 0x0123;
    byteSend = 0x67;
    eepromWriteByte(IIC_BASE_ADDRESS, EEPROM_ADDRESS,
    		eepromAddr, byteSend);

    usleep(6000); // delay >5ms
    // wait for eeprom internal write cycle

    byteRecv = eepromReadByte(IIC_BASE_ADDRESS, EEPROM_ADDRESS, eepromAddr);

    xil_printf("Received: 0x%X\n\r", byteRecv);
    if( byteRecv == byteSend)
    	xil_printf("EEPROM byte wirte and read OK.\n\r");

    cleanup_platform();
    return 0;
}


int eepromWriteByte(u32 IicBaseAddress,
				u8 deviceAddress,
				int romAddr,
				u8 dataByte)
{
	int bitCount;
	u8 writeBuffer[3];

	writeBuffer[0] = (u8)(romAddr >> 8); //higher 8 bits
	writeBuffer[1] = (u8)(romAddr & 0x00FF); //lower 8 bits
	writeBuffer[2] = dataByte;

	bitCount = XIic_Send(IicBaseAddress, deviceAddress,
			writeBuffer, 3, XIIC_STOP);
	return bitCount;
}

u8 eepromReadByte(u32 IicBaseAddress,
				u8 deviceAddress,
				int romAddr)
{

	u8 recvData;
	int bitCount;
	u8 writeBuffer[2];

	writeBuffer[0] = (u8)(romAddr >> 8); //higher 8 bits
	writeBuffer[1] = (u8)(romAddr & 0x00FF); //lower 8 bits

	bitCount = XIic_Send(IicBaseAddress, deviceAddress,
			writeBuffer, 3, XIIC_REPEATED_START);

	bitCount = XIic_Recv(IicBaseAddress, deviceAddress,
				&recvData, 1, XIIC_STOP);

	return recvData;
}
```


