# phil-nginx

## 打开免费SSL

[免费SSL](https://freessl.cn/)

## 输入域名

```Shell
fage.org.cn	
```

## 点击详情

```Shell
### 复制类似以下的命令到服务器上执行
acme.sh --issue -d fage.org.cn  --dns dns_dp --server https://acme.freessl.cn/v2/DV90/directory/fylivnjak8w00rbaxbyn

```

## 拷贝文件

```Shell
### 将以下两个文件拷贝到 nginx 的conf 目录
cp /root/.acme.sh/fage.org.cn/fage.org.cn.cer  /home/zfw/docker/run_space/nginx/conf/
cp /root/.acme.sh/fage.org.cn/fage.org.cn.key /home/zfw/docker/run_space/nginx/conf/
```

## 修改Nginx配置 {id="nginx_1"}

```Shell
server {
        listen       443 ssl;
        server_name  fage.org.cn;

        ssl_certificate      fage.org.cn.cer;
        ssl_certificate_key  fage.org.cn.key;
	#location /tj/tj.min.js {
        #    proxy_pass http://10.20.53.96/;
        #}

        location / {
	    root  /usr/share/nginx/html;
            try_files $uri $uri/ /index.html;
        }

	error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /usr/share/nginx/html;
        }

   }

```

## 重启Nginx

```Shell
## reload 也行

## 已打开的浏览器窗口需要重启

```

## 格式转换

```Shell
### 文本格式的pem转二进制格式的crt和key

openssl x509 -in fullchain.pem -out fullchain.crt

openssl rsa -in privkey.pem -out privkey.key
```


