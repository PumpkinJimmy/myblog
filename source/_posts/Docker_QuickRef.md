# Docker Quick Reference
```bash
# 如果没有适当配置，以下命令都要sudo
docker create # 创建容器
docker run # 使用给定镜像创建容器来执行给定命令
docker start # 启动容器
docker stop # 停止容器
docker rm # 删除容器
docker ps # 查看当前运行的容器
docker ps -a # 查看所有容器
docker container inspect <container> # 查看容器详细信息
/etc/docker/daemon.json # Docker默认配置文件

# Docker服务管理
sudo systemctl start docker
sudo systemctl restart docker 
```