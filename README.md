# container-playgroud
### CREATE DOCKER NETWORK
`docker network create todo-app`

### GET LIST OF DOCKER NETWORKS
`docker network ls`
```
NETWORK ID          NAME                DRIVER              SCOPE
2c48d99e0f53        bridge              bridge              local
b9006d84da7b        host                host                local
618da7d7e94c        none                null                local
c4429a21cab9        todo-app            bridge              local
```

### INSPECT DOCKER NETWORK
`docker network inspect todo-app `
```
[
    {
        "Name": "todo-app",
        "Id": "c4429a21cab9653a4a4f9d76c44a02002c7639e212abec9201942bf4bdd60972",
        "Created": "2021-12-07T17:44:37.806928366+05:30",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "0b942f9f800e9e7fee2d344adc33ea7764b5303a66d3cd58c1fa0e627f138c33": {
                "Name": "great_dubinsky",
                "EndpointID": "1a2ec4d9b77cdfe26c38a0ba387b8948d1473897385d5a7bfe867897432331cd",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            },
            "8873d0d6cddfdff33fb13030fc5cedeb9996f94e0260b5ab65f928342fcc127a": {
                "Name": "cranky_wu",
                "EndpointID": "9adda1fd101a881d94e688c725a114ceaf2dce65c0bc2fc08b0c1bb473245435",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```



### RUN TODO-APP DOCKER CONTAINER
 `docker run -d --network todo-app --network-alias mysql -v todo-mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=todos mysql:5.7`

### LOGIN TO MYSQL DOCKER CONTAINER
`docker exec -it <mysql-container-id> mysql -u root -p`

```
Enter password:
Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.36 MySQL Community Server (GPL)

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

### CREATE USER & GRANT PERMISSIONS IN MYSQL SERVER
`CREATE USER 'root'@'%' IDENTIFIED BY 'secret'`
`GRANT ALL ON *.* TO 'root'@'%' WITH GRANT OPTION`
`FLUSH PRIVILEGES`
### START TODO-APP DOCKER CONTAINER
`docker run -dp 3000:3000 -w /app -v "$(pwd):/app" --network todo-app -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=secret -e MYSQL_DB=todos node:12-alpine sh -c "yarn install && yarn run dev"`
### TEST TODO-APP IN BROWSER
**http://127.0.0.1:3000/**


### GET LOGS OF MYSQL DOCKER CONTAINER
`docker logs -f <mysql-container-id>`
```
07 12:24:14+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.36-1debian10 started.
07 12:24:14+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
07 12:24:14+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.36-1debian10 started.
07 12:24:14+00:00 [Note] [Entrypoint]: Initializing database files
07T12:24:14.944757Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
07T12:24:15.478696Z 0 [Warning] InnoDB: New log files created, LSN=45790
07T12:24:15.558554Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
07T12:24:15.635656Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 97c07f5a-5758-11ec-9e15-0242ac120002.
07T12:24:15.747642Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
07T12:24:16.476665Z 0 [Warning] A deprecated TLS version TLSv1 is enabled. Please use TLSv1.2 or higher.
07T12:24:16.476779Z 0 [Warning] A deprecated TLS version TLSv1.1 is enabled. Please use TLSv1.2 or higher.
07T12:24:16.477192Z 0 [Warning] CA certificate ca.pem is self signed.
07T12:24:16.748655Z 1 [Warning] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
07 12:24:21+00:00 [Note] [Entrypoint]: Database files initialized
07 12:24:21+00:00 [Note] [Entrypoint]: Starting temporary server
07 12:24:21+00:00 [Note] [Entrypoint]: Waiting for server startup
07T12:24:21.877697Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
07T12:24:21.878859Z 0 [Note] mysqld (mysqld 5.7.36) starting as process 75 ...
07T12:24:21.883885Z 0 [Note] InnoDB: PUNCH HOLE support available
07T12:24:21.883956Z 0 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
07T12:24:21.884011Z 0 [Note] InnoDB: Uses event mutexes
07T12:24:21.884062Z 0 [Note] InnoDB: GCC builtin __atomic_thread_fence() is used for memory barrier
07T12:24:21.884120Z 0 [Note] InnoDB: Compressed tables use zlib 1.2.11
07T12:24:21.884174Z 0 [Note] InnoDB: Using Linux native AIO
07T12:24:21.884387Z 0 [Note] InnoDB: Number of pools: 1
07T12:24:21.884557Z 0 [Note] InnoDB: Using CPU crc32 instructions
07T12:24:21.885838Z 0 [Note] InnoDB: Initializing buffer pool, total size = 128M, instances = 1, chunk size = 128M
07T12:24:21.891217Z 0 [Note] InnoDB: Completed initialization of buffer pool
07T12:24:21.892756Z 0 [Note] InnoDB: If the mysqld execution user is authorized, page cleaner thread priority can be changed. See the man page of setpriority().
07T12:24:21.904195Z 0 [Note] InnoDB: Highest supported file format is Barracuda.
07T12:24:22.015710Z 0 [Note] InnoDB: Creating shared tablespace for temporary tables
07T12:24:22.016032Z 0 [Note] InnoDB: Setting file './ibtmp1' size to 12 MB. Physically writing the file full; Please wait ...
07T12:24:22.131142Z 0 [Note] InnoDB: File './ibtmp1' size is now 12 MB.
07T12:24:22.131976Z 0 [Note] InnoDB: 96 redo rollback segment(s) found. 96 redo rollback segment(s) are active.
07T12:24:22.131985Z 0 [Note] InnoDB: 32 non-redo rollback segment(s) are active.
07T12:24:22.132259Z 0 [Note] InnoDB: Waiting for purge to start
07T12:24:22.182506Z 0 [Note] InnoDB: 5.7.36 started; log sequence number 2749723
07T12:24:22.182932Z 0 [Note] Plugin 'FEDERATED' is disabled.
07T12:24:22.187828Z 0 [Note] Found ca.pem, server-cert.pem and server-key.pem in data directory. Trying to enable SSL support using them.
07T12:24:22.188063Z 0 [Note] Skipping generation of SSL certificates as certificate files are present in data directory.
07T12:24:22.188131Z 0 [Warning] A deprecated TLS version TLSv1 is enabled. Please use TLSv1.2 or higher.
07T12:24:22.188221Z 0 [Warning] A deprecated TLS version TLSv1.1 is enabled. Please use TLSv1.2 or higher.
07T12:24:22.188055Z 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/ib_buffer_pool
07T12:24:22.189381Z 0 [Note] InnoDB: Buffer pool(s) load completed at xxxxxx12:24:22
07T12:24:22.189890Z 0 [Warning] CA certificate ca.pem is self signed.
07T12:24:22.189987Z 0 [Note] Skipping generation of RSA key pair as key files are present in data directory.
07T12:24:22.207354Z 0 [Warning] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
07T12:24:22.213614Z 0 [Note] Event Scheduler: Loaded 0 events
07T12:24:22.213898Z 0 [Note] mysqld: ready for connections.
 '5.7.36'  socket: '/var/run/mysqld/mysqld.sock'  port: 0  MySQL Community Server (GPL)
