---
title: nginx & certbot 紀錄
date: 2023-05-25 14:11:22
tags: [nginx, certbot]
---

之前在 appworks 的時候直接用了朋友推薦的[這個 docker image](https://github.com/JonasAlfredsson/docker-nginx-certbot)，直接把 nginx 跟 certbot 打包好給你，只要寫少少的 .conf 就可以了。不過這次在寫 CNL 的期末專案的時候，組員寫的 html 會掛在外面，要弄成 docker 有點麻煩（其實就是我懶得請人家弄），所以就直接在 EC2 上面弄 nginx + certbot 了，記錄一下免得之後忘記。

環境：AWS EC2 instance(Ubuntu 22.04)

## 安裝 nginx

```shell
sudo apt update
sudo wget http://nginx.org/keys/nginx_signing.key
sudo apt-key add nginx_signing.key
```

```shell
sudo apt update
sudo apt install nginx
sudo systemctl start nginx.service
```

到這裡就已經開好 nginx 服務了，可以用

```shell
sudo systemctl status nginx.service
```

確認有沒有在跑

## 安裝 certbot

```shell
sudo apt-get update -y
sudo apt-get install certbot python3-certbot-nginx -y
```

申請憑證

```shell
sudo certbot --nginx --email <你的email> --agree-tos -d <你的域名>
```

申請成功後，憑證會被放在`/etc/letsencrypt/live/<你的域名>`，certbot 也會幫你改好你的 nginx.conf 或者 nginx/sites-available/default，這時候連上去自己的域名應該就可以用 https 了
