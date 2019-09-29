+++ 
draft = false
date = 2019-01-05T17:14:50+08:00
title = "Free DDNS"
slug = "" 
tags = []
categories = []
+++

# DDNS

* DDNS（Dynamic Domain Name Server）是动态域名服务的缩写。  
* DDNS是将用户的动态IP地址映射到一个固定的域名解析服务上，用户每次连接网络的时候客户端程序就会通过信息传递把该主机的动态IP地址传送给位于服务商主机上的服务器程序，服务器程序负责提供DNS服务并实现动态域名解析。  
* 目前ISP大多为我们提供动态IP（拨号上网），而很多服务通过远程访问时需要一个固定的IP，但固定IP的费用很难让客户接受。所以DDNS为大家提出了一种全新的解决方案，它可以捕获用户每次变化的IP，然后将其与域名相对应，这样客户就可以通过域名来进行远程访问了。

## [Free DDNS](https://freemyip.com)  
> Free dynamic DNS. No login required. No ads, newsletters, links to click, expiration dates, etc.  

这个ddns服务很良心，没有广告，订阅，也无需登录，还免费，开箱即用。  
它的用法很简单，只需要添加一个定时任务就可以: 

`(crontab -l;echo "*/19 * * * * curl \"https://freemyip.com/update?token=YOUR_TOKEN&domain=YOUR_DOMAIN.freemyip.com\">/dev/null 2>&1")|crontab -`

## [Help](https://freemyip.com/help)