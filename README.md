# WildfireChat Docker

## IniT

```bash
mkdir -p chat/chat-app chat/chat-server chat/chat-mysql
cd chat
wget `curl https://api.github.com/repos/wildfirechat/server/releases/latest | grep "browser_download_url" | cut -d '"' -f 4` -O chat-server.tar.gz
wget `curl https://api.github.com/repos/wildfirechat/im-app_server/releases/latest | grep "browser_download_url" | cut -d '"' -f 4` -O app-server.tar.gz
tar -zxvf chat-server.tar.gz -C chat-server
tar -zxvf app-server.tar.gz -C chat-app
```

## Docker-Compose

```dockerfile
version: "3"
services: 
  chat_server: 
    hostname: chat_server
    container_name: chat_server
    image: xrsec/java:8
    restart: on-failure:3
    network_mode: "host"
    working_dir: "/chat-server"
    command: "sh ./bin/wildfirechat.sh"
    volumes: 
      - "./chat-server:/chat-server"

  chat_app: 
    hostname: chat_app
    container_name: chat_app
    image: xrsec/java:8
    restart: on-failure:3
    network_mode: "host"
    working_dir: "/chat-app"
    command: "java -jar app-0.52.jar"
    volumes: 
      - "./chat-app:/chat-app"

  chat_mysql:
    hostname: chat_mysql
    image: mariadb:latest
    container_name: chat_mysql
    restart: on-failure:3
    network_mode: "host"
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    environment:
        - MYSQL_ROOT_PASSWORD=ihiVcJNgxLMvd9
        - MYSQL_PASSWORD=VBUzbkCrcAzU4b
        - MYSQL_DATABASE=wfchat
        - MYSQL_USER=wildfirechat
    volumes:
        - "./chat-mysql:/var/lib/mysql"
```

## Tree

```ini
➜  ~ tree
.
└── chat
    ├── chat-app
    ├── chat-mysql
    ├── chat-server
    └── docker-compose.yml

20 directories, 744 files
```


