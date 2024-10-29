```markdown
# Nginx Reverse Proxy Setup for Multiple Applications 🚀

This guide walks you through using Nginx as a reverse proxy to manage multiple applications on different ports. By setting up Nginx in this way, you can route incoming requests to the appropriate application based on the port.

## 🔧 Prerequisites
- Ensure you have Nginx installed on your server.
- Root or sudo access to configure Nginx and firewall settings.

## 📁 Step-by-Step Setup with Nginx

### 1. Install Nginx (if you haven't already)
```bash
sudo dnf install nginx
```

### 2. Create Nginx Configuration File
Create or edit a configuration file for your applications:
```bash
sudo nano /etc/nginx/conf.d/myapps.conf
```

### 3. Add Configuration for Each Application
Add the following configuration to handle requests for each application:

```nginx
server {
    listen 8081;
    server_name 192.168.100.150;

    location / {
        proxy_pass http://localhost/SendMail/;  # Adjust the internal URL as needed
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

server {
    listen 8082;
    server_name 192.168.100.150;

    location / {
        proxy_pass http://localhost/bkashSendMail/;  # Adjust the internal URL as needed
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

server {
    listen 8083;
    server_name 192.168.100.150;

    location / {
        proxy_pass http://localhost/Mail/;  # Adjust the internal URL as needed
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### 4. Test and Restart Nginx
After adding your configurations, test the Nginx configuration for errors:
```bash
sudo nginx -t
```

If the test passes, restart Nginx to apply the changes:
```bash
sudo systemctl restart nginx
```

### 5. Configure Firewall for Port Access
Make sure your firewall allows traffic on the specified ports:
```bash
sudo firewall-cmd --zone=public --add-port=8081/tcp --permanent
sudo firewall-cmd --zone=public --add-port=8082/tcp --permanent
sudo firewall-cmd --zone=public --add-port=8083/tcp --permanent
sudo firewall-cmd --reload
```

## 🌐 Accessing Your Applications
Once configured, you can access your applications via the following URLs:
- [http://192.168.100.150:8081](http://192.168.100.150:8081) ➡️ Routes to the `/SendMail/` application.
- [http://192.168.100.150:8082](http://192.168.100.150:8082) ➡️ Routes to the `/bkashSendMail/` application.
- [http://192.168.100.150:8083](http://192.168.100.150:8083) ➡️ Routes to the `/Mail/` application.

---

With this setup, Nginx acts as a reverse proxy, effectively routing incoming traffic to the appropriate application based on the designated port. 🎉

Happy configuring!
```