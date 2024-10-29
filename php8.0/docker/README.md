```bash
docker build -t nginx-php8-oci8:latest .
```

```bash
docker run -d \
  -p 8080:80 \
  -p 4430:443 \
  --restart=always \
  -v /var/www/html:/usr/share/nginx/html \
  --name nginx-php8-oci8 \
  --log-driver json-file \
  --log-opt max-size=15m \
  --log-opt max-file=5 \
  nginx-php8-oci8:latest
```
