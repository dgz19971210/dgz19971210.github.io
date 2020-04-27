---
title: Ubuntu下使用Python的坑
date: 2018-07-16 16:15:30
categories: Linux
tags:
    - Ubuntu
    - Linux
---

## 前言
学习Python，需要用Linux，这里说一下我我遇到的各种坑

---
# Python版本更改
参考文章：[遥远的她](https://blog.csdn.net/fang_chuan/article/details/60958329) 和 [GitHub Issues](https://github.com/eagleon/eagleon.github.com/issues/2)

## 基于用户修改版本
1. 查看所拥有的Python版本
    ```shell
    ls /usr/bin/python*
    ```

2. 查看默认版本信息
    ```shell
    python --version
    ```

3. 修改默认版本
    ```shell
    alias python='/usr/bin/python3.6'
    ```

4. 查看是否修改成功
    重新登录或重新加载.bashrc文件
    ```shell
    . ~/.bashrc
    ```
    查看Python版本：参看上述第三步

    <!-- more -->
---
# VIM 下 Python 自动补全
### 参考文章：[niepangu](https://blog.csdn.net/niepangu/article/details/78976243)和[运维笔记](https://blog.linuxeye.cn/324.html)和[xpleaf](http://blog.51cto.com/xpleaf/1682449)
 1. 安装pydictin插件
    ```shell
        wget https://github.com/rkulla/pydiction/archive/master.zip
        unzip -q master
        mv pydiction-master pydiction
        mkdir -p ~/.vim/tools/pydiction
        cp -r pydiction/after ~/.vim
        cp pydiction/complete-dict ~/.vim/tools/pydiction
    ```

2. 创建~/.vimrc文件
    ```shell
        vim ~/.vimrc
    ```
    
3. 添加配置
    ```shell
        filetype plugin on
        let g:pydiction_location = '~/.vim/tools/pydiction/complete-dict'
        let g:pydiction_menu_height = 3
    ```

4. 编辑.py文件，测试是否成功

---
# 使用SSH连接Ubuntu
### 参考文章：[rabbittinbee](https://blog.csdn.net/rabbittinbee/article/details/52422790)

1. 网络连通
    Windows下查看ip cmd下:`ipconfig`
    Ubuntu下：`ifconfig`
    windows下ping Ubuntu的ip，查看是否ping通

2. Ubuntu开启SSH
    * 查看是否开启SSH
    ```shell
        ssh localhost
    ```
    如果出现下面提示表明还没有安装
    ```shell
        ssh: connect to hostlocalhost port 22: Connection refused 
    ```
    * 开启SSH
    ```shell
    sudo apt-getinstall –y openssh-server 
    ```

---
# Ubuntu安装fcitx使用小鹤双拼
### 参考文章：[点击这里](http://besky.me/2018/1/how-to-use-double-pinyin-with-fcitx-rime-on-ubuntu)
1.  安装fcitx
    ```shell
    # 安装 fcitx-rime， 是 fcitx 社区维护的
    $ sudo apt-get install fcitx-rime

    # 安装双拼方案
    $ sudo apt-get insatll librime-data-double-pinyin

    # 配置 fcitx 为默认输入法，然后重新部署或者重启
    $ im-config
    $ sudo reboot # (如果已经装过 fcitx 就不需要重启啦，系统托盘 fcitx 图标右键重新启动即可)

    # 添加输入法
    $ fcitx-config-gtk3 # (一般安装好就有了，最好确认一下)
    ```

2. 添加小鹤方案
    添加配置文件：`~/.config/fcitx/rime/default.custom.yaml`
    ```shell
    patch:
        schema_list:
            - schema: luna_pinyin          # 朙月拼音
            - schema: luna_pinyin_simp     # 朙月拼音 简化字模式
            - schema: luna_pinyin_tw       # 朙月拼音 臺灣正體模式
            - schema: terra_pinyin         # 地球拼音 dì qiú pīn yīn
            - schema: bopomofo             # 注音
            - schema: jyutping             # 粵拼
            - schema: cangjie5             # 倉頡五代
            - schema: cangjie5_express     # 倉頡 快打模式
            - schema: quick5               # 速成
            - schema: wubi86               # 五笔 86
            - schema: wubi_pinyin          # 五笔拼音混合輸入
            - schema: double_pinyin        # 自然碼雙拼
            - schema: double_pinyin_mspy   # 微軟雙拼
            - schema: double_pinyin_abc    # 智能 ABC 雙拼
            - schema: double_pinyin_flypy  # 小鶴雙拼
            - schema: wugniu               # 吳語上海話（新派）
            - schema: wugniu_lopha         # 吳語上海話（老派）
            - schema: sampheng             # 中古漢語三拼
            - schema: zyenpheng            # 中古漢語全拼
            - schema: ipa_xsampa           # X-SAMPA 國際音標
            - schema: emoji                # emoji 表情
    ```

---
# VMNet0、VMNet1和VMNet8的区别  

* vmnet0，实际上就是一个虚拟的网桥，这个网桥有很若干个端口，一个端口用于连接你的Host，一个端口用于连接你的虚拟机，他们的位置是对等的，谁也不是谁的网关。所以在Bridged模式下，你可以让虚拟机成为一台和你的Host相同地位的机器。

* vmnet1是host-only，也就是说，选择用vmnet1的话就相当于VMware给你提供了一个虚拟交换机，仅将虚拟机和真实系统连上了，虚拟机可以与真实系统相互共享文件，但是虚拟机无法访问外部互联；

* vmnet8是NAT，就是网络地址转换，相当于给你一个虚拟交换机，将虚拟机和真实系统连上去了，同时这台虚拟交换机又和外部互联网相连，这样虚拟机和真是系统可以相互共享，同时又都能访问外部互联网，而且虚拟机是借用真实系统的IP上网的，不会受到IP-MAC绑定的限制。

* 注意：很多人把VMnet8在宿主机比作虚拟机的路由器，这个是不对的，如果比作路由器的话，那么，虚拟机若想上网，必须把虚拟机上面的IP地址和宿主机VMnet8的ip地址相同才可以，如果那样就没有意义了，显然是不对的。所以，我们把宿主机VMnet8比作：“一个交换机，同时和外部互联网相连”。

---
# VS Code 编辑 Python 

## 乱码问题
参考文章：[zhaoshizi](https://www.cnblogs.com/zhaoshizi/p/9050768.html)

首选项中把`code-runner.executorMap`设置为`"python":"set PYTHONIOENCODING=utf-8 && python"`
完美解决

## 配置教程
参考文章：[浪晋](https://zhuanlan.zhihu.com/p/31417084)

---
# VIM下按 Ctrl+s 后假死的解决方法

## Ctrl+q 退出假死
* 使用vim时，如果你不小心按了 Ctrl + s后，你会发现不能输入任何东西了，像死掉了一般，其实vim并没有死掉，这时vim只是停止向终端输出而已，要想退出这种状态，只需按Ctrl + q 即可恢复正常。

---
# 持续更新中。。。