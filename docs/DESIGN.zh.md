# DESIGN

Zweig base on Snabb switch, powered by LuaJIT

- Zweig built in with etcd. And can use external etcd service


#　SPEC

1.　数据日志文件存在至少两个　position,
　
　　　－　writePos，　当前数据的写入位置
     - commitPos, 　当前数据的提交位置

2.　系统默认提供 discover service，　用于集群启动，　理论上，集群一旦启动，就不再需要　discover;
     此即为 etcd　的　discover service
    （目前，只支持外部的 etcd　服务）
    　
3.　CLI servcie，　提供 repl 的命令行接口

4.　参考 Kafka，　提供基于 Vagrant 的部署脚本
　　　
　　　－ nodes
     - management console
        * etcd web admin ui, etc

5. Topic, 一系列相似的数据流，　
    －　存在模板 topic，　为　_template1
    －　数据流的配置可以复制
    －　可选参数
    　　* 复制因子
    　　* 几份或多少比例的副本确认，认为数据确认
    　　* 数据确认前的缓冲区大小（类似　kafka　的　batch　productor)
       * Block Size，每写入一段长度后，会切换到新文件，此为文件的大小，默认为 64M
       * Sync Policy， 数据的同步策略，立即同步，还是 写满了一个区块再同步
       * Topic Size    在队列中保存数据的大小，为Block 的倍数，向上取整
       * Topic Window  在队列中保存数据的时间窗口，实际以Block为单位
       * S3_XXXX       访问对象存储的参数
       * S3 Policy     当数据超过 Topic Size 时，上传 | 当 Block 写完时 上传

6. 数据文件压缩采用 facebook 的 zstd 压缩，提供统一的外部词典

7. 系统各节点直接传输数据，采用 UDP， 可以对 UDP 本身限速（现在包文数量）

8. 支持 Lua 脚本的热更新

9. 提供 Web API 用于管理

10.提供抽象的对存储设备的描述接口， 可以在多个存储设备中，实现负载均衡。能够报告 系统退化的情况
   - 仅 HDD，对 SSD 暂不适用

11.日志采集的 Agent 端，集成 facebook 的 osquery

12.系统支持从 epoll 中读取数据， 支持从多个设备中读取数据

13.系统支持在 stream 数据上，直接进行计算

14.系统默认提供 minino 的部署策略