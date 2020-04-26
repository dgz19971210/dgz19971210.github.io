---
title: Hexo Next主题调教
date: 2018-09-24 22:17:19
tags: 
    - Hexo博客
categories: Hexo博客
---

## {% cq %}前言{% endcq %}

<p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;突然想给自己的博客换主题，以前用的是 `Anisina` ，现在想换成 `Next` 了，毕竟以前的主题用的人不太多，想要自己打造要被折磨死。所以决定下狠心，换成 `Next` 了，直接一步到位。但就算是 Next 也让我折腾了两天，各种翻文档，搜博客，终于把自己想要的给折腾出来了。可算是花了些功夫，在这里记录一下我博客主题的配置方式，主要时插件的安装和设置，十分费心力，这里放上安装方式和参考博客。

</p>

<!-- more -->

## {% cq %}Hexo - Next 6.0配置{% endcq %}

 ### 根目录下的`_config.yml`

* 博客站点的配置文件



### Next主题下的 `_config.yml`

* Next 主题的配置文件



### Next 插件的安装

* Github官方文档：https://github.com/theme-next

* Algolia搜索插件
  * [官方文档教程](https://github.com/theme-next/hexo-theme-next/blob/master/docs/ALGOLIA-SEARCH.md) | [简书教程](https://www.jianshu.com/p/00f4bc425249) | [掘金教程](https://juejin.im/post/5af3f9d1518825673e35a6eb)

  * 在[Algolia官网](https://link.jianshu.com/?t=https%3A%2F%2Fwww.algolia.com%2F)注册并在indices中添加一个index，记住填写的名字

  * 在ApiKey中找到Search-Only API Key，再点击ALL API KEYS找到对应的Key并将personal_blog添加进去，在ACL中勾选如下，点击更新：

    ![](https://upload-images.jianshu.io/upload_images/5999132-33c0996fd2b3e81d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/602)

  * 修改站点配置文件，添加内容如下，在红线处填入你的Algolia中的Key，apikey即为Search-Only API Key：

    ![](https://upload-images.jianshu.io/upload_images/5999132-252dfadd66ed1de6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/630)

  * 修改 Next 主题配置文件

    ```
    algolia_search:
    	enable: true
    ```

  * 安装插件

    ```shell
    npm install hexo-algolia --save
    ```

  * 配置环境变量

  * ```shell
    export HEXO_ALGOLIA_INDEXING_KEY=你的API Key
    ```

  * 更新index

  * ```shell
    hexo algolia
    ```

  * 若出现下列画面则成功

    ![](https://upload-images.jianshu.io/upload_images/5999132-3870537e6625fc42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

  * 然后在站点配置中找到package.json， 把里面的hexo-algolia， 換成 "hexo-algolia": "^0.2.0"，如图：

    ![](https://upload-images.jianshu.io/upload_images/5999132-a343e8a0df3b0df5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/507)

* `encrypt` 加密插件

  * 参考教程：[Bill Yang's Blog](https://blog.bill.moe/encrypt/) | [官方教程]

  * 安装：`npm install hexo-encrypt --save`

  * 修改站点配置文件`_config.yml`，添加：

    ```shell
    encrypt:
    	password: 123456
    ```

    此处的密码为默认密码，博客可以设置独立密码，在未设置独立密码时使用默认密码

  * 在`package.json` 中修改插件的版本号为：`"hexo-encrypt": "^0.2.0"`

  * 在写博客时，在需要加密的文章的头部加入下列代码：

  * ```
    encrypt: true
    enc_pwd: 123456
    ```

    此处的独立密码可不加

* hexo-symbols-count-time 插件

  * [官方文档](https://github.com/theme-next/hexo-symbols-count-time)

  * Next主题自带了本插件，无需下载

  * 站点配置：

  * ```
    symbols_count_time:
      symbols: true # 文章字数
      time: true # 阅读时长
      total_symbols: true # 所有文章总字数
      total_time: true # 所有文章阅读中时长
    ```

  * Next主题配置

  * ```
    symbols_count_time:
      separated_meta: true  # 是否换行显示 字数统计 及 阅读时长
      item_text_post: true  # 文章 字数统计 阅读时长 使用图标 还是 文本表示
      item_text_total: false # 博客底部统计 字数统计 阅读时长 使用图标 还是 文本表示
      awl: 4
      wpm: 275
    ```

* Hexo-leancloud-counter-security

  * 教程：[官方文档](https://github.com/theme-next/hexo-leancloud-counter-security)

  * 作用：访问记数插件和评论系统

  * 注册 [leancloud](https://leancloud.cn) 账号，并在主题配置文件中填入 `appid` 和 `appkey`，并设置 `enable: true`

* 显示浏览文章进度

  * 主题设置文件查找`scrollpercent`修改为`true`：

    ```shell
    # Back to top in sidebar (only for Pisces | Gemini). 浏览进度和回到顶部（显示在侧边栏）
    b2t: true
    # Scroll percent label in b2t button. 显示浏览进度条
    scrollpercent: true
    ```

  * 只开启scrollpercent，回到顶部和阅读进度会显示在右下角，两者都开启则会显示在侧边栏下

* pace 进度条插件

  * 教程：[官方文档](https://github.com/theme-next/theme-next-pace)

  * 配置主题文件中的：`pace: true`

  * 选择合适的主题

    ![](http://upload-images.jianshu.io/upload_images/5308475-6d44a78e76dbf950.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Theme-next-fancybox3 图片浏览重插件

  * [官方文档](https://github.com/theme-next/theme-next-fancybox3)


* 生成网站地图 

  * 教程：[xiaojiejie123s](https://blog.csdn.net/xiaojiejie123s/article/details/77922625)

  * 安装 sitemap 插件

    ```shell
    npm install hexo-generator-sitemap --save     
    npm install hexo-generator-baidu-sitemap --save
    ```

  * 修改博客配置文件

    ```
    # URL
    ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
    url: http://ding.gq
    root: /
    # permalink: :year/:month/:day/:title/
    permalink: :posts/:category/:year-:month-:day-:title.html
    permalink_defaults:
    ```

  * 部署博客：`hexo d -g`

  * 部署完后可查看网站是否生成 siemap.xml 和 baidusitemap.xml 文件，可通过 `https://ding.gq/baidusitemap.xml` 查看

  * 向百度提交链接(百度站长平台)：主动推送 > 被动推送 > sitemap



* 设置首页不显示全文（只显示预览）

  * Next 主题配置文件搜索 `auto_excerpt` enable设置成 true即可

* 主页文章添加阴影

  * 打开 `\themes\next\source\css\_custom\custom.styl`，添加下列代码

    ```css
    // 主页文章添加阴影效果
     .post {
       margin-top: 60px;
       margin-bottom: 60px;
       padding: 25px;
       -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
       -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
      }
    ```



* 博客主页菜单设置

  * 找到主题配置中的 `menu` 开启所需选项

  * 关于菜单内容为空的情况，可用下列代码添加菜单页面并设置页面类型

    ```shell
    hexo new page categories
    ```

  * 打开 `source/categories/index.md` 页面类型为 categories

    ```
    title: categories
    date: 2018-09-24 00:55:04
    type: "categories"
    common: false
    ---
    ```

  * 其他菜单选项的页面都是这样设置，例如 about、tags 等新建页面和设置类型分别对应即可

* 博客页面不显示文章全文

  * 使用主题自带的 `auto_excerpt`
  * 使用 `<!-- more -->` 标记，博客会显示到标记隔断的位置



* 其他详细配置
  * [生生是我](https://blog.csdn.net/shengshengshiwo/article/details/79350413) | [DimpleMe](https://blog.csdn.net/qq_32454537/article/details/79482896) | [SEO优化](https://blog.csdn.net/sunshine940326/article/details/70936988) | [seo](https://blog.csdn.net/lzy98/article/details/81140704) | [麻瓜码农](http://jeffyang.top/Hexo/Hexo%E4%B8%BB%E9%A2%98Next%E7%BE%8E%E5%8C%96/) | [WenBo丨星空灬](https://www.jianshu.com/p/9f0e90cc32c2) | [官方帮助文档](http://theme-next.iissnan.com/faqs.html)
  * 超详细next主题配置解释：[reuixiy](https://reuixiy.github.io/technology/computer/computer-aided-art/2017/06/09/hexo-next-optimization.html)

 