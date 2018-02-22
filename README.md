# manifest
html 新属性 离线存储 
兼容不太好 基本没用（用不了，么办法啊）
原文地址： `http://yanhaijing.com/html/2014/12/28/html5-manifest/`
# 什么是Cache Manifest
首先manifest是一个后缀名为minifest的文件，在文件中定义那些需要缓存的文件，支持manifest的浏览器，会将按照manifest文件的规则，像文件保存在本地，从而在没有网络链接的情况下，也能访问页面。

当我们第一次正确配置app cache后，当我们再次访问该应用时，浏览器会首先检查manifest文件是否有变动，如果有变动就会把相应的变得跟新下来，同时改变浏览器里面的app cache，如果没有变动，就会直接把app cache的资源返回，基本流程是这样的。

# Manifest的特点
离线浏览: 用户可以在离线状态下浏览网站内容。
更快的速度: 因为数据被存储在本地，所以速度会更快.
减轻服务器的负载: 浏览器只会下载在服务器上发生改变的资源。

# 如何使用
html新增了一个manifest属性，可以用来指定当前页面的manifest文件。

创建一个和html同名的manifest文件，比如页面为index.html，那么可以建一个index.manifest的文件，然后给index.html的html标签添加如下属性即可：

`<html lang="en" manifest="index.manifest">`
Manifest文件
接下来详细说说manifest的细节，一个典型的manifest文件代码结构像下面这样：

CACHE MANIFEST
#version 1.3

CACHE:
    test.css

NETWORK:
	*
manifest文件，基本格式为三段： CACHE， NETWORK，与 FALLBACK，其中NETWORK和FALLBACK为可选项。

而第一行CACHE MANIFEST为固定格式，必须写在前面。

以#号开头的是注释，一般会在第二行写个版本号，用来在缓存的文件更新时，更改manifest的作用，可以是版本号，时间戳或者md5码等等。

CACHE:（必须）
标识出哪些文件需要缓存，可以是相对路径也可以是绝对路径。

a.css
`http://yanhaijing.com/a.css`
NETWORK:（可选）

这一部分是要绕过缓存直接读取的文件，可以使用通配符＊。

下面的代码 “login.asp” 永远不会被缓存，且离线时是不可用的：

NETWORK:
login.asp
可以使用星号来指示所有其他资源/文件都需要因特网连接：

NETWORK:
*
FALLBACK:（可选）
指定了一个后备页面，当资源无法访问时，浏览器会使用该页面。该段落的每条记录都列出两个 URI—第一个表示资源，第二个表示后备页面。两个 URI 都必须使用相对路径并且与清单文件同源。可以使用通配符。

下面的例子中，如果无法建立因特网连接，则用 “404.html” 替代 /html5/ 目录中的所有文件。

FALLBACK:
/html5/ /404.html
下面的例子中，则用 “404.html” 替代所有文件。

FALLBACK:
*.html /404.html
# 如何更新缓存
如下三种方式，可以更新缓存：

更新manifest文件
通过javascript操作
清除浏览器缓存
给manifest添加或删除文件，都可更新缓存，如果我们更改了js，而没有新增或删除，前面例子中注释中的版本号，可以很好的用来更新manifest文件。

html5中引入了js操作离线缓存的方法，下面的js可以手动更新本地缓存。

window.applicationCache.update();
如果用户清除了浏览器缓存（手动或用其他一些工具）都会重新下载文件。

# 注意事项
浏览器对缓存数据的容量限制可能不太一样（某些浏览器设置的限制是每个站点 5MB）。
如果manifest文件，或者内部列举的某一个文件不能正常下载，整个更新过程都将失败，浏览器继续全部使用老的缓存。
引用manifest的html必须与manifest文件同源，在同一个域下。
FALLBACK中的资源必须和manifest文件同源。
当一个资源被缓存后，该浏览器直接请求这个绝对路径也会访问缓存中的资源。
站点中的其他页面即使没有设置manifest属性，请求的资源如果在缓存中也从缓存中访问。
当manifest文件发生改变时，资源请求本身也会触发更新。