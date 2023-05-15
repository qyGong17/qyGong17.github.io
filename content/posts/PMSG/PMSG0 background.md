---
title: "PMSG0 background and learning resources"
date: 2023-05-15
draft: False
math: katex
Summary: As I say background, it is actually a learning background of myself, not necessarily that of the PMSG. 
tags: ["PMSG control"]
---


# Introduction
Recently, I learned the basic control of the doubly-fed induction generator (DFIG) through a nice step-by-step tutorial on Youtube: [DFIM Tutorial](https://www.youtube.com/watch?v=ddHO1D6_FAw&list=PLqepZUuyemmHFf9KTcc4TWzhasTjNgjMc&index=3).  

- Notes (in my other blog): [Some Notes in Chinese](https://blog.csdn.net/qq_34181877/category_12242963.html?spm=1001.2014.3001.5482)
- Simulation models: [DFIG_tutorial_Simulink](https://github.com/qyGong17/DFIG_tutorial_Simulink)

Now, It's time to turn to permanent magnet synchronous generator (PMSG). So, I started this one with a little background of DFIG. 

My goals are: 
- Basic operating principles of the PMSG
- Modelling in the abc frame and the synchronous frame
- How the model is utilized in a basic vector control scheme
- A set of simulation models in MATLAB/Simulink. 

It is almost my first time trying to create content in English (apart from a bunch of coursework I did years ago). 

I am also going to guess a lot, firstly trying to do things myself, then refer to textbooks or other sources. It may take longer, but I'm in no hurry and it just would be more fun. (Motors are fun, I guess. )

Maybe this learning process would also be interesting for another post. 

# Learning Resources
1. A nice animation of PMSM from Youtube: [Working of Synchronous Motor](https://www.youtube.com/watch?v=Vk2jDXxZIhs)
2. A series of tutorial on Youtube: [Position Signal for PMSM and FOC Modeling](https://www.youtube.com/watch?v=dA5BtNf2BDk&list=PLrdYI-lrfRRJWSpPh_le4GzqRorteWcJK)
	1. Of course, typing `PMSM` or `PMSG` on Google or Youtube would give hours of other lectures as well. 
3. A lecture in Chinese: [第十七讲 永磁同步直驱风力发电系统结构及模型推导](https://www.bilibili.com/video/BV1fL4y1G79F/?spm_id_from=333.788.recommend_more_video.-1&vd_source=391e544f8c4b215664c5081f4db16bed)