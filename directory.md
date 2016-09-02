##目录结构

这个详细图文教程网上很多，这里就不赘述了。这里就是简单的点一下：

>manifests

这个目录下只有一个xml文件：AndroidManifest.xml。
这个文件是总配置文件，可以指定你的应用程序里面使用到的服务（如电话服务，短信服务，gps服务等）。
当新添加一个activity时，也需要在这个文件中进行配置，只有在这里配置好后，才能调用到activity。
AndroidManifest.xml将包含如下设置：application permissions、Activities、intent filters等。

>java

这个目录分了几个包，我们找到MainActivity这个java文件即可。在这里面编写代码。

>res

资源文件目录。比较重要的是layout这个目录，放一些静态页面。我们可以在MainActivity里面渲染调用这些静态页面。

ok，暂时就介绍这么多。