---
title: "在mac osx上安装hadoop"
date: 2017-12-15T19:22:01+08:00
tags: ["mac", " hadoop", " bigdata"]
author: "ox0spy"
categories: ["bigdata"]
---

## 安装Hadoop

只是安装来学习Hadoop，所以，在自己笔记本上安装的。正式的线上环境会用多台机器搭建集群 或者 直接使用AWS的EMR。

我希望尽可能的通过包管理安装软件，而不是手动下载安装。

	$ brew install hadoop

### 修改配置文件

Hadoop安装路径: `brew --prefix hadoop` 。

配置文件路径: `$(brew --prefix hadoop)/libexec/etc/hadoop/`。

**core-site.xml:**

	<configuration>
		<property>
		    <name>fs.defaultFS</name>
		    <value>hdfs://localhost:9000</value>
		</property>
	</configuration>

**hdfs-site.xml:**

	<configuration>
		<property>
		    <name>dfs.replication</name>
		    <value>1</value>
		</property>
	</configuration>

**mapred-site.xml:**

	<configuration>
		<property>
		    <name>mapreduce.framework.name</name>
		    <value>yarn</value>
		</property>
	</configuration>

**yarn-site.xml:**

	<configuration>
		<property>
		    <name>yarn.nodemanager.aux-services</name>
		    <value>mapreduce_shuffle</value>
		</property>
	</configuration>

### 启动HDFS (NameNode, DataNode)

1. 格式化HDFS文件系统：`$ bin/hdfs namenode -format`
2. 启动NameNode 和 DataNode：`$ sbin/start-dfs.sh`
3. 创建HDFS目录，为运行MapReduce做准备

	```
	$ bin/hdfs dfs -mkdir /user
	$ bin/hdfs dfs -mkdir /user/<username>
	$ bin/hdfs dfs -put libexec/etc/hadoop input
	```

4. 运行demo:

	```
	$ bin/hadoop jar libexec/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.8.0.jar grep input output 'dfs[a-z.]+'
	```

5. 查看demo输出: `$ bin/hdfs dfs -cat outpu/*`
6. 停止NameNode 和 DataNode: `$ sbin/stop-dfs.sh`

修改 ~/.zshrc ，将hadoop的bin, sbin加入PATH环境变量：

	# hadoop
	HADOOP_HOME=$(brew --prefix hadoop)
	export PATH="$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH"

### YARN (ResourceManager)

1. 启动:

	```
	$ sbin/start-yarn.sh
	```

2. 查看: [http://localhost:8088/](http://localhost:8088/)

3. 停止:

	```
	$ sbin/stop-yarn.sh
	```

## 参考文档

- [Hadoop: Setting up a Single Node Cluster.](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html)