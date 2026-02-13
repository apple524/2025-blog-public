# 安装环境
- 系统： Rocky 8 （关闭SE Linux）
- 操作系统：建议使用Linux发行版，如Ubuntu 20.04 LTS。
- 内存：至少4GB RAM。
- CPU：至少2核。
- 磁盘空间：至少40GB的可用空间。
- 网络：具有稳定的互联网连接，并允许必要的端口访问。
- 浏览器：Firefox、Chrome、Safari、Edge（使用最新版）
- Teable   https://github.com/teableio/teable

# 安装环境
## Rocky 8 最小化安装
## 关闭SELinux
```
vim /etc/selinux/config
SELINUX=disabled
```

## 更新国内镜像源

参考 [《一键更新Linux源工具》](https://wiki.mwifi.top:8888/zh/Linux%E7%9B%B8%E5%85%B3/%E4%B8%80%E9%94%AE%E6%9B%B4%E6%96%B0Linux%E6%BA%90%E5%B7%A5%E5%85%B7)
更新Rocky源及update系统补丁

## 安装Docker

参考 [《Docker安装脚本》](https://wiki.mwifi.top:8888/zh/Linux相关/Docker安装脚本)
安装Docker 和 Docker compose

# 安装Teable
## 在home下创建docker-compose 文件
```
cd /home
mkdir teable
cd teable
```
创建一个 docker-compose.yaml
```
vi docker-compose.yaml
```

```
services:
  teable:
    image: registry.cn-shenzhen.aliyuncs.com/teable/teable-ee:latest
    restart: always
    ports:
      - '3000:3000'
    volumes:
      - teable-data:/app/.assets:rw
    env_file:
      - .env
    environment:
      - NEXT_ENV_IMAGES_ALL_REMOTE=true
    networks:
      - teable
    depends_on:
      teable-db:
        condition: service_healthy
      teable-cache:
        condition: service_healthy
    healthcheck:
      test: ['CMD', 'curl', '-f', 'http://localhost:3000/health']
      start_period: 5s
      interval: 5s
      timeout: 3s
      retries: 3

  teable-db:
    image: registry.cn-shenzhen.aliyuncs.com/teable/postgres:15.4
    restart: always
    ports:
      - '42345:5432'
    volumes:
      - teable-db:/var/lib/postgresql/data:rw
    environment:
      - POSTGRES_DB=${POSTGRES_DB}
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
    networks:
      - teable
    healthcheck:
      test: ['CMD-SHELL', "sh -c 'pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}'"]
      interval: 10s
      timeout: 3s
      retries: 3

  teable-cache:
    image: registry.cn-shenzhen.aliyuncs.com/teable/redis:7.2.4
    restart: always
    expose:
      - '6379'
    volumes:
      - teable-cache:/data:rw
    networks:
      - teable
    command: redis-server --appendonly yes --requirepass ${REDIS_PASSWORD}
    healthcheck:
      test: ['CMD', 'redis-cli', '--raw', 'incr', 'ping']
      interval: 10s
      timeout: 3s
      retries: 3

networks:
  teable:
    name: teable-network

volumes:
  teable-db: {}
  teable-data: {}
  teable-cache: {}
```

创建一个 .env 文件
```
vi .env
```
```
# 替换下面默认密码, 推荐使用 8 位以上的强密码。
POSTGRES_PASSWORD=replace_this_password
REDIS_PASSWORD=replace_this_password
SECRET_KEY=replace_this_secret_key

# 请将下面替换为可公开访问的地址
PUBLIC_ORIGIN=http://127.0.0.1:3000

# ---------------------

# Postgres
POSTGRES_HOST=teable-db
POSTGRES_PORT=5432
POSTGRES_DB=teable
POSTGRES_USER=teable

# Redis
REDIS_HOST=teable-cache
REDIS_PORT=6379
REDIS_DB=0

# App
PRISMA_DATABASE_URL=postgresql://${POSTGRES_USER}:${POSTGRES_PASSWORD}@${POSTGRES_HOST}:${POSTGRES_PORT}/${POSTGRES_DB}
BACKEND_CACHE_PROVIDER=redis
BACKEND_CACHE_REDIS_URI=redis://default:${REDIS_PASSWORD}@${REDIS_HOST}:${REDIS_PORT}/${REDIS_DB}
```

# 启动Teable
```
docker-compose pull

docker-compose up -d
```


