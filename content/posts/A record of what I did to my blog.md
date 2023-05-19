---
title: "A record of what I did to my blog"
date: 2023-04-01
draft: False
math: katex
tags: ["test Hugo and PaperMod"]
---

Since I don't know the first thing about web design, all of the modifications are from Google or other bloggers. The source will be included. Thanks in advance. 

## 2023-05-15 
### Organizing content
I planned a few blog posts for the basic modelling and control of PMSG. I wanted to put all the relevant posts into a folder, such as `content/post/PMSG`. 

- Documentation: [Content Organization | Hugo (gohugo.io)](https://gohugo.io/content-management/organization/)
- Demo: [hugo-PaperMod/content/posts/papermod](https://github.com/adityatelange/hugo-PaperMod/tree/exampleSite/content/posts/papermod)
- Source of the demo: [PaperMod (adityatelange.github.io)](https://adityatelange.github.io/hugo-PaperMod/)

Summary: create `content/post/PMSG/_index.md`. (It has to be `_index.md`) And  `_index.md` will not be listed in posts. 

## 2023-05-18
### Code display on mobile devices
-  [[BUG] On iOS mobile phone, the code block font size is not the same · Issue #828 · adityatelange/hugo-PaperMod (github.com)](https://github.com/adityatelange/hugo-PaperMod/issues/828#issuecomment-1171994855)

`assets/css/extended/custom.css`
```css
/* Fixes iOS font sizing anomaly */
code {
    text-size-adjust: 100%;
    -ms-text-size-adjust: 100%;
    -moz-text-size-adjust: 100%;
    -webkit-text-size-adjust: 100%;
}
```


## 2023-05-19
### Text color
I wanted more contrast between different texts, mainly, inline code, `code`, and urls like: [test url](www.blank.com). 
Added `repo/assets/css/extended/text-color.css`:
```css
.post-content code {
    color: rgb(245, 8, 8);
}  /* for inline code `code`*/
.post-content a{
    color: blue;
} /* for urls [name](url)*/
/* see PaperMod/assets/css/common/post-single.css*/
```
- Learned from this discussion: [In-line code highlight - support - HUGO (gohugo.io)](https://discourse.gohugo.io/t/in-line-code-highlight/43744)