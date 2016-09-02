## 简易环境搭建

>JDK

一般选用1.8版本。

>环境变量

jdk安装完成后，需要统一配置一下环境变量。需要创建两个变量：<br />
1.JAVA_HOME : D:/Java/jdk1.8.0 <br />
2.PATH : 末尾追加%JAVA_HOME%\bin <br />
3.CLASSPATH : 用于编译时java类的路径，.是指当前目录。<br />
.;%JAVA_HOME%\lib\tools.jar <br />

以上配置完毕后，通过cmd运行java -version 和javac两个命令，如果正确输出，继续进行下一步。
<br />

>andorid-studio

摒弃eclipse繁琐的安装流程，直接用这个集成包，环境变量自动配置，虚拟机也自动安装好，还有自动提示，开发界面
都是一等一的，强烈推荐之。

