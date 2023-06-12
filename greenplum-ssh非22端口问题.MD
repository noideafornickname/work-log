# 服务器修改ssh端口后, greenplum6.6版本启动不了

### 现象: greenplum集群修改了ssh端口后, gpstart无法启动系统, 日志中显示, ssh 22端口拒绝连接

### 分析: 根据日志分析, greenplum使用了ssh默认的22端口去连接数据节点进行相关操作, 而服务器修改了ssh默认的端口

### 解决过程:  
1: 查看文档, 发现greenplum主节点配置文件只是配置了数据节点的ip, 没有配置ssh端口的方式
2: 查询资料, 发现greenplum版本的一些问题, 代码中未提供指定ssh端口的方式, 只能修改代码, 尝试编辑gpssh-exkeys脚本和相关python`/lib/python/gppylib/commands/base.py`文件, 手动再执行ssh命令的地方加上
数据节点的端口号`-p 2222`; 尝试启动数据库成功, 但是个别数据节点segment未正常加载, 尝试执行gprecoversegment进
