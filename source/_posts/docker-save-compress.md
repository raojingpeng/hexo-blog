---
title: docker save 时压缩镜像体积
author: Ackerman
date: 2021-12-09 23:19:59
categories:
- [Learning, Docker]
tags:
- docker
---

docker save 命令将指定镜像打包成 tar 归档文件。如果镜像比较大的话，打出的包体积也会很大。为了便于网络传输，我们可以通过以下命令压缩打包文件体积

```shell
# for not running docker, use save:
docker save <dockernameortag> | gzip > mycontainer.tgz

# for running or paused docker, use export:
docker export <dockernameortag> | gzip > mycontainer.tgz

# load
# for save
docker load -i mycontainer.tgz
# for export
docker import mycontainer.tgz dockername:dockertag
```

