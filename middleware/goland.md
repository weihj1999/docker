
goland 破解

1. 官方下载GoLand 2019.3

https://www.jetbrains.com/go/download/download-thanks.html?platform=windows

2. 从这里获取补丁 jetbrains-agent 并将它放置到 GoLand安装目录的\bin目录下


3, 剩余步骤参考 https://juejin.im/post/5df9ebad51882512657bb253的激活教程，

在 “Help” -> “Edit Custom VM Options …”。步骤，我这里添加的例子是

```
-Xms128m
-Xmx2028m
-XX:ReservedCodeCacheSize=240m
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-XX:CICompilerCount=2
-Dsun.io.useCanonPrefixCache=false
-Djava.net.preferIPv4Stack=true
-Djdk.http.auth.tunneling.disabledSchemes=""
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
-Djdk.attach.allowAttachSelf=true
-Dkotlinx.coroutines.debug=off
-Djdk.module.illegalAccess.silent=true
-javaagent:C:\Program Files\JetBrains\GoLand 2019.3.3\bin\jetbrains-agent.jar
```

看最后一行即可。

在保存后，退出goland，然后重新启动

**切记重启GoLand软件**

4. 实际上已经完成了激活

![激活](imamges/goland-001.png)

