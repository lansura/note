### gitlab runner 添加

手动添加runner有三个步骤
1. 安装gitlab runner
2. 指定code所在url
3. 设置注册token

###### 安装并启动docker
`$ sudo docker pull gitlab/gitlab-runner:latest`

`$ sudo docker run --network host -d --name gitlab-runner --restart always   -v /srv/gitlab-runner/config:/etc/gitlab-runner   -v /var/run/docker.sock:/var/run/docker.sock   gitlab/gitlab-runner:latest`


###### 查看docker进程号
`$ sudo docker ps`

```
CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS              PORTS               NAMES
e691977972ba        gitlab/gitlab-runner:latest   "/usr/bin/dumb-init …"   2 months ago        Up 2 months                             gitlab-runner
```

###### 开始注册
`$ sudo docker exec -ti 7938ab4a2c7e gitlab-runner register`

添加url

添加token






