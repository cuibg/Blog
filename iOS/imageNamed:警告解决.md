
## 问题描述

控制台打印 log :

```
CUICatalog: Invalid asset name supplied: (null)
```

## 解决方案

问题定位方法如下：

Add symbolic breakpoint for [UIImage imageNamed:]

![](http://i.stack.imgur.com/ATz38.png)

Add $arg3 == nil condition (on Simulator) or $r0 == nil condition on iPhone device
Run your application and see where is the problem persist


## 参考文档

* [Error: CUICatalog: Invalid asset name supplied: (null), or invalid scale factor : 2.000000](http://stackoverflow.com/a/30071894)

