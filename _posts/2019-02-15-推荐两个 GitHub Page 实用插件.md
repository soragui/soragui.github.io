---
layout: post
title: 推荐两个 Github Page 实用插件
description: 两个很实用的插件，让你的 Github Page 博客更受欢迎。
---

> 如果你还不知道GitHub Page,可以参考[这个链接](https://pages.github.com/)。简单来说就是一个静态网站托管的地方，一般都是用来写博客或文档的地方。

## 1. [jekyll-seo-tag](https://github.com/jekyll/jekyll-seo-tag)

第一个就是 jekyll-seo-tag。这个是一个 SEO 插件。 SEO 是 Search Engine Optimization 的缩写。字面意思就是搜索引擎优化，也就是一些网站为了提高在搜索引擎中排名的方式，而进行的相关优化和设计。一般的就是在网页HTML文档里面添加一些元数据，也就是 metadata。比如网站的标题、描述、关键字以及 Twitter Facebook 等网站分享时的链接方式等。参考[这里](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/The_head_metadata_in_HTML)获取更多关于metadata的信息。

好了，下面就开始为你的GHP网站添加 seo 支持吧。首先在在 Gemfile 中添加插件：

```yaml
gem 'jekyll-seo-tag'
```

然后在 _config.yml 中添加如下：

```yml
plugins:
    - jekyll-seo-tag
```

好了，最后一步就是在你的html文件的 `</head>` 标签前面添加如下：

<!-- {% raw %} -->
  ```liquid
  {% seo %}
  ```
<!-- {% endraw %} -->

基本工作已经完成，下面就是在 _config.yml 文件里面配置一些参数了。可以参考[这里](https://github.com/jekyll/jekyll-seo-tag/blob/master/docs/usage.md)查看完整参数配置。

## 2. [disqus](https://disqus.com/)

**disqus** 问你的网站添加评论互动功能，现在很多网站博客上都有这个功能，下面就开始来手动为你的博客添加此功能吧。

首先如果你还没有 disqus 账号的话，去注册一个，然后进入[管理员](https://disqus.com/admin/)页面，添加一个网址，填写相关的参数后就可以添加了：

### 1. 在 _config.yml 文件里面添加：

```yml
disqus: YOUR_SHORTNAME
```
把 YOUR_SHORTNAME 参数替换成你自己刚刚配置的 Shortname.

### 2. 添加 disqus.html

在你的 _includes 目录里面新建 disqus.html，然后拷贝如下内容进去，第二部就算完成了，下面就是把这个添加到你的每一个 post 文件里面去。

```html
<section class="disqus">
    <div id="disqus_thread"></div>
    <script type="text/javascript">
        var disqus_shortname = '{{site.disqus}}'; 
        var disqus_developer = 0;
        (function() {
            var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
            dsq.src = window.location.protocol + '//' + disqus_shortname + '.disqus.com/embed.js';
            (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
        })();
    </script>
    <noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
    <a href="http://disqus.com" class="dsq-brlink">comments powered by <span class="logo-disqus">Disqus</span></a>
</section>
```

3. 找到你的 post.html 文件，然后在合适的地方添加如下：

```html
{% include disqus.html %}
```

大功告成，现在你可以重新生成看一下效果了。

今天分享的两个插件希望大家喜欢...
