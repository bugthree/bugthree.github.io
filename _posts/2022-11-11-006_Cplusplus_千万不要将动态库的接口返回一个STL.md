---
layout: post
title: "千万不要将动态库的接口返回一个STL"
date: 2022-11-11
description: "动态库的接口返回一个STL"
tag: Cplusplus
author: bugthree
---

## 0. 千万不要将动态库的接口返回一个STL

![错误信息](/images/Cplusplus/1.jpg)

[参考链接](https://blog.csdn.net/china_jeffery/article/details/79656307)
[参考链接](https://www.cnblogs.com/jiayouwyhit/p/3492437.html)
[参考链接](https://blog.csdn.net/sdnumqy/article/details/103062777)
[参考链接](https://blog.csdn.net/10km/article/details/80522287#:~:text=std%3A%3Astring%20%E6%98%AFSTL%E4%B8%AD%E5%AE%9A%E4%B9%89%E7%9A%84%E6%A8%A1%E6%9D%BF%E7%B1%BB%EF%BC%8C%E6%89%80%E4%BB%A5%E7%BC%96%E8%AF%91%E5%99%A8%E5%9C%A8%E7%BC%96%E8%AF%91%E5%8A%A8%E6%80%81%E5%BA%93%E6%97%B6%E4%BC%9A%E5%B0%86%20std%3A%3Astring%20%E5%AE%9E%E4%BE%8B%E5%8C%96%2C%E5%9C%A8%E7%BC%96%E8%AF%91exe%E6%97%B6%E4%B9%9F%E4%BC%9A%E5%B0%86%E5%85%B6%E5%AE%9E%E4%BE%8B%E5%8C%96%EF%BC%8C%E4%B9%9F%E5%B0%B1%E6%98%AF%E8%AF%B4%E6%9C%89%E4%B8%A4%E5%A5%97,std%3A%3Astring%20%E5%AE%9E%E4%BE%8B%E4%BB%A3%E7%A0%81%E5%88%86%E5%88%AB%E5%9C%A8exe%E5%92%8Cdll%E4%B8%AD.%20%E5%9B%A0%E4%B8%BA%E6%88%91%E7%9A%84dll%E6%98%AF%2FMT%E7%BC%96%E8%AF%91%E7%9A%84%E6%89%80%E4%BB%A5%E8%BF%9E%E6%8E%A5%E7%9A%84%E6%98%AFstatic%20crt%EF%BC%8C%E6%89%80%E4%BB%A5%E5%8A%A8%E6%80%81%E5%BA%93%E6%9C%89%E8%87%AA%E5%B7%B1%E7%8B%AC%E7%AB%8B%E7%9A%84heap%EF%BC%8C%E5%8F%82%E8%A7%81%E5%8F%82%E8%80%83%E8%B5%84%E6%96%993.)

简单来说，对于运行库为`MTD`时，千万不能将一个动态库的接口，返回一个标准库内容。

更好的做法时，不要将动态库的接口返回一个STL