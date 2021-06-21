# Docker - Redis

### [docker 安装部署 redis（配置文件启动）](https://segmentfault.com/a/1190000014091287)



> ```
> # 创建并运行一个名为 myredis 的容器
> docker run \
> -p 16379:6379 \
> -v $PWD/data:/data \
> -v $PWD/conf/redis.conf:/etc/redis/redis.conf \
> --privileged=true \
> --name myredis \
> -d redis redis-server /etc/redis/redis.conf
> ```

```
sed -i 's/logfile ""/logfile "access.log"/' conf/redis.conf
sed -i 's/# requirepass foobared/requirepass 123456/' conf/redis.conf
sed -i 's/appendonly no/appendonly yes/' conf/redis.conf

还有取消protected-mode
注释掉bind 127.0.0.1 ->必须注释，否则无法通过GUI以及Python连接
```

`docker exec -it myredis bash`



