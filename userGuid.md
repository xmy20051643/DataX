# DataX

DataX 是阿里巴巴集团内被广泛使用的离线数据同步工具/平台，实现包括 MySQL、SQL Server、Oracle、PostgreSQL、HDFS、Hive、HBase、OTS、ODPS 等各种异构数据源之间高效的数据同步功能。

# Features

DataX本身作为数据同步框架，将不同数据源的同步抽象为从源头数据源读取数据的Reader插件，以及向目标端写入数据的Writer插件，理论上DataX框架可以支持任意数据源类型的数据同步工作。同时DataX插件体系作为一套生态系统, 每接入一套新数据源该新加入的数据源即可实现和现有的数据源互通。

# System Requirements

| 中间件  | 版本  | 备注 |
| ------------ | ------------ | ------------ |
| linux | - | - |
| jdk   | 1.8.0_332 | 1.8+ |
| python | 2/3 | |
| maven | 3+| |

# Quick Start

* 部署datax
    * 下载源码：
    ``` shell
    $git clone https://github.com/xmy20051643/DataX.git
    ```
    * maven打包软件：
    ``` shell
    $cd ${CODE_DIR}
    $mvn clean package -DskipTests
    ```
    打包成功：
    ```
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 02:44 min
    [INFO] Finished at: 2022-04-27T15:45:59+08:00
    [INFO] Final Memory: 556M/2196M
    [INFO] ------------------------------------------------------------------------
    ```
    打包后程序位置```${CODE_DIR}/datax-assembly/target```目录中：
    ```
    $ls ${CODE_DIR}/datax-assembly/target
    archive-tmp  datax  datax.tar.gz
    $ls ${CODE_DIR}/ls datax-assembly/target/datax/datax/
    bin  conf  job  lib  plugin  script  tmp
    ```


* 配置示例：从stream读取数据并打印到控制台

  * 第一步、创建作业的配置文件（json格式）

    可以通过命令查看配置模板： python datax.py -r {YOUR_READER} -w {YOUR_WRITER}

    ``` shell
    $ cd  {YOUR_DATAX_HOME}/bin
    $  python datax.py -r streamreader -w streamwriter
    DataX (UNKNOWN_DATAX_VERSION), From Alibaba !
    Copyright (C) 2010-2015, Alibaba Group. All Rights Reserved.
    Please refer to the streamreader document:
        https://github.com/alibaba/DataX/blob/master/streamreader/doc/streamreader.md

    Please refer to the streamwriter document:
         https://github.com/alibaba/DataX/blob/master/streamwriter/doc/streamwriter.md

    Please save the following configuration as a json file and  use
         python {DATAX_HOME}/bin/datax.py {JSON_FILE_NAME}.json
    to run the job.

    {
        "job": {
            "content": [
                {
                    "reader": {
                        "name": "streamreader",
                        "parameter": {
                            "column": [],
                            "sliceRecordCount": ""
                        }
                    },
                    "writer": {
                        "name": "streamwriter",
                        "parameter": {
                            "encoding": "",
                            "print": true
                        }
                    }
                }
            ],
            "setting": {
                "speed": {
                    "channel": ""
                }
            }
        }
    }
    ```

    根据模板配置json如下：

    ``` json
    #stream2stream.json
    {
      "job": {
        "content": [
          {
            "reader": {
              "name": "streamreader",
              "parameter": {
                "sliceRecordCount": 10,
                "column": [
                  {
                    "type": "long",
                    "value": "10"
                  },
                  {
                    "type": "string",
                    "value": "hello，你好，世界-DataX"
                  }
                ]
              }
            },
            "writer": {
              "name": "streamwriter",
              "parameter": {
                "encoding": "UTF-8",
                "print": true
              }
            }
          }
        ],
        "setting": {
          "speed": {
            "channel": 5
           }
        }
      }
    }
    ```

  * 第二步：启动DataX

    ``` shell
    $ cd {YOUR_DATAX_DIR_BIN}
    $ python datax.py ./stream2stream.json
    ```

    同步结束，显示日志如下：

    ``` shell
    ...
    2015-12-17 11:20:25.263 [job-0] INFO  JobContainer -
    任务启动时刻                    : 2015-12-17 11:20:15
    任务结束时刻                    : 2015-12-17 11:20:25
    任务总计耗时                    :                 10s
    任务平均流量                    :              205B/s
    记录写入速度                    :              5rec/s
    读出记录总数                    :                  50
    读写失败总数                    :                   0
    ```

# Contact us

Google Groups: [DataX-user](https://github.com/alibaba/DataX)
