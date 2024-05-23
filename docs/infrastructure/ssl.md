# SSL Certificates

我们使用 Let's Encrypt 来签发通配符证书
```
certbot certonly  -d "*.mirror.nyist.edu.cn" --manual --preferred-challenges dns-01  --server https://acme-v02.api.letsencrypt.org/directory
```

等待签发完成后 证书位于 **/etc/letsencrypt/live/domain-name/**

**nginx** 配置参考
```shell title="/etc/nginx/conf.d/test.conf"
        ssl_certificate    /home/NYISTSSLCERT/fullchain.pem;
        ssl_certificate_key    /home/NYISTSSLCERT/privkey.pem;
        ssl_trusted_certificate /home/NYISTSSLCERT/chain.pem;
        ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3 SSLv2 SSLv3;
        ssl_ciphers  ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP;
        ssl_prefer_server_ciphers on;
        ssl_session_cache shared:SSL:10m;
        ssl_session_timeout 10m;
```
