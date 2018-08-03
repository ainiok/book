# 配置
* [title](#title)
* [author](#author)
* [description](#description)
* [language](#language)
* [links](#links)
* [styles](#styles)
* [title](#title)
* [plugins](#plugins)
* [pluginsConfig](#pluginsConfig)

# 插件
* [search-plus](#search Plus)












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