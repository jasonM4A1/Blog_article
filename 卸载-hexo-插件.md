---
title: 卸载 hexo 插件
date: 2020-10-17 01:01:25
tags: 
	- hexo
	- 插件
categories:
	- 博客
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017141234.jpg
---

曾经安装了某些hexo插件，某一天觉得是个累赘，那么如何卸载呢？

### 查看插件

```
npm list
├─┬ hexo-generator-tag@0.2.0
│ ├── hexo-pagination@0.0.2 deduped
│ └── object-assign@4.1.1 deduped
├─┬ hexo-qiniu-sync@1.4.7
│ ├─┬ chokidar@1.7.0
│ │ ├─┬ anymatch@1.3.2
│ │ │ ├── micromatch@2.3.11 deduped
│ │ │ └── normalize-path@2.1.1 deduped
│ │ ├── async-each@1.0.1
```

　　我要删除的是[_hexo-qiniu-sync@1.4](mailto:_hexo-qiniu-sync@1.4).7_，我不仅用不到它，还觉得它浪费hexo启动时间：

```
zhoupqcom:blog zhoupq$ blognew "卸载 hexo 插件"

INFO  -----------------------------------------------------------
INFO  qiniu state: online
INFO  qiniu sync:  true
INFO  qiniu local dir:  static
INFO  qiniu url:   http://xxxxxxxxxxx.clouddn.com/static
INFO  -----------------------------------------------------------
INFO  Created: ~/zhoupq.com/blog/source/_posts/卸载 hexo 插件.md
```

### npm 卸载

```
npm uninstall hexo-qiniu-sync@1.4.7
```

### 删除相关配置和文件

- 配置文件中有PLugins模块，删除对应的插件设置；
- 删除 *node_modules/* 目录下对应的插件文件

```
zhoupqcom:blog zhoupq$ hexo clean
ERROR Plugin load failed: hexo-qiniu-sync
TypeError: Cannot read property 'secret_file' of undefined
    at Object.<anonymous> (/Users/zhoupq/zhoupq.com/blog/node_modules/hexo-qiniu-sync/config.js:8:22)
    at Module._compile (module.js:635:30)
zhoupqcom:node_modules zhoupq$ rm -rf ./hexo-qiniu-sync
zhoupqcom:blog zhoupq$ hexo server
INFO  Start processing
[Browsersync] Access URLs:
 ----------------------------------
          UI: http://localhost:3001
 ----------------------------------
 UI External: http://localhost:3001
 ----------------------------------
INFO  ---- START COPYING TAG CLOUD FILES ----
INFO  ---- END COPYING TAG CLOUD FILES ----
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```

> <a href="https://www.dazhuanlan.com/2019/10/12/5da110cdd9a7b/?__cf_chl_jschl_tk__=ecbbdb310991188568a6d52a052d0a95ece74b5b-1602867587-0-AWTdZCJ64KcOykRx_GthhAl0nIr6noDp2_3UcVJAiFB989_Pstm1S4ZRCPWQSsIe72jyKHqdv6KZtAHytVHF1hyNK7HAcc3fbKUw2IapBUN0Dki2JMZA21aP_7RWRoDcXmduCDjR904sT-OCj8pdElFNEBb1g3udb1x2RUVz_zjg9PxIpg2u8v_wmQNdCR9uA2l4KnoQGTfd88qCqJx5-zKQCJYF3NaeHbmwSX9_AC-AMXfRkFcfpuUFOHmV5aNw8xB55-skeyhZuJhnjBgrCXdZxd7pG0-Rv0LUDbuzR8hT-ZNtA18bejF6KlVucJ2Rog">转载的原文链接</a>

