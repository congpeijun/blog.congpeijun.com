---
title: PHP中获取文件扩展名的N种方法
date: 2012-03-19 21:23:42
categories: [PHP]
tags: 
---

转自：<a href="http://www.oschina.net/code/snippet_127872_8813" title="http://www.oschina.net/code/snippet_127872_8813">http://www.oschina.net/code/snippet_127872_8813</a>
PHP中获取文件扩展名的N种方法 
从网上收罗的，基本上就以下这几种方式：



```php
// 第1种方法：

function get_extension($file)
{
    substr(strrchr($file, '.'), 1);
}

// 第2种方法：
function get_extension($file)
{
    return substr($file, strrpos($file, '.')+1);
}
// 第3种方法：
function get_extension($file)
{
    return end(explode('.', $file));
}
// 第4种方法：
function get_extension($file)
{
    $info = pathinfo($file);
    return $info['extension'];
}
// 第5种方法：
function get_extension($file)
{
    return pathinfo($file, PATHINFO_EXTENSION);
}
```
<!--more-->
以上几种方式粗看了一下，好像都行，特别是1、2种方法，在我不知道pathinfo有第二个参数之前也一直在用。但是仔细考虑一下，前四种方法都有各种各样的毛病。要想完全正确获取文件的扩展名，必须要能处理以下三种特殊情况。
没有文件扩展名
路径中包含了字符.，如/home/test.d/test.txt
路径中包含了字符.，但文件没有扩展名。如/home/test.d/test
很明显：1、2不能处理第三种情况，3不能正确处理第一三种情况。4可以正确处理，但是在不存在扩展名时，会发出一个警告。只有第5种方法才是最正确的方法。顺便看一下pathinfo方法。官网上介绍如下：


```php
$file_path = pathinfo('/www/htdocs/your_image.jpg');

echo "$file_path ['dirname']\n";
echo "$file_path ['basename']\n";
echo "$file_path ['extension']\n";
echo "$file_path ['filename']\n"; // only in PHP 5.2+

```
它会返回一个数组，包含最多四个元素，但是并不会一直有四个，比如在没有扩展名的情况下，就不会有extension元素存在，所以第4种方法才会发现警告。但是phpinfo还支持第二个参数。可以传递一个常量，指定返回某一部分的数据：
PATHINFO_DIRNAME - 目录
PATHINFO_BASENAME - 文件名（含扩展名）
PATHINFO_EXTENSION - 扩展名
PATHINFO_FILENAME - 文件名（不含扩展名，PHP>5.2）
这四个常量的值分别是1、2、4、8，刚开始我还以为可以通过或运算指定多个：

```php
pathinfo($file, PATHINFO_EXTENSION | PATHINFO_FILENAME);
```
后来发现这样不行，这只会返回几个进行或运算常量中最小的那个。也就是四个标志位中最小位为1的常量。