---
title: github-pages + hexo ＋ next主题的建博全记录
date: 2017-02-16 11:24:11
tags: hexo
---

话说开始写我这个站上第一篇博客了，好紧脏。  
搭建这个网站可以说是踩坑无数，来 让我们来一起慢慢填坑。

#建立博客
##建立github相应的仓库
建立这个博客的时候，关于git的坑踩了特别多。这个放到接下来一篇来写。  
首先我们在git上建立一个仓库，名字为yourname.github.io
![](http://i1.piimg.com/567571/081bbc5e8c3b39a4.png)
填好名字确认之后，我们的博客空间就已经建立了。
<!-- more -->
##hexo初步搭建
安装了node.js之后，在你相应的根目录下执行下面代码来安装hexo
> npm install -g hexo

git init啦之类的。hexo安装好之后建立hexo博客框架
> hexo init  

生成静态页面
> hexo g

启动本地服务器，在浏览器中输入`localhost:4000`可进入生成的静态界面
>hexo s

![](http://7xr185.com1.z0.glb.clouddn.com/hexo.png)
接下来打开博客目录下的_config.yml文件，建立与github库的关联

```
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/NicoNicoNicoNico/NicoNicoNicoNico.github.io.git
  branch: master
```

配置完成之后记得`hexo d`来部署hexo,若报错执行`npm install hexo-deployer-git --save`  
再正常的提交代码，然后你就可以去你的博客`yourname.github.io`观摩了。可能会有个十分钟的延迟。
page建立的成功与失败，github都会给你发邮件哒。  
 \\("▔□▔)/\\("▔□▔)/\\("▔□▔)/  
（当时各种错被git的邮件折磨到恐惧）

mac旁友，记得.gitnore处理掉.DS_Store这个烦人的家伙。

此阶段感谢大哥mrkou47与CNFeat的博  
[http://mrkou47.github.io/2016/04/01/setting-blog/](http://mrkou47.github.io/2016/04/01/setting-blog/)   
[http://blog.sina.com.cn/s/blog_617ccc0c0101h84p.html](http://blog.sina.com.cn/s/blog_617ccc0c0101h84p.html)

#主题next
##安装主题next
[next主题github地址](https://github.com/iissnan/hexo-theme-next) / 
[next主题使用文档](http://theme-next.iissnan.com/)  

这里我又犯坑了，刚开始我是直接`git clone`到仓库的themes目录下的，当我都配置完成之后push到git后，报错邮件又一封一封的来了。  

![](http://i1.piimg.com/567571/5856af9259038ba9.png)

我按照链接给的方法对submodule操作之后提交成功过，但之后又崩了。按照大哥的方法把theme进行git clone到别的位置，再复制过去也成功过，后来不知道咋又崩了…… 后来重新来过之后好像又是直接git clone到根目录的themes文件夹中，也没什么问题到现在。不明白具体什么情况啊…… 邮箱倒是攒了不少这个问题的邮件…… 请赐教啊各位

![](http://p1.bqimg.com/567571/d55860514e5f8fb9.png)

跑题了（羞）

将主题next放到themes下之后，在_config.yml更改`theme: next`,之后再`hexo g`,`hexo s`就可以看到主题已经配置成功了。  
具体的主题配置，文档已经给的很详细了，在这里就不赘述了。

## algolia搜索
next的文档里已经讲了一部分添加搜索的内容，但还是踩了一些坑。
首先要注意的是  

![](http://p1.bpimg.com/567571/3e5a5545c85ce02c.png)
接下来按照文档内容一步步去注册  

![](http://p1.bpimg.com/567571/42c22ab987404f91.png)

安装algolia
> npm install --save hexo-algolia

获取验证信息

![](http://i1.piimg.com/567571/1866348b65bb8351.png)

当配置完成，在站点根目录下执行`hexo algolia`来更新 Index。请注意观察命令的输出。

![](http://p1.bpimg.com/567571/991ebc28ed4c5af4.png)

更改主题配置文件，找到 Algolia Search 配置部分：

```
# Algolia Search
algolia_search:
  enable: false
  hits:
    per_page: 10
  labels:
    input_placeholder: Search for Posts
    hits_empty: "We didn't find any results for the search: ${query}"
    hits_stats: "${hits} results found in ${time} ms"
```

将 enable 改为 true 即可，根据需要你可以调整 labels 中的文本。

文档相关内容到此为止了，接下来坑开始来了。当我完成这些步骤之后在menu出现了搜索标（与我之前在menu添加的搜索重复），点卡搜索之后，能够检索到相关博文，但是点开都是404.

后来在这里找到了相关答案。[http://www.jianshu.com/p/fa2354d61e37](http://www.jianshu.com/p/fa2354d61e37)  

在themes\next\layout_partials中找到header.swig，找到以下代码并修改

```
<nav class="site-nav">
  <!-- 添加  theme.algolia_search.enable -->
  {% set hasSearch = theme.swiftype_key || theme.algolia_search.enable || theme.tinysou_Key || config.search %}
    
  {% if theme.menu %}
    <ul id="menu" class="menu">
      {% for name, path in theme.menu %}
        {% set itemName = name.toLowerCase() %}
        <li class="menu-item menu-item-{{ itemName }}">
          <a href="{{ url_for(path) }}" rel="section">
            {% if theme.menu_icons.enable %}
              <i class="menu-item-icon fa fa-fw fa-{{theme.menu_icons[itemName] | default('question-circle') | lower }}"></i> 

            {% endif %}
            {{ __('menu.' + itemName) }}
          </a>
        </li>
      {% endfor %}

  {% if hasSearch %}
    <li class="menu-item menu-item-search">
      {% if theme.swiftype_key %}
        <a href="javascript:;" class="st-search-show-outputs">
      {% elseif config.search %}
        <a href="javascript:;" class="popup-trigger">

<!-- 添加 开始 -->

      {% elseif theme.algolia %}
        <a href="javascript:;" class="popup-trigger">

<!-- 添加 结束 -->

      {% endif %}
        {% if theme.menu_icons.enable %}
          <i class="menu-item-icon fa fa-search fa-fw"></i> <br />
        {% endif %}
        {{ __('menu.search') }}
      </a>
    </li>
  {% endif %}
</ul>

  {% endif %}

  {% if hasSearch %}
    <div class="site-search">
      {% include 'search.swig' %}
    </div>
  {% endif %}
</nav>
```

还没完  
在_config.yml下的algolia配置下加path

```
#algolia
algolia:
  applicationID: ''
  apiKey: ''
  adminApiKey: ''
  indexName: ''
  chunkSize: 5000
  fields:
    - title
    - slug
    - path
    - content:strip
```

把 hexo的版本改成 `"hexo-algolia": "^0.2.0" `,在 node_modules/hexo-algolia/lib， 找到command.js，找到下面这句，在后面的参数里加path,如同下句。

```var storedPost = _.pick(data, ['title', 'date', 'slug', 'path', 'content', 'excerpt', 'objectID']);```

改完之后再`hexo algolia`  
我按照别人的方法，如此尝试了多次才得以成功。


每次提交之前都会  

> hexo g  
> hexo s  
> hexo d  

之后再提交代码

祝大家码站愉快～ 

![](http://p1.bpimg.com/567571/e397862b710fccd1.png)

