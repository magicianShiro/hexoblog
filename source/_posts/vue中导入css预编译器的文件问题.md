---
title: vue中引用css的预编译器的路径问题
tags:
  - vue
  - css预编译器
banner: futaba.jpg
layout: "posts"
date: 2017-08-03 09:06:44
categories: 技术
---



> 起因只是因为老大突然给了一个新需求，然后我改了好多代码。不行。。不能再这样下去了，想了一想还是重构得了。自己写下的代码，跪着也得重构。

<!-- more -->
{% asset_img mmp.jpg 嘴上笑嘻嘻,心里mmp %}

正当我用`stylus`兴致勃勃的写好通用样式，定义好全局变量，写好工具，往`vue`里面怼的时候，艾玛，出问题了。。。

stylus的项目目录如下

```
│  main.styl
│  _extend.styl
│  _variables.styl
│      
└─_unit
        mixin.styl
```
其中 `main.styl` 用 引入了其他的三个文件，然后在 `vue` 的 `main.js` 中引入 `main.styl` 

打开页面一看， 没毛病。样式都有好好的执行着。

然后我在子组件中想要使用 mixin 中的东西，我用 `@import '@/xxxx'` 这样导入文件，怎么导入怎么错。

用`vue-cli`脚手架的应该都知道这个`@`是配置在`build=>webpack.base.conf.js=>resolve=>alias`里面的。

相当用这个键 去代替一个冗长的路径。不然你嵌套深了的话，写相对路径还不得累死。

但是在 `stylus` 的文件中 这样导入路径还是有问题，困扰了好久，翻文档，请教大神，最终解决。

# 解决办法：
  其实我翻 `stylus-loader` 的时候就发现了 `~` 这么个路径符，但是我只是片面的理解成了 它是直接匹配到 `node_modules` 中的东西。
  后来在社区提问，一大佬告诉我，你可以 `@import '~@/xxxx'` 我一试。。我去，成了。。

总得来说 就是可以用 `~` 加上 `alias` 中 配置的路径组合起来使用，比如我 `alias` 中配置的就是 `'stylus': resolve('src/assets/stylus')` 这样我使用的时候直接 `@import '~stylus/xxxx'` 方便快捷， 从此告别 `../`


