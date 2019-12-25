# idea-zookeeper
idea的zookeeper插件该代码是在[zookeeper-intellij](https://github.com/linux-china/zookeeper-intellij)基础上做了一些微调，并解决了npe等问题

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

所以在使用在配置`zoo.cfg`开启，不然会报错

``` shell
# 可以指定开启ruok，全部开启 *
4lw.commands.whitelist=ruok
```

