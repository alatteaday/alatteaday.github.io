---
layout: post
title:  "[Trouble Shooting] When mathematical expression syntax isn't applying on GitHub Pages"
categories: ['Error Resolution']
lang: en
tags: gitio markdown
---



I wrote a math expression in a GitHub blog post, but there was an issue with applying markdown syntax. I'm posting this to document the solution that I applied. 



## 1. Modify the _config.yml file

Check and modify the markdown-related settings in the _config.yml file like below. If they don't exist, add them like below. It's recommended to set the markdown engine to kramdown.

```yaml
markdown: kramdown
highlighter: rouge
lsi: false
excerpt_separator: "\n\n"
incremental: false
```



## 2. Write a HTML file of math expression syntax within the _includes folder

Generally, GitHub blogs contain an _include folder. Write a script within this folder to enable math expression syntax to be applied to posts. Let's assume creating a file named 'math.html'

```html
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
        TeX: {
          equationNumbers: {
            autoNumber: "AMS"
          }
        },
        tex2jax: {
        inlineMath: [ ['$', '$'] ],
        displayMath: [ ['$$', '$$'], ['\\[', '\\]'] ],
        processEscapes: true,
      }
    });
    MathJax.Hub.Register.MessageHook("Math Processing Error",function (message) {
          alert("Math Processing Error: "+message[1]);
        });
    MathJax.Hub.Register.MessageHook("TeX Jax - parse error",function (message) {
          alert("Math Processing Error: "+message[1]);
        });
</script>
<script type="text/javascript" async
    src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
```

You can set each math syntax mark for the ```inlineMath``` and ```displayMath```. Similar to the ```displayMath``` item in the above code, you can specifiy multiple marks in the list. Following the example, if you wrap the formula in ```$$``` or ```\\[``` and ``` \\]```, the math style will be displayed as the display style.

*When setting the syntax as ```\[``` and ``` \]``` instead of ```\\[``` ``` \\]```, there might be instances where ordinary text enclosed within square brackets is also treated as part of the math expression.



### Inline and Display style

The inline style and the display style are two styles of math expression. 

* Inline style: Representing math expression within a sentence without line breaks

* Display style: Generating math expression as blocks for representation

  ```
  $2$ plus $3$ is $5$: $$2+3=5$$
  ```

  $2$ plus $3$ is $5$: \\[2+3=5\\]

  

## 3. Apply the HTML script created in 2. to the post

To apply the script created above to an actual post, you'll need to modify the HTML file related to the layout. Find an appropriate file in the _layout folder and incorporate the content of 'math.html' into the section where the post's content is inserted. For example, I found and modified the 'default.html' file like the example below:

``````html
<html>
<body>
	...
  <div class="content">
    {{ content }}
    {% include math.html %}
  </div>
</body>
</html>
``````

```{{ content }}``` 의 위치에 작성한 포스트의 본문이 보여집니다. ```{% include file.html %}``` 은 'file.html'의 내용을 가져온다는 뜻입니다. 따라서 해당 블록 내에 'math.html'에서 작성한 수식 문법 사항을 적용하겠다는 의미의 코드가 됩니다. 

```{{ content }}``` displalys the main body of the post. ```{% include file.html %}``` means it includes the content of 'file.html'. Therefore, within this block, it signifies applying the math syntax written in 'math.html'

You can modify the code and adjust if applying the math syntax or not,

```html
<html>
  <body>
  	...
    <div class="content">
      {{ content }}
      {% if page.use_math %}
        {% include math.html %}
      {% end if %}
    </div>
  </body>
<html>
```

The code ```page.use_math``` being ```true``` indicates that the content of 'math.html' will be applied. Here, ```page``` refers to the 'page.html' written about the layout of each post. To set ```page.use_math```, simply add ```use_math: true``` to the Front Matter of each post.

```
---
title: ABCD
layout: post
...
use_math: true
---
```

For posts where math expressions are not needed or you prefer not to apply them, simply omit the ```use_math``` tag or set it to ```false``` 



---

### reference 

https://junia3.github.io/blog/markdown

https://an-seunghwan.github.io/github.io/mathjax-error/





