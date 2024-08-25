# 在Windows上安装v2ray服务端
意外的非常简单，，，，，甚至从开查开搞到完事只用了不到俩小时（也可能是以前粗读过官方文档）  
*本文纯新手向，所以比较啰嗦*  

## 下载官方 v2ray-core Windows 包  
以我现在写文章的最新版为例，[v2fly/v2ray-core Official repo v5.16.1](https://github.com/v2fly/v2ray-core/releases/tag/v5.16.1)，
在release里找到`v2ray-windows-64.zip`（release总览记得点展开）  
然后发到你的服务器上，远程桌面的话可以直接ctrlc+ctrlv，如果你是海外小鸡的话直接开个浏览器下也行  

## 下载用于生成配置文件的v2rayN客户端（偷懒式生成配置文件）
随便找一个客户端，2dust官方的或者第三方改的都行，这边只需要生成配置文件，用于连接服务器还是建议用官方的  
但是有一点很重要的，就是你找的这个客户端得有`导出所选服务器为服务端配置`，新版本的官方v2rayN没这个选项了  
（找官方老版本也行，就是我不知道要多老，正好看见一个第三方的就直接拿来用了）  

比如我这边用了一个第三方改版的老版本v2rayN [>repo release<](https://github.com/crazypeace/v2rayN-3.29-VLESS/releases/tag/v3.29.0.8)，下载core版  
解压后直接启动本体，双击托盘图标打开主窗口，  
右键新建一个Vmess服务器
- 地址：随便写，这部分在导出的时候会直接扔掉（如果需要用这个配置文件生成vmess链接的话就填服务器地址）
- 端口：服务器的监听端口，选个喜欢的（不伪装的话往大的开就完事了）
- 用户ID：生成（找别的UUID生成器生成也可）
- 额外ID：不建议改，默认的就行（你也可以看看[白话文教程](https://guide.v2fly.org/basics/vmess.html#%E6%9C%8D%E5%8A%A1%E5%99%A8)上的解释）
- 加密方式：奥托
- 别名：1145141919810（诶嘿~☆）

下半部分按需求改，如果你是国内小鸡想试试免流啥的话，只是用用的话就不用改了  
石角 月定 （狗头）  

然后在主界面上**右键你刚刚生成的服务器**，在`export server`里面点`导出所选服务器为服务端配置`，  
文件名随便写一个，待会需要改回`config.json`（不在当前目录下保存的话也可以直接一步到位，因为默认打开的目录下有同名文件）  
扔到服务器上  

## 服务端配置
把之前下好的压缩包解压一下，老版本的里面有三个exe（wv2ray,v2ray,v2ctl），新版本只有一个v2ray了  
（简单解释一下老版本，可以选择开wv2ray和v2ray，区别就是弹不弹窗，wv需要去任务管理器里关）  

把解压出来的`config.json`重命名一下，比如`config.json.bak`，这个方便后续微调配置文件时候拿来对照  
（改不了后缀的话先去资源管理器设置里面把文件扩展名勾上，在查看里面）  
把刚做好的`config.json`放到目录里，然后做点简单的微调（特调建议先把白话文文档看完再搞）  
- log部分：如果需要保留日志就在`access`和`error`行填个文件名（或者路径加文件名），比如`log.log`和`err.log`，
如果需要放下一层目录里就写`logs/log.log`，注意下一层目录需要提前新建好（this md file repo: github@bilibili33）
- inbounds部分：如果你开了日志的话建议把email那一行删了（上一行的逗号也要删），不然日志每行都会带上email

特调（非必须）：可以加上outbounds字段，一般来说服务端分两条就够了（freedom和blackhole），然后配合routing字段和geo文件简单处理一下一般而言需要封禁的地址，防止客户端分流失误  

完事之后可以用v2ray跑一下文件test  
在资源管理器中点击地址栏，然后输入cmd并回车，输入`v2ray.exe test`，如果报ok那就完事了，不然就照着提示检查一下  
（注意不是所有错误都能检查出来，如果服务端没有按预期运行的话建议好好检查一下）

## 启动服务端
首次启动可以先cmd敲一遍，打开cmd的方式同上，然后在cmd中输入`v2ray.exe run`，会停留在using default config，说明启动完成  

然后去防火墙放行端口  
如果你的vps提供商有安全组相关的页面的话，先把Windows的防火墙直接关了，然后在服务商后台放行刚才设置的传入端口就行了  
没有的话就打开`控制面板>防火墙>高级设置>入站规则>新建规则>端口>填刚才设置的端口>下一步>下一步>名称随便写（比如v2）>确定`
完成后就可以测试了  

---
## 附录
[新 V2Ray 白话文指南](https://guide.v2fly.org/)  
[V2ray.com routing](https://v2ray.com/chapter_02/03_routing.html)  
[Xray 超顶级白话文文档](https://xtls.github.io/document/)  
[233boy 二维码生成](https://233boy.github.io/tools/qr.html) 最大字符数约1600个  

## 其他  
其实查了一下v2fly的文档，应该是可以直接引用别的config文件的，但是试了试不成功，就按照最简单的方法来了
[v2fly.org 命令行参数](https://www.v2fly.org/guide/command.html)  
可以加tls，配合nginx做ws伪装，给服务器上个域名，拿nginx建个站挂个证书，给ws单独设置个路径，nginx做本地转发  
[谷中望月](https://blog.kukmoon.com/08413c56e3db/) 写完了之后又搜了搜看到这篇文档，参考价值挺大  
