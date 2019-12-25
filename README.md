# idea-zookeeper
[idea-zookeeper]( https://github.com/zhaoyunxing92/idea-zookeeper )插件是在[zookeeper-intellij](https://github.com/linux-china/zookeeper-intellij)基础上做了一些微调，并解决了npe等问题

### 使用前注意

由于该插件使用的是[curator]( http://curator.apache.org/ )操作zookeeper的代码里面`org.mvnsearch.intellij.plugin.zookeeper.ZkProjectComponent#ruok`

```java
public boolean ruok(String server) {
    try {
        String[] parts = server.split(":");
        Socket sock = new Socket(parts[0], Integer.valueOf(parts[1]));
        sock.getOutputStream().write("ruok".getBytes());
        ByteArrayOutputStream bos = new ByteArrayOutputStream();
        IOUtils.copyBytes(sock.getInputStream(), bos, 1000);
        if (!sock.isClosed()) {
            sock.close();
        }
        return "imok".equals(new String(bos.toByteArray()));
    } catch (Exception e) {
        return false;
    }
}
```

所以在zookeeper配置`conf/zoo.cfg`开启，不然会报错

``` shell
# 可以指定开启ruok，全部开启 *
4lw.commands.whitelist=ruok
```

但是有的时候zookeeper是你不能修改的，但是你又想用这个插件，你可以在源码里面注释上面的代码重新打包即可

再次感谢[zookeeper-intellij](https://github.com/linux-china/zookeeper-intellij)提供的源码

### 插件怎么编译

关于这个问题可以去看[IDEA插件开发]( https://www.jianshu.com/p/722841c6d0a9 )不赘述