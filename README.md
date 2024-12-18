Python 3.11.2 使用ssr 

兼容ProxyPanel & SSRPanel的自改版SSR(R)后端，可兼容原版SS、SSR，本版本是带有IP自动上报功能的

## 安装
```
git clone https://github.com/110560/shadowsocksr.git
```


## 更新软件源
#### CentOS
- yum update

#### Python3.11.2


```
apt update && apt install -y libsqlite3-dev libffi-dev python3-pip git
```

安装shadowsocks依赖

```
cd shadowsocksr
pip install -r requestment.txt  --break-system-packages
```
返回结果
```
python3 server.py 
```

---


#### 编辑节点配置（混淆、协议、限速、IPV6）

    vi user-mysql.json

    protocol 协议，带 _compatible 结尾兼容 原版，直接用原版可以改为 origin
    protocol_param 协议参数，配置了的话，客户端也要一致
    obfs 混淆 tls1.2_ticket_auth 可以限制客户端数量 tls1.2_ticket_auth_compatible 兼容原版，直接用原版可以改为 plain
    obfs_param 混淆参数，当obfs为 tls1.2_ticket_auth 的时候，这个值为 1 到 256 之间，表示限制客户端数量
    additional_ports 单端口配置，请看wiki
    additional_ports_only 强制单端口，改为true则所有非设置的单端口都无法连接，只能用additional_ports设置的那些端口连接
    dns_ipv6 为true时，会优先走ipv6，需要节点服务器至少有一个2开头的ipv6地址（有时候会导致IPV4失效，不推荐开启，你可以自己试试）
    connect_verbose_info 为1时记录用户访问网址，推荐打开，可以清楚知道连接成功与否
    redirect 请求失败时返回信息伪造成访问配置里网址


#### 编辑数据库连接信息

    vi usermysql.json

    host 数据库地址，如果是本机就是127.0.0.1
    port 数据库连接端口
    user 数据库连接用户，不推荐使用root
    password 数据库连接密码
    db 面板所在数据库
    node_id 节点ID，对应面板里的 节点列表 最左侧的id（请先将面板搭建好，然后创建一个节点，就有节点ID了）
    transfer_mul 节点流量计算比例，默认1.0，填1也可以，1表示：用了100M算100M，10表示用了100M算1000M，0.1表示用了100M算10M。

#### 运行、关闭、看日志

    sh logrun.sh
    sh stop.sh
    sh tail.sh

#### 其他

数据库机的 iptables、firewall 得对本节点IP开放
数据库机的 mysql 的对本节点进行授权（不推荐使用root账号）
