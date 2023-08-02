## 什么是redis
基于内存的key-value结构数据库
* 基于内存存储,读写性能高
* 存热点数据(热点商品,咨询,新闻),短时间之内,大量用户访问

### Redis用用场景
* 缓存
* 任务队列
* 消息队列
* 分布式锁

### redis安装
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731131036.png)

### linux启动redis服务(在src目录下)
启动服务
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731131249.png)
启动客户端
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731131313.png)

后台启动:
修改conf文件,将ase中的no改为yes
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731131727.png)

修改密码后的客户端

![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731132249.png)
需要认证才能输入命令
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731132359.png)

当前的redis默认只能本地连接,windows主机上无法访问
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731132636.png)

配置文件中,默认只能本地连接
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731132838.png)

现在可以远程连接
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731133115.png)

### 五种常用数据类型
string
hash
list(任务队列,先进先出)
set
sorted set
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731133210.png)

![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731134227.png)

![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731134236.png)

![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731134416.png)

![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731134905.png)

![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731135004.png)

### 通用命令
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731135141.png)

### java程序操作redis
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731135231.png)
spring对redis客户端进行了整合
```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

```
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230731140300.png)