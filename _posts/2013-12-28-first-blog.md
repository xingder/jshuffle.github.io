---
layout: post
title: "测试post"
categories: 
tags: 
---

```html
<!DOCTYPE html>
<html>
  <head>
    {% include html_meta %}
    <title>How to use MathJax in Jekyll generated Github pages -- Haixing Hu's Homepage</title>
    {% if page.use_math %}
      {% include mathjax_support %}
    {% endif %}
  </head>
  <body>
    {% include navigation_bar %}
    <div class="container-narrow">
      <div class="content">
        {{ content }}
      </div>
      <hr/>
      {% include footer %}
    </div>
    {% include JB/analytics %}
  </body>
</html>
```

测试用。

代码块：

```
debug=TRUE
if debug:
	print('hello word')
```

公式块：


$$
\text{for OTP : $\qquad$ if }\quad E(k,\:m)=c\\
\begin{align}
k\oplus m &= c \\
k &= m\oplus c
\end{align}
\\
\#\{\;k \in \mathscr K : \quad E(k,\:m)=c \;\}=1 \quad \forall m,\:c
$$



### 这是标题栏

Throughout this guide there are a number of small-but-handy pieces of information that can make using Jekyll easier, more interesting, and less hazardous. Here’s what to look out for.

我的个人博客：

`jshuffle.github.io`

