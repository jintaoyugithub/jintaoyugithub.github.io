---
title: Markdown Notes
date: 2023-12-21 12:11:01
tags:
mathjax: true
---

## Basic font setting 

`* *`: for *italic* word

`** **`: for ***bold*** word

` `` `: for `hightlight` word

`~~ ~~`: for ~~strikethrough~~ word

`<font color='color scheme'> text </font>`: for change <font color='red'> color </font> 

# headline 1
## headline 2 
### headline 3
#### headline 4
##### headline 5
###### headline 6

## links
### url link
`[link description](url)`: for [url link](https://www.google.com/) 
### image link
`![image description](path/to/the/image)`: for insert a image into current file
### reference url/image link
`[reference link]: url`: for if you have multiple links to the same webpage, e.g. 

[here is a link to google][linkToGoogle]

[here is a link to youtube][linkToYoutube]

[here is a link also to google][linkToGoogle]

don't forget to write these lines at the bottom of the document

`[linkToGoogle]: https://www.google.com/` 

`[linkToYoutube]: https://www.youtube.com/` 

[linkToGoogle]: https://www.google.com/
[linkToYoutube]: https://www.youtube.com/

## text

`>`:   
> for block quotes, keep in mind that it only works when > is placed at the beginning of a line

`> in multiple lines` 
> if you have multiple line need to be blocked
>
> then you placed the caret character at the beginning of each line
> 
> even blank lines should be included

`* list name`: for create an unordered list
* cpp
* python
* opengl
* rust

`num list name`: for create an ordered list
1. cpp
2. python
3. opengl
4. rust

```
*
 * : to add sub-lists to the main lists
```
* programing language
    * cpp
    * python

`inserting two spaces after each new line`: for formatting paragraphs

This is paragraphs without two spaces after each new line

but forcefully insert a new line

This is paragraphs witht two spaces after each new line  
and without inserting a new line  

## math
`$ $`: for in-line math, e.g. $f(x) = x^2$

`$$ $$`: for math blocks
{% mathjax %}
    \int_0^\infty \frac{x^3}{e^x-1}\,dx = \frac{\pi^4}{15}
{% endmathjax %}

`\\`: for start a new line in math blocks

{% mathjax %}
a_1=b_1+c_1\\
a_2=b_2+c_2-d_2+e_2
{% endmathjax %}


