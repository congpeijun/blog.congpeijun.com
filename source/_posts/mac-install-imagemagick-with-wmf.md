---
title: mac 编译安装 imagemagick 支持wmf
date: 2015-04-19 18:47:00
categories: OSX
tags: [PHP,OSX]
---

由于需要将wmf转换为png或是其他格式需要用到 php imagick，
但是通过 pecl 安装的 imageick 扩展没有对wmf的支持，
查找原因为 brew 安装的 imagemagick 不支持 wmf，应该多方查找解决方法无果。只要重新自己编译了（可能我没找到方法）

## 以尝试过得方法列表

```bash
brew install imagemagick --with-libwml
brew install imagemagick --with-libwml --with-wmf=yes
```

## 安装libwmf

```bash
brew install libxml
```

## 安装 imagemagick

### 官方文档

[http://www.imagemagick.org/script/advanced-unix-installation.php][1]

### 下载源码
```bash
wget http://www.imagemagick.org/download/ImageMagick-6.9.1-1.tar.gz
tar -zxvf ImageMagick-6.9.1-1.tar.gz
```

### 编译安装
因为有些依赖在我用brew 安装 imagemagick 的时候已经安装。

```bash
    ./configure '--disable-osx-universal-binary' \
    '--prefix=/usr/local/Cellar/imagemagick/6.9.1-1' \
    '--disable-dependency-tracking' \
    '--disable-silent-rules' \
    '--enable-shared' \
    '--disable-static' \
    '--with-modules' \
    '--disable-openmp' \
    '--without-gslib' \
    '--without-perl' \
    '--with-gs-font-dir=/usr/local/share/ghostscript/fonts' \
    '--without-pango' \
    '--without-openjp2' \
    '--without-x' \
    --with-freetype=yes' \
    '--with-wmf=yes' \
    'CC=clang' \
    'CXX=clang++'

    make && make install

    # 查看是否支持wmf
    convert -list configure | grep -i wmf
```

## 安装 php imagick 扩展

```
   sudo pecl install imagick
```


  [1]: http://www.imagemagick.org/script/advanced-unix-installation.php