---
layout: post
title: Spark环境搭建
categories: Tech笔记
tags: [spark, 环境搭建]
---

* content
{:toc}


扩展：https://www.jianshu.com/p/aa6f3a366727


#### 一、涉及的软件版本：

- ubuntu16.04 LTS
- jdk-8u144-linux-x64.tar.gz
- hadoop-2.7.5
- spark-2.2.0-bin-hadoop2.7

#### 二、配置ssh免密码登录

1. 配置/etc/hosts, 在每台机器上做如下修改(改动项为含instance注释的那几行)

   ```
   brook@HelloWorld:~$ cat /etc/hosts
   127.0.0.1 localhost

   172.31.12.137 fang-ubuntu  #instance -> scrapy_1
   172.31.3.182 fei-ubuntu    #instance -> scrapy_2
   172.31.4.53 kun-ubuntu     #instance -> scrapy_3

   # The following lines are desirable for IPv6 capable hosts
   ::1 ip6-localhost ip6-loopback
   fe00::0 ip6-localnet
   ff00::0 ip6-mcastprefix
   ff02::1 ip6-allnodes
   ff02::2 ip6-allrouters
   ff02::3 ip6-allhosts
   ```

   配置完后ping一下各机器名称看是否生效

2. 如果没有安装ssh，需要安装Openssh server，命令为：

   ```
   brook@HelloWorld:~$ sudo apt-get install openssh-server
   ```

3. 生成私钥和公钥

   ```
   brook@HelloWorld:~$ ssh-keygen -t rsa  # 一路回车
   ```

4. 将公钥添加到authorized_keys

   ```
   brook@HelloWorld:~$ cd .ssh
   brook@HelloWorld:.ssh$ cat id_rsa.pub >> authorized_keys
   ```

5. 将其他机器的id_rsa.pub传过来(用scp语句), 同样用上面的语句写入authorized_keys中,汇总后的authorized_keys传到其他机器中

6. 测试免密码登录

   ```
   brook@HelloWorld:~$ ssh ubuntu@fei-ubuntu
   ```

#### 三、安装jdk

1. 解压jdk到`/opt`目录:

   ```
   brook@HelloWorld:~$ sudo tar -xzvf jdk-8u144-linux-x64.tar.gz -C /opt
   ```

2. 建立jdk软链接

   ```
   brook@HelloWorld:~$ cd /opt
   brook@HelloWorld:/opt$ sudo chown -R brook:brook jdk1.8.0_144  #如果有新建专门的hadoop用户,可以改为hadoop
   brook@HelloWorld:/opt$ sudo ln -s jdk1.8.0_144/ jdk 
   ```

3. 修改环境变量

   ```
   brook@HelloWorld:/opt$ cd ~
   brook@HelloWorld:~$ vim .bashrc
   ```
   在.bashrc末尾添加:
   ```
   ## JAVA
   export JAVA_HOME=/opt/jdk
   export JRE_HOME=${JAVA_HOME}/jre
   export CLASSPATH=.${JAVA_HOME}/lib:${JRE_HOME}/lib
   export PATH=${JAVA_HOME}/bin:$PATH
   ```
   保存后执行如下两个命令, 显示类似的结果则jdk安装成功

   ```
   brook@HelloWorld:~$ source .bashrc
   brook@HelloWorld:~$ java -version
   java version "1.8.0_144"
   Java(TM) SE Runtime Environment (build 1.8.0_144-b01)
   Java HotSpot(TM) 64-Bit Server VM (build 25.144-b01, mixed mode)
   brook@HelloWorld:~$ 
   ```


#### 四、Hadoop安装配置:

1. 从hadoop官网下载hadoop的二进制tar.gz包

2. 解压到 `/opt` 目录下建立软连接

   ```
   brook@HelloWorld:~$ sudo tar -xzvf hadoop-2.7.5.tar.gz -C /opt
   brook@HelloWorld:~$ cd /opt
   brook@HelloWorld:opt$ sudo chown -R root:root hadoop-2.7.5.tar.gz  #如果有新建hadoop用户,可以改为hadoop
   brook@HelloWorld:opt$ sudo ln -s hadoop-2.7.5.tar.gz hadoop
   ```

3. 修改环境变量

   ```
   brook@HelloWorld:~$ vim .bashrc
   ```

   添加一下内容

   ```
   ## hadoop
   export HADOOP_HOME=/opt/hadoop
   export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop
   export YARN_HOME=/opt/hadoop
   export YARN_CONF_DIR=${YARN_HOME}/etc/hadoop
   ```

