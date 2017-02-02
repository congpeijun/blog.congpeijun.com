---
title: 文本框Input中字母大写小写转换的代码
date: 2012-02-01 10:36:10
categories:
tags: [HTML,CSS]
---

首先，要实现的是在文本框输入大写字母自动转换为小写，效果：</li>

大写字母自动转换为小写：

```html
<input name="veryhuo" type="text" onkeyup="this.value=this.value.toLowerCase()" />
```

通过上面的实现，我们可以举一反三了，把toLowerCase()修改为toUpperCase()就可以转换为大写，再来看看效果：

小写字母自动转换为大写：

```html
<input name="veryhuo" type="text" onkeyup="this.value=this.value.toUpperCase()"/>
```

使用CSS实现的方法，这个方法可以实现表面上的小写转大写，但是在提交表单和复制粘贴时是不起作用的.
CSS实现小写字母自动转换为大写：

```html
<input name="veryhuo" type="text" style="text-transform:uppercase;" />
```
