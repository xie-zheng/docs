# MySQL

GreptimeCloud 可以通过 MySQL 协议访问，兼容大多数标准客户端和驱动程序，其连接使用 TLS 加密。有关更多信息，请参阅 GreptimeDB 的 [MySQL 客户端](https://docs.greptime.cn/user-guide/clients/mysql)。

要连接到 GreptimeCloud，使用以下信息：

- Host: `<host>`
- Port: `4002`
- Database: `<dbname>`
- Username: `<username>`
- Password: *Your GreptimeCloud service password*

## MySQL CLI

使用 `mysql` CLI 连接到 GreptimeCloud 服务实例。

```shell
mysql --ssl-mode=REQUIRED -u <username> -p -h <host> -P 4002 -A <dbname>
```

## MariaDB CLI

MariaDB CLI 与原始的 MySQL 的 `ssl` 参数有些许不同：

```shell
mysql --ssl -u <username> -p -h <host> -P 4002 -A <dbname>
```

## JDBC URL

使用以下连接字符串连接你的 JDBC 客户端。记得将 *PASSWORD* 替换为 GreptimeCloud service 的密码。

```
jdbc:mysql://<host>:4002/<dbname>?user=<username>&password=PASSWORD
```
