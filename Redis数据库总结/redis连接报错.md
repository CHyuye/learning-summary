## redis-cli连接时出现Could not connect to Redis at 127.0.0.1:6379: Connection refused

* **分析问题**：redis-server(服务端)没启动

* **具体步骤**

  > 1. cd到redis的配置文件目录 [根据自己安装时的路径输入]
  >
  >    ```python
  >    cd /usr/local/etc/redis
  >    ```
  >
  > 2. 修改redis的配置信息 [一定要加上sudo，只有root权限才能修改保存]
  >
  >    ```python
  >    sudo vim redis.conf
  >    ```
  >
  > 3. 根据需要修改以下值
  >
  >    * 设置默认后台运行
  >
  >      ```python
  >      daemonize yes
  >      ```
  >
  >    * 设置绑定IP地址
  >
  >      <u>192.168.232.xxx是你的本机IP与物理网卡绑定的IP地址，127.0.0.1是回送地址，一般用于测试不需要联网</u>
  >
  >      ```python
  >      bind 192.168.232.xxx 127.0.0.1
  >      ```
  >
  >      <u>两个IP地址之间空格隔开</u>，这样做的好处是方便其他计算机远程访问
  >
  > 4. 保存退出 [ESC键，退出编辑，' ! '表示强制退出]
  >
  >    ```python
  >    :wq!
  >    ```

  ***

  * **重新启动**

  ```python
  redis-server /usr/local/etc/redis/redis.conf
  ```

  检查一下redis服务器是否启动

  ```python
  ps aux | grep redis
  ```

  ![](G:\文章写入文件夹\Redis数据库总结\redis.png)~~重新连接一下即可，至此完结~~

  ​