4. 保存`.bashrc`文件,执行source命令使环境变量生效

   ```
   brook@HelloWorld:~$ source .bashrc
   ```

5. 进入 `hadoop/etc/hadoop` 目录

6. 编辑 hadoop-env.sh，设置JAVA_HOME。大约在第25行：

   ```
   export JAVA_HOME=${JAVA_HOME}
   ```
   将其改为你本地的jdk位置，例如我的jdk放在 `/opt` 下， 所以改成：

   ```
   export JAVA_HOME=/opt/jdk
   ```

7. 在yarn-env.sh中配置JAVA_HOME

   ```
   # some Java parameters
   export JAVA_HOME=/opt/jdk
   ```

8. 在slaves中配置slave节点的IP或者host

   ```
   fei-ubuntu
   kun-ubuntu
   ```

9. 先在~目录下创建如下几个文件夹（也可以指定其他位置）

   ```
   brook@HelloWorld:~$ mkdir tmp
   brook@HelloWorld:~$ mkdir hdfs
   brook@HelloWorld:~$ mkdir hdfs/data
   brook@HelloWorld:~$ mkdir hdfs/name
   ```

10. 配置 core-site.xml 文件

   ```
   <configuration>
   <property>
     <name>fs.default.name</name>
     <value>hdfs://fang-ubuntu:9000</value>
   </property>
   <property>
     <name>hadoop.tmp.dir</name>
     <value>file:/home/ubuntu/tmp</value>
   </property>
   </configuration>
   ```

11. 配置 `hdfs-site.xml`文件

   ```
   <configuration>
     <property>
      <name>dfs.replication</name>
      <value>3</value>
     </property>
     <property>
       <name>dfs.namenode.secondary.http-address</name>
       <value>fang-ubuntu:9001</value>
     </property>
     <property>
      <name>dfs.name.dir</name>
      <value>file:/home/ubuntu/hdfs/name</value>
     </property>
     <property>
      <name>dfs.data.dir</name>
      <value>file:/home/ubuntu/hdfs/data</value>
     </property>
   </configuration>
   ```

12. 修改 `mapred-site.xml`文件

   ```
   <configuration>
   <property>
     <name>mapreduce.framework.name</name>
     <value>yarn</value>
   </property>
   </configuration>
   ```

13. 修改yarn-site.xml

   ```
   <configuration>
     <property>

       <name>yarn.nodemanager.aux-services</name>
       <value>mapreduce_shuffle</value>
     </property>
     <property>
       <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
       <value>org.apache.hadoop.mapred.ShuffleHandler</value>
     </property>
     <property>
       <name>yarn.resourcemanager.address</name>
       <value>fang-ubuntu:8032</value>
     </property>
     <property>
       <name>yarn.resourcemanager.scheduler.address</name>
       <value>fang-ubuntu:8030</value>
       </property>
     <property>
       <name>yarn.resourcemanager.resource-tracker.address</name>
       <value>fang-ubuntu:8035</value>
     </property>
     <property>
       <name>yarn.resourcemanager.admin.address</name>
       <value>fang-ubuntu:8033</value>
     </property>
     <property>
       <name>yarn.resourcemanager.webapp.address</name>
       <value>fang-ubuntu:8088</value>
     </property>
   </configuration>
   ```

14. 将上述配置好的文件发送给所有的slaves节点

15. 接下来进行handoop的namenode格式化

   ```
   brook@HelloWorld:hadoop$ bin/hadoop namenode -format
   ```

16. 启动

   ```
   brook@HelloWorld:~$ start-dfs.sh
   brook@HelloWorld:~$ start-yarn.sh
   ```

17. 使用后java自带的jps命令查看所有的守护进程

   ```
   brook@HelloWorld:~$ jps
   24913 ResourceManager
   25042 NodeManager
   25143 Jps
   24762 SecondaryNameNode
   24573 DataNode
   24415 NameNode
   brook@HelloWorld:~$
   ```

18. 在每个slave上应该有以下几个进程

   ```
   brook@HelloWorld:~$ jps
   5958 NodeManager
   6169 Jps
   5596 DataNode
   ```

19. 打开浏览器，输入`http://localhost:50070/dfshealth.html`查看集群监控页面




#### 五、Spark安装

1. 官网下载spark安装包,我这边用的是spark-2.2.20-bin-hadoop2.7.tar.gz

