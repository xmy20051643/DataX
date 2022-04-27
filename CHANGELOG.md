# 2022-04-27
1. 添加用于打包的项目datax-assembly,同时将package.xml移动到该项目main/assembly下，
打包命令：
``` shell
$cd ${CODE_DIR}
$mvn clean package -DskipTests
```
打包成功日志：
``` shell
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
```

2. 代码中把```import com.alibaba.datax.common.element.*```去掉，禁止通过*进行包引入；
3. ${CODE_DIR}/pom.xml中删除maven-assembly-plugin插件部分；
