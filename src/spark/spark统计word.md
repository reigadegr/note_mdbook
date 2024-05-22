## spark统计词频

### 部署环境

- 下载tgz包

- tar -xzvf *tgz解压缩

- cd  ./$`解压后的目录`/bin

- 启动服务：（执行以下三条命令）
    ```shell
    #启动hdfs
    start-all.sh
    
    /opt/software_package/hadoop-2.7.1/sbin/mr-jobhistory-daemon.sh start historyserver
    
    /opt/software_package/spark/sbin/start-history-server.sh
    ```
    
- 执行shell命令：

  ```shell
   $(find /opt -name spark-shell)
  ```

  以启动scala终端

### 写代码

- 再开一个session，链接到刚才开的虚拟机

- 执行shell命令：

  ```shell
  vim  /root/words.txt
  ```


- 写几个词，用空格分隔。例如以下内容：

  ```txt
  hello me you her
  hello me her
  hello
  ```

- 在第一个session写以下代码：

  ```shell
  sc.textFile("file:///root/words.txt").flatMap(_.split(" ")).map((_,1)).reduceByKey(_+_).collect
  ```

  回车执行就行

