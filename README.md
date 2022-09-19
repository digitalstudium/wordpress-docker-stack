# wordpress-docker-stack
Here it is described how to install wordpress via docker
### Step 1: Install docker
```
sudo apt install docker.io -y
sudo usermod -aG docker $USER ; newgrp docker
docker swarm init
```
### Step 2: Deploy wordpress stack
```
docker stack deploy -c stack.yml wp
```
### Step 3: Install nginx and certbot
```
sudo apt install nginx-light certbot python3-certbot-nginx -y
```
### Step 4: Create nginx config
Create  `/etc/nginx/conf.d/example.com.conf` file with such content:
```
server {
  server_name example.com www.example.com;
  access_log /var/log/nginx/example.com-access.log;
  error_log /var/log/nginx/example.com-error.log;
  client_max_body_size 0;
  location / {
    proxy_pass http://127.0.0.1:8080;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
}
```
### Step 5: Create ssl cert
```
sudo certbot --nginx
```

That's it!
