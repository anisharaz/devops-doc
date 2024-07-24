# Nginx
Nginx is a webserver which can serve static files and act as reverse proxy to other services.It is also very easy to install & configure.
## Installation

### APT
```bash
sudo apt intall nginx
```

### Pacman
```bash
sudo pacman -S nginx
```
### APK
```bash
sudo apk add nginx
```

## Configuring as Reverse Proxy
> config file location: `/etc/nginx/nginx.conf` <br>
you can add a line to the config file inside http block(if it doesn't exist) to include the config file. <br>
```
http{
    ....
    include /etc/nginx/conf.d/*.conf;
    ....
}
```
create a folder `conf.d` at `/etc/nginx/` and add your config files there.

you can name you config files for each service for Eg. <br>
`youtube.nginx.conf` for youtube service. <br>
`github.nginx.conf` for github service.

Inside these files add a server block for each service. <br> For Eg. Your app container is running on ip address 11.0.0.3:8000 and you want to serve it on `app.example.com` then replace the {domain.name} with app.example.com, {internal_IP} as 11.0.0.3 and {PORT} as 8000.

## The generalized and universal server block inside http block of nginx config, Just edit the fields inside {__}

```yaml
server {
    listen 80;
    server_name {domain.name};
    location / {
        add_header X-Served-By $host;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-Scheme $scheme;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Real-IP $remote_addr; 
        proxy_pass http://{internal_IP}:{PORT}$request_uri;
        access_log /var/log/nginx/{service_name}.access.log;
    }
}
```

> [!IMPORTANT]
> Add this file as a `default.nginx.conf` in `conf.d` folder. This will restrict the direct access to web server thought ip address and return 403.

```yaml
server {
    listen 80 default_server;
    location / {
        access_log /var/log/nginx/default.access.log;
    }
    return 403;
}
```
restart the nginx service after adding the config files.
```bash
sudo nginx -s reload
```

### After these steps you can simply point your domain to you server public IP address and it will be served by nginx.

