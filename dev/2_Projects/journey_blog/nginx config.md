
This can be used to set up the certs manually and house them for development use. Always an option but I will probably just use acme. 


```
openssl req -subj '/CN=aruljohn.com/O=Arul John/C=US' -new -newkey rsa:2048 -sha256 -days 365 -nodes -x509 -keyout server.key -out server.crt
```

```
server {
    listen      443 ssl;
    server_name blog.f0nzy.com;
    root   html;

    ssl on;
    ssl_certificate     /etc/nginx/ssl/server.crt;
    ssl_certificate_key /etc/nginx/ssl/server.key;
    ssl_protocols TLSv1.3;
 }
```


Another way to generate a self signed cert. 
```
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 10000
```