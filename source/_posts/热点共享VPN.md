---
title: 热点共享VPN
date: 2018-07-08 13:54:13
categories: VPN
tags: 
    - vpn
---


# 使用手册

### 简介

>* 本项目是利用电脑的或手机的WiFi热点，实现热点覆盖范围内ssr可用

---
<!-- more -->

### SSR客户端下载

1. Windows客户端下载地址：https://github.com/shadowsocksrr/shadowsocksr-csharp/releases

2. Mac客户端下载地址：https://github.com/flyzy2005/ss-ssr-clients/raw/master/ssr/SS-X-R.zip

3. Linux客户端下载地址：https://github.com/shadowsocks/shadowsocks-qt5/wiki/Installation

4. Android/安卓客户端下载地址：https://github.com/flyzy2005/ss-ssr-clients/raw/master/ssr/ShadowsocksR-3.4.0.8.apk



---
### 电脑WiFi共享SSR

    1、ShadowsocksR的设置

       右键 -> 设置选项 -> 勾选 “允许来自局域网的连接”

    2、获取电脑的IP地址

       运行 -> cmd -> ipconfig

    3、给手机等设备设置Proxy或HTTP代理

       *  IOS设置HTTP代理的方法：
              
          设置 -> Wi-Fi -> 感叹号 -> HTTP代理【手动】 -> 输入电脑ip及端口
          
       *  Android设置HTTP代理的方法：
          
          Wi-Fi -> 长按选择修改网络 -> 代理选择手动 -> 填写ip及端口

---
### 手机WiFi热点共享SSR

	1. Android系统
		* 在谷歌商店下载 ShadowsocksR 和 HTTP注射器
		* SSR中设置UDP转发
		* 开启WiFi热点
		* HTTP注射器右上角菜单打开中继连接
		* 查看日志是否中继成功
		
	2. IOS系统(我使用的是Shadowrocket，但一直未成功，仅供参考)
		* Shadowrocket -> 设置 -> 代理 -> 代理共享 -> 启用共享
		* 开启热点
		* 在Shadowrocket代理共享的界面，允许的ip地址中填入你接入设备的地址
		* 接入设备按照上述设置HTTP代理的方法进行设置


    
    

