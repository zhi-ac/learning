
mkdir -p /mydata/redis/conf
touch /mydata/redis/conf/redis.conf

docker pull redis

docker run -p 6379:6379 --name redis \
-v /mydata/redis/data:/data \
-v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis redis-server /etc/redis/redis.conf

# 直接进去redis客户端。
docker exec -it redis redis-cli

vim /mydata/redis/conf/redis.conf
# 插入下面内容
appendonly yes       #此时是持久化的
保存

docker restart redis

# 设置redis容器在docker启动的时候启动
docker update redis --restart=always
