---
layout: post
title:  "Github.io에서 markdown 수식 문법 적용이 안될 때"
categories: ['Error Resolution']
lang: ko
tags: gitio markdown
---



Github blog 포스트에 수식을 작성했는데, markdown 수식 문법 적용이 되지 않는 문제가 있었습니다. 해결 방법을 기록해두고자 포스팅합니다.



## 1. _config.yml 파일 수정

markdown process 관련 설정을 확인하여 수정, 없으면 추가합니다. markdown engine을 kramdown으로 설정해야 한다고 합니다.

```yaml
markdown: kramdown
highlighter: rouge
lsi: false
excerpt_separator: "\n\n"
incremental: false
```



## 2. _includes 폴더 내 수식 문법 관련 html 파일 작성

일반적으로 github blog 내에는 _include 폴더가 존재합니다. 폴더 내에 수식 문법이 포스트에 적용될 수 있게끔 하기 위한 스크립트를 작성합니다. 'math.html' 이라는 이름의 파일을 작성한다고 가정 하겠습니다. 

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

```inlineMath``` 와 ```displayMath``` 항목에서 각각의 수식 문법 기호를 설정할 수 있습니다. 위 예시의 ```displayMath``` 와 같이 리스트 내에 여러 기호를 설정할 수 있습니다. 위 예시에 따르면 수식을  ```$$``` 로 감싸거나, ```\\[``` ``` \\]``` 사이에 입력하면 display style로 작성할 수 있게 됩니다.

*```\\[``` ``` \\]``` 말고  ```\[``` ``` \]``` 로 문법을 설정하여 포스트에 적용하면, [ ] 괄호를 사용한 일반 텍스트까지 수식으로 처리되는 경우가 있었습니다.



### Inline과 Display style

수식 입력 방식에는 inline style과 display style이 있습니다.

* Inline style: 줄 바꿈 없이, 문장 내에서 수식을 표기하는 방법

* Display style: 수식을 블록으로 생성해 표기하는 방법

  ```
  $2$ plus $3$ is $5$: $$2+3=5$$
  ```

  $2$ plus $3$ is $5$: \\[2+3=5\\]

  

## 3. 2에서 작성한 html 스크립트를 포스트에 적용

위에서 작성한 스크립트를 실제 포스팅 시 적용하기 위해 layout에 관련한 html 파일을 수정합니다. _layout 폴더에 있는 html 파일 중 적합한 파일을 찾아 포스트의 내용 부분에 'math.html'의 내용을 가져와 적용합니다. 저는 'default.html' 파일 중 content가 입력되는 부분을 찾아 수정했습니다. 아래 예시와 같습니다.

``````html
<html>
  <body>
  	...
    <div class="content">
      {{ content }}
      {% include math.html %}
    </div>
  </body>
<html>
``````

```"content"``` 블록 내 ```{{ content }}``` 의 위치에 작성한 포스트의 본문이 보여집니다. ```{% include file.html %}``` 은 'file.html'의 내용을 가져온다는 뜻입니다. 따라서 해당 블록 내에 'math.html'에서 작성한 수식 문법 사항을 적용하겠다는 의미의 코드가 됩니다. 

위 코드를 아래와 같이 수정하면 수식 문법 적용 여부를 포스팅 시 설정해 줄 수 있는데요,

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

```page.use_math``` 가 ```true``` 이면 'math.html' 내용을 적용한다는 의미의 코드입니다. 여기서 ```page``` 는 각 포스트의 레이아웃에 관해 작성된 'page.html'을 의미합니다. ```page.use_math``` 을 설정하기 위해서는 매 포스트 작성 시 Front Matter에 ```use_math: true``` 를 추가해주면 됩니다.

```
---
title: ABCD
layout: post
...
use_math: true
---
```

수식이 필요 없거나, 수식을 적용하기 싫은 포스트에는 ```use_math``` 를 추가하지 않거나 ```false``` 로 설정하면 됩니다. 





---

### reference 

https://junia3.github.io/blog/markdown

https://an-seunghwan.github.io/github.io/mathjax-error/