07 12:24:22+00:00 [Note] [Entrypoint]: Temporary server started.
 Unable to load '/usr/share/zoneinfo/iso3166.tab' as time zone. Skipping it.
 Unable to load '/usr/share/zoneinfo/leap-seconds.list' as time zone. Skipping it.
 Unable to load '/usr/share/zoneinfo/zone.tab' as time zone. Skipping it.
 Unable to load '/usr/share/zoneinfo/zone1970.tab' as time zone. Skipping it.
07 12:24:25+00:00 [Note] [Entrypoint]: Creating database todos

07 12:24:25+00:00 [Note] [Entrypoint]: Stopping temporary server
07T12:24:25.838860Z 0 [Note] Giving 0 client threads a chance to die gracefully
07T12:24:25.839011Z 0 [Note] Shutting down slave threads
07T12:24:25.839086Z 0 [Note] Forcefully disconnecting 0 remaining clients
07T12:24:25.839155Z 0 [Note] Event Scheduler: Purging the queue. 0 events
07T12:24:25.839262Z 0 [Note] Binlog end
07T12:24:25.839708Z 0 [Note] Shutting down plugin 'ngram'
07T12:24:25.839775Z 0 [Note] Shutting down plugin 'partition'
07T12:24:25.839832Z 0 [Note] Shutting down plugin 'BLACKHOLE'
07T12:24:25.839897Z 0 [Note] Shutting down plugin 'ARCHIVE'
07T12:24:25.839959Z 0 [Note] Shutting down plugin 'PERFORMANCE_SCHEMA'
07T12:24:25.840032Z 0 [Note] Shutting down plugin 'MRG_MYISAM'
07T12:24:25.840104Z 0 [Note] Shutting down plugin 'MyISAM'
07T12:24:25.840172Z 0 [Note] Shutting down plugin 'INNODB_SYS_VIRTUAL'
07T12:24:25.840229Z 0 [Note] Shutting down plugin 'INNODB_SYS_DATAFILES'
07T12:24:25.840293Z 0 [Note] Shutting down plugin 'INNODB_SYS_TABLESPACES'
07T12:24:25.840357Z 0 [Note] Shutting down plugin 'INNODB_SYS_FOREIGN_COLS'
07T12:24:25.840414Z 0 [Note] Shutting down plugin 'INNODB_SYS_FOREIGN'
07T12:24:25.840479Z 0 [Note] Shutting down plugin 'INNODB_SYS_FIELDS'
07T12:24:25.840565Z 0 [Note] Shutting down plugin 'INNODB_SYS_COLUMNS'
07T12:24:25.840632Z 0 [Note] Shutting down plugin 'INNODB_SYS_INDEXES'
07T12:24:25.840688Z 0 [Note] Shutting down plugin 'INNODB_SYS_TABLESTATS'
07T12:24:25.840752Z 0 [Note] Shutting down plugin 'INNODB_SYS_TABLES'
07T12:24:25.840804Z 0 [Note] Shutting down plugin 'INNODB_FT_INDEX_TABLE'
07T12:24:25.840884Z 0 [Note] Shutting down plugin 'INNODB_FT_INDEX_CACHE'
07T12:24:25.840939Z 0 [Note] Shutting down plugin 'INNODB_FT_CONFIG'
07T12:24:25.841008Z 0 [Note] Shutting down plugin 'INNODB_FT_BEING_DELETED'
07T12:24:25.841072Z 0 [Note] Shutting down plugin 'INNODB_FT_DELETED'
07T12:24:25.841128Z 0 [Note] Shutting down plugin 'INNODB_FT_DEFAULT_STOPWORD'
07T12:24:25.841199Z 0 [Note] Shutting down plugin 'INNODB_METRICS'
07T12:24:25.841263Z 0 [Note] Shutting down plugin 'INNODB_TEMP_TABLE_INFO'
07T12:24:25.841319Z 0 [Note] Shutting down plugin 'INNODB_BUFFER_POOL_STATS'
07T12:24:25.841388Z 0 [Note] Shutting down plugin 'INNODB_BUFFER_PAGE_LRU'
07T12:24:25.841452Z 0 [Note] Shutting down plugin 'INNODB_BUFFER_PAGE'
07T12:24:25.841509Z 0 [Note] Shutting down plugin 'INNODB_CMP_PER_INDEX_RESET'
07T12:24:25.841574Z 0 [Note] Shutting down plugin 'INNODB_CMP_PER_INDEX'
07T12:24:25.841638Z 0 [Note] Shutting down plugin 'INNODB_CMPMEM_RESET'
07T12:24:25.841694Z 0 [Note] Shutting down plugin 'INNODB_CMPMEM'
07T12:24:25.841763Z 0 [Note] Shutting down plugin 'INNODB_CMP_RESET'
07T12:24:25.841828Z 0 [Note] Shutting down plugin 'INNODB_CMP'
07T12:24:25.841884Z 0 [Note] Shutting down plugin 'INNODB_LOCK_WAITS'
07T12:24:25.841955Z 0 [Note] Shutting down plugin 'INNODB_LOCKS'
07T12:24:25.842019Z 0 [Note] Shutting down plugin 'INNODB_TRX'
07T12:24:25.842075Z 0 [Note] Shutting down plugin 'InnoDB'
07T12:24:25.842174Z 0 [Note] InnoDB: FTS optimize thread exiting.
07T12:24:25.842349Z 0 [Note] InnoDB: Starting shutdown...
07T12:24:25.942622Z 0 [Note] InnoDB: Dumping buffer pool(s) to /var/lib/mysql/ib_buffer_pool
07T12:24:25.947838Z 0 [Note] InnoDB: Buffer pool(s) dump completed at xxxxxx 12:24:25
07T12:24:27.369177Z 0 [Note] InnoDB: Shutdown completed; log sequence number 12659645
07T12:24:27.370494Z 0 [Note] InnoDB: Removed temporary tablespace data file: "ibtmp1"
07T12:24:27.370611Z 0 [Note] Shutting down plugin 'MEMORY'
07T12:24:27.370678Z 0 [Note] Shutting down plugin 'CSV'
07T12:24:27.370734Z 0 [Note] Shutting down plugin 'sha256_password'
07T12:24:27.370797Z 0 [Note] Shutting down plugin 'mysql_native_password'
07T12:24:27.370976Z 0 [Note] Shutting down plugin 'binlog'
07T12:24:27.372286Z 0 [Note] mysqld: Shutdown complete

