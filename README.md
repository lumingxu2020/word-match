# QNAP NAS 部署

当前实例部署在：

- NAS：QNAP TS-251D
- 目录：`/share/CACHEDEV1_DATA/Container/word-match`
- 容器：`word-match`
- 镜像：`nginx:1.27-alpine`
- 端口：`3093 -> 80`
- 局域网地址：`http://192.168.50.251:3093/`

更新 `dist/index.html` 后，将文件同步到 NAS 的 `word-match/www/index.html`。Nginx 使用只读挂载，通常不需要重启容器；浏览器刷新即可加载新版本。

如需重建容器：

```sh
cd /share/CACHEDEV1_DATA/Container/word-match
mkdir -p /tmp/word-match-docker-config
/share/CACHEDEV1_DATA/.qpkg/container-station/bin/docker \
  --config /tmp/word-match-docker-config compose -f compose.yml up -d --force-recreate
```

检查状态：

```sh
/share/CACHEDEV1_DATA/.qpkg/container-station/bin/docker \
  inspect --format '{{.State.Status}} {{.State.Health.Status}}' word-match
curl -fsSI http://127.0.0.1:3093/
```

公网访问需要在路由器上转发 TCP `3093`，或者在 QNAP 反向代理中把 HTTPS 域名转发到 `127.0.0.1:3093`。
