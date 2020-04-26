---
title: 安装iTerm2及oh-my-zsh
date: 2018-09-07 11:48:48
categories: Mac
tags:
    - Mac
    - 终端
---



## {% cq %}前言 {% endcq %}

{% note info %}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;完成了黑苹果的折腾，用上了黑苹果，本以为折腾完了，可以安心享受 Mac 带来的乐趣了，结果还是自己想太多。我正在面临的是一条无止境的路，Mac 上面有各种诱惑你的东西，让你了解的越多，也就感觉到自己的知识越来越匮乏，自己所感兴趣的和想折腾的也久越来越多。。 好了，不多废话，马上开始！

{% endnote %}

<!-- more -->

---

## {% cq %}调教iTerm2{% endcq %}

* 参考文档：[WenBo丨星空灬](https://juejin.im/post/5a815edd5188251c85636034) 

1. 安装 [iTerm2](https://www.iterm2.com/downloads.html)

2. 安装powerline

   ```shell
   	// 没有安装pip先安装pip
   # sudo easy_install pip
   
   // 之后安装powerline（这里可能会报错，可以参考问题解决）
   # pip install powerline-status
   
   // 若报错则执行下列命令
   # pip install powerline-status --user -U
   ```

3. 安装oh-my-zsh

4. ```shell
   # curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
   
   ```



5. 安装字体库fonts

   ```shell
   // 克隆字体库到本地
   # git clone https://github.com/powerline/fonts.git
   
   // 安装字体
   # cd fonts
   # ./install.sh
   
   ```

   安装成功后输出：

   ```shell
   ➜  fonts git:(master) ./install.sh
   Copying fonts...
   Powerline fonts installed to /Users/WENBO/Library/Fonts
   
   ```

6. 导入配色

   * 下载solarized：`# git clone https://github.com/altercation/solarized`

   * 进入`solarized/iterm2-colors-solarized` 文件夹，双击`Solarized Dark.itermcolors`和`Solarized Light.itermcolors`进行安装导入

   * 安装后进入`iTerm状态栏 Profiles-> Open Profiles -> Edit Profiles -> Color->Color Presets -> 选择Solarized Dark主题` 

7. 使用主题agnoster

   * 下载主题

   * ```shell
     // 克隆主题到本地
     # git clone  https://github.com/fcamblor/oh-my-zsh-agnoster-fcamblor
     
     // 安装主题
     # cd oh-my-zsh-agnoster-fcamblor
     # ./install
     
     ```

   * 编辑`~/.zshrc`文件，将`ZSH_THEME`的值改为`agnoster`

8. 添加指令高亮效果：[zsh-syntax-highlighting](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fzsh-users%2Fzsh-syntax-highlighting)

    ```shell
    // 克隆项目到本地
    # git clone git://github.com/zsh-users/zsh-syntax-highlighting.git
    
    // 编辑.zshrc文件，在最后添加如下内容
    source /Users/ding/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
    plugins=(zsh-syntax-highlighting)
    
    ```

9. 解决oh-my-zsh对于`.bash_profile`自定义的配置失效问题

   * 修改 oh-my-zsh 的配置文件`.zshrc`

     ```shell
     # vim ~/.zshrc
     
     // 在最后添加配置
     source ~/.bash_profile
     
     ```

   * 终端执行：`# source ~/.zshrc`

10. 解决 oh-my-zsh 的socks5代理问题

    * 设置代理

      ```shell
      // 编辑配置文件
      # vim ~/.zshrc

      // 添加配置
      alias proxy='export all_proxy=socks5://127.0.0.1:1086'
      alias unproxy='unset all_proxy'

      // 另一种带提示的配置
      proxy () {
        export http_proxy="http://127.0.0.1:1086"
        export https_proxy="http://127.0.0.1:1086"
        echo "HTTP Proxy on"
      }
      unproxy () {
        unset http_proxy
        unset https_proxy
        echo "HTTP Proxy off"
      }

      ```

    * 保存退出并重新加载资源：`# sourse ~/.zshrc`

    * 验证ip

      ```shell
      // 二者皆可
      # curl ip.cn

      # curl cip.cc
      ```

11. iTerm2快捷键

|        说明        | 快捷键                                       |
| :----------------: | :------------------------------------------- |
|      新建标签      | command + t                                  |
|      关闭标签      | command + w                                  |
|      切换标签      | command + 数字/左右                          |
|      切换全屏      | command + enter                              |
|        查找        | command + f                                  |
|      垂直分屏      | command + d                                  |
|      水平分屏      | command + shift + d                          |
|      切换屏幕      | command + option + 方向键 /  command + [ / ] |
|    查看历史命令    | command + ;                                  |
|   查看剪切板历史   | command + shift + h                          |
|     清除当前行     | ctrl + u                                     |
|       到行首       | ctrl + a                                     |
|       到行尾       | ctrl + e                                     |
|      前进后退      | ctrl + f/b (相当于左右方向键)                |
|     上一条命令     | ctrl + p                                     |
|    搜索命令历史    | ctrl + r                                     |
| 删除当前光标的字符 | ctrl + d                                     |
| 删除光标之前的字符 | ctrl + h                                     |
| 删除光标之前的单词 | ctrl + w                                     |
|   删除到文本末尾   | ctrl + k                                     |
|   交换光标处文本   | ctrl + t                                     |
|       清屏1        | command + r                                  |
|       清屏2        | ctrl + l                                     |

