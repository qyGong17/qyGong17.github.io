---
title:  Obsidian templater日记模板添加一个随机问题和Templater Javascript
date: 2023-05-07
draft: False
math: katex
tags: ["Obsidian"]
---

# 简介
每天日记里写同样的东西，感觉有点无聊，想问自己一些问题，每天不同。

查到有插件`random structural diary`，我想要的功能就是这样，但是没懂这个插件怎么放进template。

想要的功能很简单，所以自己写一个：新建一个日记时，从一个自己设定的问题列表中选择一个，打印在日记中。

![](/images/20230507150708.png)

# 写一个简单的脚本
`randomDailyQuestion.js`: 
```javascript
function randomDailyQuestion() {
    let myQuestionArray = [];
    myQuestionArray.push("Question apple");
    myQuestionArray.push("Question banana");
    myQuestionArray.push("Question orange");

    let randomIndex = Math.floor(Math.random() * myQuestionArray.length);
    let randomQuestion = myQuestionArray[randomIndex];
    return randomQuestion;
}

module.exports = randomDailyQuestion
```

（语法全问GPT，比自己搜快多了）
![](/images/20230507152310.png)


# Templater设置
设置存放程序的目录：

![](/images/20230507153819.png)

# 加入template
在对应的template代码中加入刚才的脚本，比如这里是在日期下面加了这个问题：
```
# <% tp.date.now("dddd MMMM Do YYYY") %>
<% tp.user.randomDailyQuestion(tp) %>
```

# 最终结果
上面两行显示为：
![](/images/20230507154855.png)
每次会打印其中一个问题。


# 警告
注意，个人知识管理（personal knowledge management, PKM）**并不等于实际的工作**（除非你是这方面的博主）。不要花大量时间在这些花里胡哨的东西上面。**去干活！！！**

[Stop Procrastinating With Note-Taking Apps Like Obsidian, Roam, Logseq](https://www.youtube.com/watch?v=baKCC2uTbRc&t=38s)


# 参考资料
关于在Templater使用JS，这个资料很详细，Templater插件首页推荐的：
[how-to-use-templater-js-scripts - shabeblog (shbgm.ca)](https://shbgm.ca/blog/obsidian/how-to-use-templater-js-scripts)

一些可以问自己的问题：
[88 Daily Journal Prompts & Questions - Parade: Entertainment, Recipes, Health, Life, Holidays](https://parade.com/1308069/steph-nguyen/journaling-prompts/)