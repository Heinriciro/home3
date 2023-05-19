---
title: "[Android逆向] 某麦网APK抢票接口加密参数分析及抢票工具开发"
date: 2023-05-17T10:45:07Z

description: 
tags: [
  "draft"
]
issueId: 21
---

# 0x00 概述

针对大麦网部分演唱会门票仅能在app渠道抢票的问题，本文研究了APK的抢票接口并编写了抢票工具。本文介绍的顺序为环境搭建、抓包、trace分析、接口参数获取、rpc调用实现，以及最终的功能实现。通过阅读本文，你将学到反抓包技术破解、Frida hook、jadx apk逆向技术，并能对淘系APP的运行逻辑有所了解。本文仅用于学习交流，严禁用于非法用途。

先放成功截图：

![image](https://github.com/m2kar/m2kar.github.io/assets/16930652/b7c8e2be-cf53-432f-9a33-9960a66f715e)

# 0x01 缘起

疫情结束的2023年5月，大家对出去玩都有点疯狂，歌手们也扎堆开演唱会。但演唱会多，票一点也不好抢，抢五月天的门票难度不亚于买五一的高铁票。所以想尝试找一些脚本来辅助抢票，之前经常用selenium和request做一些小爬虫来搞定自动化的工作，所以在 [MakiNaruto/Automatic_ticket_purchase](https://github.com/MakiNaruto/Automatic_ticket_purchase)  的基础上改了改，实现抢票功能。但是大麦网实在太**狡猾**了，改完爬虫才发现几乎所有的热门演唱会只允许在app购买，所以就需要利用APP实现接口自动化。

本着能省事则省事的原则，笔者在文章 [[Android] 基于Airtest实现大麦网app自动抢票程序](https://github.com/m2kar/m2kar.github.io/issues/20) 中用自动化测试技术实现了抢票程序，但是速度太慢，几乎不能用。果然捷径往往不好走，因此继续尝试分析大麦网apk的api接口。

# 0x02 环境

- 大麦网 apk 版本8.5.4 (2023-04-26)
- bluestacks 5 
- adb
- wireshark
- frida
- jadx
- 

# 0x02 frida环境搭建


# 0x04 抓包


# 0x05 trace分析


# 0x06 hook得到接口参数


# 0x07 通过rpc调用


# 0x08 踩坑经历花絮


# 0x09 总结


<hr/>

- 欢迎[评论](https://github.com/m2kar/m2kar.github.io/issues/21)以及发邮件和作者交流心得。
- **版权声明**：本文为 m2kar 的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
- **作者**: m2kar
- **打赏链接**: [欢迎打赏m2kar,您的打赏是我创作的重要源泉](http://m2kar-cn.mikecrm.com/wy97haW)
- **邮箱**: `m2kar.cn<at>gmail.com`
- **主页**: [m2kar.cn](https://m2kar.cn)
- **Github**: [github.com/m2kar](https://github.com/m2kar)
- **CSDN**: [M2kar的专栏](https://m2kar.blog.csdn.net)

<hr/>

**欢迎在ISSUE参与本博客讨论**: [m2kar/m2kar.github.io#21](https://github.com/m2kar/m2kar.github.io/issues/21)