# 在Docker与Mac IDEA中安装单机Hadoop环境

为了方便以后的使用，我们打算将单机版的Hadoop部署在Docker中。我们主要依照的是两个教程，一个是[Hadoop安装教程_单机/伪分布式配置_Hadoop2.6.0/Ubuntu14.04](http://www.powerxing.com/install-hadoop/)还有一个是[Hadoop: Setting up a Single Node Cluster.](http://hadoop.apache.org/docs/r2.8.0/hadoop-project-dist/hadoop-common/SingleCluster.html)。

首先安装远程控制软件，主要是为了往docker中传输文件，当然使用docker自身的命令也是可以做到的，所以这个远程控制软件的安装不是必须的。我们可以直接使用docker cp命令来进行文件的拷贝。所以这一步我们可以直接跳过。

## 安装Java环境

hadoop是依赖Java的。我们安装openjdk。我们同时安装了jdk和jre。

```shell
sudo apt-get install openjdk-7-jre openjdk-7-jdk
```

然后我们找一下他的根目录用以设置环境变量。这里我们使用了dpkg -L命令。

```shell
root@244ca27dce99:/# dpkg -L openjdk-7-jdk
/.
/usr
/usr/lib
/usr/lib/jvm
/usr/lib/jvm/java-7-openjdk-amd64
/usr/lib/jvm/java-7-openjdk-amd64/lib
/usr/lib/jvm/java-7-openjdk-amd64/lib/ir.idl
/usr/lib/jvm/java-7-openjdk-amd64/lib/tools.jar
/usr/lib/jvm/java-7-openjdk-amd64/lib/jconsole.jar
/usr/lib/jvm/java-7-openjdk-amd64/lib/ct.sym
/usr/lib/jvm/java-7-openjdk-amd64/lib/dt.jar
/usr/lib/jvm/java-7-openjdk-amd64/lib/amd64
/usr/lib/jvm/java-7-openjdk-amd64/lib/amd64/jli
/usr/lib/jvm/java-7-openjdk-amd64/lib/sa-jdi.jar
/usr/lib/jvm/java-7-openjdk-amd64/lib/orb.idl
/usr/lib/jvm/java-7-openjdk-amd64/man
/usr/lib/jvm/java-7-openjdk-amd64/man/man1
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/extcheck.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/jhat.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/native2ascii.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/jdb.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/jar.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/jrunscript.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/jinfo.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/wsimport.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/jstatd.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/javah.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/idlj.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/jarsigner.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/schemagen.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/jstack.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/javac.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/javap.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/serialver.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/wsgen.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/apt.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/jstat.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/jconsole.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/jmap.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/jsadebugd.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/rmic.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/jcmd.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/appletviewer.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/xjc.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/javadoc.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/man1/jps.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/extcheck.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/jhat.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/native2ascii.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/jdb.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/jar.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/jrunscript.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/jinfo.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/wsimport.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/jstatd.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/javah.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/idlj.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/jarsigner.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/schemagen.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/jstack.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/javac.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/javap.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/serialver.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/wsgen.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/apt.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/jstat.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/jconsole.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/jmap.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/jsadebugd.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/rmic.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/jcmd.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/appletviewer.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/xjc.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/javadoc.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/man/ja_JP.UTF-8/man1/jps.1.gz
/usr/lib/jvm/java-7-openjdk-amd64/include
/usr/lib/jvm/java-7-openjdk-amd64/include/jni.h
/usr/lib/jvm/java-7-openjdk-amd64/include/jvmti.h
/usr/lib/jvm/java-7-openjdk-amd64/include/classfile_constants.h
/usr/lib/jvm/java-7-openjdk-amd64/include/linux
/usr/lib/jvm/java-7-openjdk-amd64/include/linux/jawt_md.h
/usr/lib/jvm/java-7-openjdk-amd64/include/linux/jni_md.h
/usr/lib/jvm/java-7-openjdk-amd64/include/jawt.h
/usr/lib/jvm/java-7-openjdk-amd64/include/jvmticmlr.h
/usr/lib/jvm/java-7-openjdk-amd64/include/jdwpTransport.h
/usr/lib/jvm/java-7-openjdk-amd64/bin
/usr/lib/jvm/java-7-openjdk-amd64/bin/jps
/usr/lib/jvm/java-7-openjdk-amd64/bin/jstat
/usr/lib/jvm/java-7-openjdk-amd64/bin/xjc
/usr/lib/jvm/java-7-openjdk-amd64/bin/idlj
/usr/lib/jvm/java-7-openjdk-amd64/bin/jsadebugd
/usr/lib/jvm/java-7-openjdk-amd64/bin/jarsigner
/usr/lib/jvm/java-7-openjdk-amd64/bin/appletviewer
/usr/lib/jvm/java-7-openjdk-amd64/bin/jdb
/usr/lib/jvm/java-7-openjdk-amd64/bin/jmap
/usr/lib/jvm/java-7-openjdk-amd64/bin/javap
/usr/lib/jvm/java-7-openjdk-amd64/bin/jcmd
/usr/lib/jvm/java-7-openjdk-amd64/bin/jhat
/usr/lib/jvm/java-7-openjdk-amd64/bin/jrunscript
/usr/lib/jvm/java-7-openjdk-amd64/bin/javah
/usr/lib/jvm/java-7-openjdk-amd64/bin/jinfo
/usr/lib/jvm/java-7-openjdk-amd64/bin/apt
/usr/lib/jvm/java-7-openjdk-amd64/bin/serialver
/usr/lib/jvm/java-7-openjdk-amd64/bin/javac
/usr/lib/jvm/java-7-openjdk-amd64/bin/wsimport
/usr/lib/jvm/java-7-openjdk-amd64/bin/jconsole
/usr/lib/jvm/java-7-openjdk-amd64/bin/jar
/usr/lib/jvm/java-7-openjdk-amd64/bin/rmic
/usr/lib/jvm/java-7-openjdk-amd64/bin/schemagen
/usr/lib/jvm/java-7-openjdk-amd64/bin/wsgen
/usr/lib/jvm/java-7-openjdk-amd64/bin/native2ascii
/usr/lib/jvm/java-7-openjdk-amd64/bin/jstack
/usr/lib/jvm/java-7-openjdk-amd64/bin/jstatd
/usr/lib/jvm/java-7-openjdk-amd64/bin/javadoc
/usr/lib/jvm/java-7-openjdk-amd64/bin/extcheck
/usr/share
/usr/share/doc
/usr/share/lintian
/usr/share/lintian/overrides
/usr/share/lintian/overrides/openjdk-7-jdk
/usr/lib/jvm/java-7-openjdk-amd64/lib/amd64/jli/libjli.so
/usr/lib/jvm/java-7-openjdk-amd64/lib/jexec
/usr/lib/jvm/java-7-openjdk-amd64/include/jawt_md.h
/usr/lib/jvm/java-7-openjdk-amd64/include/jni_md.h
/usr/lib/jvm/java-7-openjdk-amd64/THIRD_PARTY_README
/usr/lib/jvm/java-7-openjdk-amd64/ASSEMBLY_EXCEPTION
/usr/lib/jvm/java-7-openjdk-amd64/src.zip
/usr/share/doc/openjdk-7-jdk
root@244ca27dce99:/# 
```

我们可以看出这个软件的根目录就是`/usr/lib/jvm/java-7-openjdk-amd64`。我们为这个java环境设计一个环境变量。我们打开配置文件`~/.bashrc`。然后使用export指令设置java的环境变量，然后进行一下检验。

```shell
root@244ca27dce99:/# java -version
java version "1.7.0_131"
OpenJDK Runtime Environment (IcedTea 2.6.9) (7u131-2.6.9-0ubuntu0.14.04.2)
OpenJDK 64-Bit Server VM (build 24.131-b00, mixed mode)
root@244ca27dce99:/# 
```

# 安装hadoop

我们现在下载当前最新版的hadoop，是2.8版本。

![](https://ws3.sinaimg.cn/large/006tKfTcly1fgpbt7keekj316g0g0jt1.jpg)

我们在本地下载并且解压，然后使用docker cp命令将这个东西拷贝到hadoop的容器中。

```shell
localhost:~ zhendu$ docker cp ~/Downloads/hadoop-2.8.0 hadoop:/
```

然后我们将这个目录放到usr/local之下，这个目录通常都会放着我们在本地安装的软件。然后我们改名并且把则个文件设为可执行的。

我们简单运行一个hadoop命令，看看行不行。

```shell
root@244ca27dce99:/usr/local/hadoop# ./bin/hadoop version
Hadoop 2.8.0
Subversion https://git-wip-us.apache.org/repos/asf/hadoop.git -r 91f2b7a13d1e97be65db92ddabc627cc29ac0009
Compiled by jdu on 2017-03-17T04:12Z
Compiled with protoc 2.5.0
From source with checksum 60125541c2b3e266cbf3becc5bda666
This command was run using /usr/local/hadoop/share/hadoop/common/hadoop-common-2.8.0.jar
root@244ca27dce99:/usr/local/hadoop
```

hadoop一开始默认就是设计为单机版，现在就可以使用hadoop的接口来开发hadoop程序了。实际上单机版并不是hadoop的唯一单机部署方法。

> Hadoop 可以在单节点上以伪分布式的方式运行，Hadoop 进程以分离的 Java 进程来运行，节点既作为 NameNode 也作为 DataNode，同时，读取的是 HDFS 中的文件。

当然我们现在就可以运行一些hadoop程序了。

## 运行hadoop程序

hadoop提供了很多案例程序，我们现在使用一个命令来运行一个样例程序：grep，先分别设定了input和output路径，然后就可以进行，样例程序的运行了。

```shell
./bin/hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar grep input output 'dfs[a-z.]+'
```

这个就是我们要运行的东西。

## 在IDEA中进行hadoop的开发

因为hadoop是完全基于java的，所以我们可以认为这个hadoop可以运行在mac物理机中，所以我门在IDEA中直接部署了hadoop的环境。

我们主要依照的教程是[使用IntelliJ IDEA 16.1写hadoop程序](http://blog.csdn.net/wk51920/article/details/51686337)

主要就是将hadoop的所有java包导入，然后在编译参数里面声明输入和输出文件。