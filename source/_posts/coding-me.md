---
title: github绑定自定义域名
---
1、在source文件夹中新建一个CNAME文件（无后缀名），然后用文本编辑器打开，在首行添加你的网站域名，如http://xxxx.com，注意前面没有http://，也没有www，然后使用hexo g && hexo d上传部署。
<!-- more -->
2、在域名解析提供商，下面以dnspod为例。
（1）先添加一个CNAME，主机记录写@，后面记录值写上你的http://xxxx.github.io
（2）再添加一个CNAME，主机记录写www，后面记录值也是http://xxxx.github.io
这样别人用www和不用www都能访问你的网站（其实www的方式，会先解析成http://xxxx.github.io，然后根据CNAME再变成http://xxx.com，即中间是经过一次转换的）。

上面，我们用的是CNAME别名记录，也有人使用A记录，后面的记录值是写github page里面的ip地址，但有时候IP地址会更改，导致最后解析不正确，所以还是推荐用CNAME别名记录要好些，不建议用IP。
3、等十分钟左右，刷新浏览器，用你自己域名访问下试试
