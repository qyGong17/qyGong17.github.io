---
title: "Python script for publishing blog posts from Obsidian"
date: 2023-05-18
draft: False
math: katex
summary: A simple script
tags: ["Obsidian", "scripts"]
---



# Introduction
Recently, I use Obsidian to write almost everything, including blog posts. I mainly use images and math in my posts. (I used to use PicGo to upload my images to a repo, now I started to store images locally.)
So I wrote a simple Python script to modify my Obsidian markdown posts for publishing. 

- There are still many bugs (for example, when the wiki link is inside a code block), I guess, but it works for my recent posts. 

1. Input a file name
2. Search for it in the blog folder, `vault/Blogs`
3. Copy the mentioned image files to the image path, `repo/static/images`. 
4. Do something to the `.md` file: 
	1. Convert wiki image links to markdown links `![](/images/fname)`
	2. `\\\` -> `\\\\` , or the (all the `\\\` will be changed, though, not just in math, but I )
	3. Remove the comments at the beginning of my file ``
	4. Write the new file to the post path, `repo/content/posts`

# Script
```python
# -*- coding: utf-8 -*-
import re
import shutil
import os
import glob

# Get the input file path
fname = input('Enter the file name (without .md):')
f_fullname =  fname + '.md'
src_file_path = 'vault/Blogs/**/' + f_fullname
file_list = glob.glob(src_file_path,recursive = True)

blog_img_dir = 'repo/static/images'
blog_content_dir = 'repo/content/posts'
src_img_dir = 'vault/images'

# For simplicity, 
# the blog file name should be unique
# or the script will exit
if len(file_list)>1:
	print('More than one file with the same name.')
	input('Press Enter to exit...')
	os._exit(1)
elif len(file_list) == 0:
	print(f'{f_fullname}: No file found.')
	input('Press Enter to exit...')
	os._exit(1)
	
# if file name is unique, continue:
with open(file_list[0], 'r',  encoding='utf-8') as f:
    # Read the entire contents of the file as a string
    text = f.read()
    f.close()

# Find all wiki style attachment file names using re
# such as `[[image.jpg]]`
pattern = r'!\[\[(.*?)\]\]'
file_names = re.findall(pattern, text)

# Move each file to the target directory
for file_name in file_names:
    src_path = src_img_dir + '/' + file_name  
    img_dest_path = blog_img_dir + '/' + file_name
    
    # Use shutil.move() to copy the file
    shutil.copy(src_path, img_dest_path)
    print(f'Moved {file_name} to {img_dest_path}')

# Replace all occurrences of [[file name]] with [](file name)
# Hogo display images in /static/images/: `![](/images/a.jpg)`
new_text = re.sub(pattern, r'![](/images/\1)', text)

# for multiline math, `\\\` -> `\\\\`
new_text = re.sub(r'\\\\\\', r'\\\\\\\\\', new_text)

# remove markdown comment ``. 
new_text = re.sub(r'', '', new_text) 

blog_dest_path = blog_content_dir + '/' + f_fullname
with open(blog_dest_path, 'w',  encoding='utf-8') as f:
    # Write the modified string to a new file
    f.write(new_text)
    f.close()

print(f'Write {blog_dest_path}')	
input('Press Enter to exit...')
```


# Some links
[How to use Glob() function to find files recursively in Python? - GeeksforGeeks](https://www.geeksforgeeks.org/how-to-use-glob-function-to-find-files-recursively-in-python/)
[How can I write a regex which matches non greedy? - Stack Overflow](https://stackoverflow.com/questions/11898998/how-can-i-write-a-regex-which-matches-non-greedy?noredirect=1&lq=1)
[devidw/obsidian-to-hugo: Process Obsidian notes to publish them with Hugo](https://github.com/devidw/obsidian-to-hugo)

