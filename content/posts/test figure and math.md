---
title: "Test math, figure, tags, toc"
date: 2023-05-14
draft: False
math: katex
tags: ["test Hugo and PaperMod"]
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
A figure from my oss, Markdown syntax `![](myoss.com/1.png)` works:
![](https://pics1237.oss-cn-shenzhen.aliyuncs.com/20230427223520.png "")

A local figure, `![](/images/test_image.png)`, while the actual image file is in `\static\images`: 
![/images/test_image.png](/images/test_image.png)

Use local image and tips: [Rendering Images in Markdown Preview of Hugo Site | Mike F. Robbins (mikefrobbins.com)](https://mikefrobbins.com/2023/02/08/rendering-images-in-markdown-preview-of-hugo-site/)


Switched to PicGo with Github repo, also works:

![](https://raw.githubusercontent.com/qyGong17/figs0/main/img/20230514090029.png)

# Github Pages and deploy

[GitHub Pages](https://pages.github.com/)


[Host on GitHub | Hugo (gohugo.io)](https://gohugo.io/hosting-and-deployment/hosting-on-github/)


# Code blocks

```cpp
module pwm_led
(
input clk,
input reset_n,
output led
);
```



# Other settings

demo: 
[PaperMod (adityatelange.github.io)](https://adityatelange.github.io/hugo-PaperMod/)

source: 
[adityatelange/hugo-PaperMod at exampleSite (github.com)](https://github.com/adityatelange/hugo-PaperMod/tree/exampleSite)

## Tags

https://discourse.gohugo.io/t/how-to-add-tag-and-category/3202

yaml: `tags: ["test Hugo and PaperMod"]`


`config.yml`: 
```
taxonomies:
  category: categories
  tag: tags
  series: series 
```

## toc
Many thanks to: [Hugo侧边目录 | 3rd's Blog](https://333rd.net/posts/tech/hugo%E4%BE%A7%E8%BE%B9%E7%9B%AE%E5%BD%95/#:~:text=PaperMod,%E4%BF%AE%E6%94%B9%E4%B8%BA%E4%BE%A7%E8%BE%B9%E7%9B%AE%E5%BD%95%E3%80%82)

and 

[Hugo博客目录放在侧边 | PaperMod主题 | Sulv's Blog (sulvblog.cn)](https://www.sulvblog.cn/posts/blog/hugo_toc_side/)


# Reference

Started this blog from this tutorial (internal access):
[使用ZJU Git快速搭建个人博客 - CC98论坛](https://www.cc98.org/topic/5473811)