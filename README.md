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
都是一等一的，强烈推荐之。<br />
安装好启动虚拟机时，可能会报一些错误，这个主要是主板设置的问题，比如我遇到了不支持虚拟化的问题，
这个主要开机进入bios，设置cpu选项，把支持虚拟化技术这一项开启就ok了，其他问题都雷同，自己按实际需求调整。

##启动
在windows下面启动，需要按照以下步骤：<br />
1）进入刚安装的Android Studio目录下的bin目录。找到idea.properties文件，用文本编辑器打开。<br />
2）在idea.properties文件末尾添加一行：disable.android.first.run=true，然后保存文件。<br />
3）关闭Android Studio后重新启动，便可进入界面。<br />