2. 解压到/opt/目录下,建立软链接

   ```
   brook@HelloWorld:~$ tar -xzvf spark-2.2.20-bin-hadoop2.7.tar.gz -C /opt/
   brook@HelloWorld:~$ sudo ln -s /opt/spark-2.2.20-bin-hadoop2 spark
   ```

3. 修改配置文件

   ```
   brook@HelloWorld:~$ cd /opt/spark/conf
   brook@HelloWorld:conf$ cp spark-env.sh.template spark-env.sh
   brook@HelloWorld:conf$ vim spark-env.sh
   ```

   增加以下内容

   ```
   export SPARK_HOME=/opt/spark
   export JAVA_HOME=/opt/jdk
   export HADOOP_HOME=/opt/hadoop
   export YARN_HOME=/opt/hadoop
   export PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$SCALA_HOME/bin
   export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop
   export YARN_CONF_DIR=${YARN_HOME}/etc/hadoop
   export SPARK_MASTER_IP=172.31.12.137
   SPARK_LOCAL_DIRS=/home/brook/sparkdata
   SPARK_DRIVER_MEMORY=1G
   export SPARK_LIBARY_PATH=:$JAVA_HOME/lib:$JAVA_HOME/jre/lib:$HADOOP_HOME/lib/native
   ```

4. 修改slaves文件,增加slave主机名

   ```
   fei-ubuntu
   kun-ubuntu
   ```

5. 将上述的配置分发给所有的slaves

6. 在master上启动spark,若同时启动了hadoop, 输入jps应该有如下输出:

   ```
   brook@HelloWorld:spark$ ./sbin/start-all.sh
   brook@HelloWorld:spark$ jps
   2801 Jps
   2244 ResourceManager
   2374 NodeManager
   2092 SecondaryNameNode
   1900 DataNode
   1740 NameNode
   1421 Master
   1534 Worker
   ```
   而slaves机器上则显示:

   ```
   brook@HelloWorld:spark$ jps
   30658 NodeManager
   30535 DataNode
   30424 Worker
   30815 Jps
   ```


#### 六、测试

1. 在master机器创建一个文本

   ```
   brook@HelloWorld:~$ touch test 
   brook@HelloWorld:~$ vim test
   ```

​      添加一些内容,比如:

```
hello hadoop
hello spark
hello pyspark
```

2. 存入hdfs文件系统

   ```
   brook@HelloWorld:~$ hdfs dfs -mkdir /test  # 创建hdfs文件夹
   brook@HelloWorld:~$ hdfs dfs -put test /test
   brook@HelloWorld:~$ hdfs dfs -ls -R /test
   -rw-r--r--   3 ubuntu supergroup         39 2018-01-11 17:43 /test/test
   ```

3. 启动pyspark

   ```
   1.一般启动方法为python2解释器
   brook@HelloWorld:~$ /opt/spark/bin/pyspark
   2.若有切换为pytahon3解释器
   brook@HelloWorld:~$ PYSPARK_PYTHON=python3 /opt/spark/bin/pyspark
   3.或者在 ~/.bashrc 文件中加入:
   export PYSPARK_PYTHON=python3
   这样在使用第1中方法时会默认使用python3解释器
   ```

4. 测试

   ```
   brook@HelloWorld:~$ PYSPARK_PYTHON=python3 /opt/spark/bin/pyspark
   Python 3.5.2 (default, Nov 23 2017, 16:37:01) 
   [GCC 5.4.0 20160609] on linux
   Type "help", "copyright", "credits" or "license" for more information.
   Setting default log level to "WARN".
   To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
   18/01/11 17:48:00 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
   18/01/11 17:48:01 WARN util.Utils: Service 'SparkUI' could not bind on port 4040. Attempting port 4041.
   18/01/11 17:48:11 WARN metastore.ObjectStore: Failed to get database global_temp, returning NoSuchObjectException
   Welcome to
         ____              __
        / __/__  ___ _____/ /__
       _\ \/ _ \/ _ `/ __/  '_/
      /__ / .__/\_,_/_/ /_/\_\   version 2.2.0
         /_/

   Using Python version 3.5.2 (default, Nov 23 2017 16:37:01)
   SparkSession available as 'spark'.
   >>> path = "/test/test"
   >>> line = sc.textFile(path)
   >>> line.collect()
   ['hello hadoop', 'hello spark', 'hello pyspark']
   >>>
   ```


