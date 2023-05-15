---
title: "FPGA LED呼吸灯"
date: 2023-05-06
draft: False
math: katex
tags: ["FPGA"]
---

FPGA Verilog简单的LED呼吸灯，作为新板子的亮灯程序。

这个LED在输出高电平时点亮。
主要的想法：
1. CMP_MAX限制最大占空比，因为发现在占空比较大时，改变占空比时，亮度变化不大，效果不明显。
2. PWM_CNT_MAX设置PWM频率
3. DUTY_CNT_MAX设置占空比变化的频率
4. CMP_STEP设置每次占空比变化的步长，通过改变CMP_STEP来改变呼吸灯的频率
5. flag_duty_increase指示占空比变化的方向

# 代码
```cpp
module pwm_led
(
input clk,
input reset_n,
output led
);

parameter PWM_CNT_MAX = 32'd50_000; //PWM frequency 50M/5e4=1kHz
parameter CMP_MAX = 32'd25_000; //max duty ratio CMP_MAX/PWM_CNT_MAX=0.5
parameter DUTY_CNT_MAX = 32'd500_000;
//duty step change frequency 50M/DUTY_CNT_MAX=100Hz, every 0.01s

parameter CMP_STEP = 32'd500; //duty step length 
//estimated cycle: 
//=(DUTY_CNT_MAX/50M)*(CMP_MAX/DUTY_STEP)*2
//=0.01s*50*2=1s

reg [31:0] pwm_cnt;
reg [31:0] duty_cnt;
reg [31:0] cmp;
reg flag_duty_increase;

always @(posedge clk)
begin
	if(!reset_n) 
		pwm_cnt <= 0;
	else if(pwm_cnt >= PWM_CNT_MAX-1)
		pwm_cnt <= 32'd0;
	else
		pwm_cnt <= pwm_cnt + 1;
end

always @(posedge clk)
begin
	if(!reset_n) 
		duty_cnt <= 0;
	else if(duty_cnt >= DUTY_CNT_MAX-1)
		duty_cnt <= 32'd0;
	else
		duty_cnt <= duty_cnt + 1;
end

always @(posedge clk)
begin
	if(!reset_n) begin
		flag_duty_increase <= 0;
		cmp <= 0;
	end
	else if(duty_cnt == DUTY_CNT_MAX-1) 
        if(flag_duty_increase) //increase cmp
            if(cmp >= CMP_MAX)
                flag_duty_increase <= 0;
            else
                cmp <= cmp + CMP_STEP;
        else //flag_duty_increase == 0, decrease cmp
            if(cmp < CMP_STEP)
                flag_duty_increase <= 1;
            else
                cmp <= cmp - CMP_STEP;
end

assign led = (cmp > pwm_cnt); //duty ratio = cmp/PWM_CNT_MAX
endmodule
```

# 效果
仿真不做了（亮灯程序，能用就行），上板实测效果：

![](/images/Record_2023_05_06_23_41_50_793.gif)