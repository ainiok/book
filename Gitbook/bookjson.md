# 配置
* [title](#title)
* [author](#author)
* [description](#description)
* [language](#language)
* [links](#links)
* [styles](#styles)
* [plugins](#plugins)
* [pluginsConfig](#pluginsConfig)

# 插件
* [search-plus](#search Plus)
* [versions](#versions)
* [Terminal](#Terminal)
* [Simple-page-toc](#Simple-page-toc)
* [donate](#donate)
* [3-ba](#3-ba)
* [baidu](#baidu)
* [ga](#ga)
* [include-codeblock](#include-codeblock)
* [advanced-emoji](#advanced-emoji)


#配置：

#title

#language
Gitbook可选的语言如下：

`en, ar, bn, cs, de, en, es, fa, fi, fr, he, it, ja, ko, no, pl, pt, ro, ru, sv, uk, vi, zh-hans, zh-tw`

#links
在左侧导航栏添加链接信息

```
"links": {
    "sidebar": {
      "Home": "http://blog.ainiok.com"
    }
  }
```

# styles
自定义页面样式，默认情况下各generatord对应的css文件

```
"styles": {
    "website": "styles/website.css",
    "ebook": "styles/ebook.css",
    "pdf": "styles/pdf.css",
    "mobi": "styles/mobi.css",
    "epub": "styles/epub.css"
}
```

#plugins
配置使用的插件

```
"plugins":[
   "search"
]
```

添加型的插件之后需要运行 `gitbook install ` 安装
Gitbook 默认带有5个插件
- height
- search
- sharing
- font-settings
- livereload

如果要去除自带的插件，可以在插件的名称前面加 ` - `

```
"plugins": [
    "-search"
]
```
#pluginsConfig
配置插件的属性
```
"pluginsConfig":{
   "github":{
       "url":"https://github.com/ainiok/book"
   },
}
```

# 插件：
记录一些实用的插件

#search Plus

#versions
添加版本选择的下拉菜单，针对文档有多个版本的情况
[插件地址](https://plugins.gitbook.com/plugin/versions-select)

```
{
    "plugins": [ "versions-select" ],
    "pluginsConfig": {
        "versions": {
            "options": [
                {
                    "value": "http://gitbook.zhangjikai.com",
                    "text": "v3.2.2"
                },
                {
                    "value": "http://gitbook.zhangjikai.com/v2/",
                    "text": "v2.6.4"
                }
            ]
        }
    }
}
```

#Terminal
模拟终端显示，主要用于显示命令以及多行输出
[插件地址](https://plugins.gitbook.com/plugin/terminal)

#Simple-page-toc
自动生成本页的目录结构。另外 GitBook 在处理重复的标题时有些问题，所以尽量不适用重复的标题
[插件地址](https://plugins.gitbook.com/plugin/simple-page-toc)

#donate
打赏插件
[插件地址](https://plugins.gitbook.com/plugin/donate)

```
{
    "plugins": ["donate"],
    "pluginsConfig": {
        "donate": {
          "wechat": "例：/images/qr.png",
          "alipay": "http://blog.willin.wang/static/images/qr.png",
          "title": "默认空",
          "button": "默认值：Donate",
          "alipayText": "默认值：支付宝捐赠",
          "wechatText": "默认值：微信捐赠"
        }
    }
}
```

#3-ba
分析
[插件地址](https://plugins.gitbook.com/plugin/3-ba)

```
{
    "plugins": ["3-ba"],
    "pluginsConfig": {
        "3-ba": {
            "token": "xxxxxxxx"
        }
    }
}
```
#baidu
百度统计
[插件地址](https://plugins.gitbook.com/plugin/baidu)

#ga
Google统计
[插件地址](https://plugins.gitbook.com/plugin/ga)

#include-codeblock
使用代码块的格式显示所包含文件的内容. 该文件必须存在。插件提供了一些配置，可以区插件官网查看。如果同时使用 ace 和本插件，本插件要在 ace 插件前面加载。
[插件地址](https://plugins.gitbook.com/plugin/include-codeblock)


#advanced-emoji
emoji表情
[查看支持的表情](https://www.webpagefx.com/tools/emoji-cheat-sheet/)


[](http://gitbook.zhangjikai.com/plugins.html)