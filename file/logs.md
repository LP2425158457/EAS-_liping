### RPC日志提取及各种日志说明

> 针对**性能慢**的情况下拿的日志

#### 客户端RPC日志

- 1、 打开客户端性能日志开关： 在eas\client\deploy\client\PerfLog_Client.properties中，将rpc=off改为rpc=on。（改完后重启客户端） 
- 2、 集群环境先在管理控制台中查找某一具体实例如server2的连接信息，然后修改客户端的服务器连接设置为server2的连接信息。 若是单实例则不需修改，直接进行下一步操作 
- 3、 打开客户端，准备好性能慢的功能点操作（仅准备好操作，不用真正操作） 

#### 服务端RPC日志

- 1、登陆堡垒机的浏览器

- 2、输入http://对应eas应用服务端的IP地址:server1的jmx端口号（例如：http://10.0.40.154:11029/）。**用户名密码：admin/admin**

  > 注：server1的jmx端口号： 查看服务器中如下文件 profiles\server1\config\jmxconnector.properties 中的设置。

> 例如154的路径如下：/kingdee/eas701/eas/server/profiles/server1/config下面的jmxconnector.properties
> 
> ==7.0后默认为11029, EAS6.0默认为11030==

- 3、搜索并找到invokeCounter，点击进入。找到RpcSqlOn和SqlPlanOn，并分别设置为True。最后点击“Apply”按钮应用。 

- 4、 操作性能慢的功能点，即点击保存 

- 5、 关闭开关，待操作结束后，关闭服务器性能日志开关。 http://serverip:port/ 用户名密码：admin/admin 进入后，搜索并找到invokeCounter，点击进入。找到RpcSqlOn和SqlPlanOn，并分别设置为False。最后点击“Apply” 按钮应用。

- 6、 收集日志，请收集以下日志： 

```
（1）服务器端：eas\server\profiles\server*\logs下 的SqlPlanD.V60SP1.log，RpcSqlD.V60SP1.log，apusic.log.0到apusic.log.9 
（2）客户端：eas\client\logs下的log4j.log，rpcD.V60SP1.log，client.log 
注意事项：
 1.操作过程中，请注意记录每个问题功能点的开始时间、结束时间,以便于技术人员分析。 记录格式为：功能点名称 开始时间（HH:MM:SS） 结束时间（HH:MM:SS），在WORD或记事本中记录都可以。 
2.请查看收集到的日志中以rpcD,SqlPlanD,RpcSqlD开头的文件大小是否为0，若为0说明相应的日志没有收集到，请重新检查性能日志开关是否打开，并重新收集；若大小不为0，则说明相应的日志收集到。
 3. 请在服务器上按以上操作收集日志，非服务器的客户端则只需操作步骤1，2及6中的（2）
```

#### 各种日志说明

客户端日志

```
- 1、记录最近一次EAS的运行日志，会记录系统运行的详细情况和出现的异常信息。每次启动客户端会删除原有的内容（开发分析问题需提供）。$EAS_HOME\client\logs\client.log
- 2、EAS运行日志，类似client.log。会保存历史日志，另外还有可控制日志详细程度。受client/deploy/client/log4j.properties控制，需要专业人士协助配置。$EAS_HOME\client\logs\log4j.log
- 3、log4j.log的历史记录，每天记录一个。$EAS_HOME\client\logs\log4j.log.*
- 4、记录自动更新日志，会保存历史记录。 $EAS_HOME\client/logs/autoupdate.log
```

服务端日志

```
- 1、记录了应用服务器启动和运行日志，包括异常信息等。$EAS_HOME\eas\server\profiles\server1\logs\apusic.log.0
- 2、记录的是管理控制台的服务器端日志。 $EAS_HOME\eas\admin\logs\admin.log
- 3、记录的是管理控制台客户端的日志。 $EAS_HOME\eas\admin\logs\admin_client.log
- 4、记录的是权限日志。$EAS_HOME\eas\server\profiles\server1\logs\PermissionTrace.log
- 5、记录的是botp日志。 $EAS_HOME\server\profiles\serverXXX\logs\botpTrace.log
```
