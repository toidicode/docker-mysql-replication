# Docker MySql Master Slave

### Install

Create `.env` file with keys or move `.env.example` to `.env` and edit it. 
 
 ```
MYSQL_DATABASE=
MYSQL_ROOT_PASSWORD=
MYSQL_USER=
MYSQL_PASSWORD=

SUBNET=172.30.0.0/16
MASTER_PORT=
MASTER_IP=

SLAVE_PORT=
SLAVE_IP=


```

Change `bind-address` on `config/master/cus.cnf` to your master IP and change `bind-address` on `config/slave/cus.cnf` to your slave IP.

> If you want more configuration, you can add these 2 files

Run command:

```
docker-compose up -d mysql_master mysql-salve
```

### Config MySql.

You need login to mysql container then login to mysql console.

#### On MySql Master

On MySql console run below command:

```
HOW MASTER STATUS;
```

The result should look like below:

```
mysql> show master status;
+-------------------------+----------+--------------+------------------+-------------------+
| File                    | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+-------------------------+----------+--------------+------------------+-------------------+
| 55ee2c9739f6-bin.000003 |      816 |              |                  |                   |
+-------------------------+----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)

```

You need to pay attention to the following parameters:

```
55ee2c9739f6-bin.000003

816
```

#### On MySql Slave

On MySql console run below command:

```
CHANGE MASTER TO MASTER_HOST='172.30.0.2',MASTER_USER='slave_user', MASTER_PASSWORD='123456', MASTER_LOG_FILE='55ee2c9739f6-bin.000003', MASTER_LOG_POS=816;
```

With `55ee2c9739f6-bin.000003` and `816` is output of the mysql master command.

Continue to run the following command.

```
START SLAVE;
```

Finish. You can check status by command:

```
SHOW SLAVE STATUS;
```
