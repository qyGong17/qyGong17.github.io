---
title: "Test math and figures"
date: 2023-05-13T18:42:24+08:00
draft: False
math: katex
---

# Math
Typeset math from: [Writing math with Hugo | Misha Brukman](https://misha.brukman.net/blog/2022/04/writing-math-with-hugo/)

test inline: $\psi_{rq} =L_ri_{rq}- \frac{L_{M}^{2}}{L_{s}}i_{rq}$

test align: 
use 6 backslashes `\\\\\\` instead of 2 `\\`, it works, but is odd...

$$
\begin{aligned}
 u_{rd} &= R_ri_{rd}+ \sigma L_{r}\frac{d i_{rd}}{dt}-\omega_{s1}\sigma L_{r}i_{rq}+ \left(\frac{L_{M}}{L_{s}}\frac{d}{dt}\psi_{s}\approx 0 \right)\\\\\\
 u_{rq} &= R_ri_{rq}+\sigma L_{r}\frac{d i_{rq}}{dt}+\omega_{s1}\sigma L_{r}i_{rd}+\omega_{s1}\frac{L_{M}}{L_{s}} \psi_s
\end{aligned}
$$

# Figure
A figure from my oss, Markdown syntax `![](myoss.com/1.png`) works:
![](https://pics1237.oss-cn-shenzhen.aliyuncs.com/20230427223520.png)

A local figure, `![](/images/test_image.png)`, while the actual image file is in `\static\images`: 
![](/images/test_image.png)

Use local image and tips: [Rendering Images in Markdown Preview of Hugo Site | Mike F. Robbins (mikefrobbins.com)](https://mikefrobbins.com/2023/02/08/rendering-images-in-markdown-preview-of-hugo-site/)


# Github Pages and deploy

[GitHub Pages](https://pages.github.com/)


[Host on GitHub | Hugo (gohugo.io)](https://gohugo.io/hosting-and-deployment/hosting-on-github/)


# Reference

Started this blog from this tutorial (internal access):
[使用ZJU Git快速搭建个人博客 - CC98论坛](https://www.cc98.org/topic/5473811)