07 12:24:27+00:00 [Note] [Entrypoint]: Temporary server stopped

07 12:24:27+00:00 [Note] [Entrypoint]: MySQL init process done. Ready for start up.

07T12:24:28.042604Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
07T12:24:28.043782Z 0 [Note] mysqld (mysqld 5.7.36) starting as process 1 ...
07T12:24:28.045717Z 0 [Note] InnoDB: PUNCH HOLE support available
07T12:24:28.045788Z 0 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
07T12:24:28.045862Z 0 [Note] InnoDB: Uses event mutexes
07T12:24:28.045915Z 0 [Note] InnoDB: GCC builtin __atomic_thread_fence() is used for memory barrier
07T12:24:28.045964Z 0 [Note] InnoDB: Compressed tables use zlib 1.2.11
07T12:24:28.046032Z 0 [Note] InnoDB: Using Linux native AIO
07T12:24:28.046304Z 0 [Note] InnoDB: Number of pools: 1
07T12:24:28.046484Z 0 [Note] InnoDB: Using CPU crc32 instructions
07T12:24:28.047605Z 0 [Note] InnoDB: Initializing buffer pool, total size = 128M, instances = 1, chunk size = 128M
07T12:24:28.053941Z 0 [Note] InnoDB: Completed initialization of buffer pool
07T12:24:28.055691Z 0 [Note] InnoDB: If the mysqld execution user is authorized, page cleaner thread priority can be changed. See the man page of setpriority().
07T12:24:28.067079Z 0 [Note] InnoDB: Highest supported file format is Barracuda.
07T12:24:28.079896Z 0 [Note] InnoDB: Creating shared tablespace for temporary tables
07T12:24:28.080088Z 0 [Note] InnoDB: Setting file './ibtmp1' size to 12 MB. Physically writing the file full; Please wait ...
07T12:24:28.138352Z 0 [Note] InnoDB: File './ibtmp1' size is now 12 MB.
07T12:24:28.139100Z 0 [Note] InnoDB: 96 redo rollback segment(s) found. 96 redo rollback segment(s) are active.
07T12:24:28.139242Z 0 [Note] InnoDB: 32 non-redo rollback segment(s) are active.
07T12:24:28.139519Z 0 [Note] InnoDB: Waiting for purge to start
07T12:24:28.189930Z 0 [Note] InnoDB: 5.7.36 started; log sequence number 12659645
07T12:24:28.190812Z 0 [Note] Plugin 'FEDERATED' is disabled.
07T12:24:28.196793Z 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/ib_buffer_pool
07T12:24:28.206863Z 0 [Note] InnoDB: Buffer pool(s) load completed at xxxxxx12:24:28
07T12:24:28.209163Z 0 [Note] Found ca.pem, server-cert.pem and server-key.pem in data directory. Trying to enable SSL support using them.
07T12:24:28.209423Z 0 [Note] Skipping generation of SSL certificates as certificate files are present in data directory.
07T12:24:28.209639Z 0 [Warning] A deprecated TLS version TLSv1 is enabled. Please use TLSv1.2 or higher.
07T12:24:28.209828Z 0 [Warning] A deprecated TLS version TLSv1.1 is enabled. Please use TLSv1.2 or higher.
07T12:24:28.210828Z 0 [Warning] CA certificate ca.pem is self signed.
07T12:24:28.211168Z 0 [Note] Skipping generation of RSA key pair as key files are present in data directory.
07T12:24:28.212117Z 0 [Note] Server hostname (bind-address): '*'; port: 3306
07T12:24:28.212372Z 0 [Note] IPv6 is available.
07T12:24:28.212911Z 0 [Note]   - '::' resolves to '::';
07T12:24:28.213197Z 0 [Note] Server socket created on IP: '::'.
07T12:24:28.223964Z 0 [Warning] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
07T12:24:28.233692Z 0 [Note] Event Scheduler: Loaded 0 events
07T12:24:28.234043Z 0 [Note] mysqld: ready for connections.
Version: '5.7.36'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server (GPL)

```
