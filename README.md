```markdown
# Nginx Reverse Proxy Setup for Multiple Applications 🚀

This guide demonstrates how to set up **Nginx as a reverse proxy** to manage multiple applications running on different ports. By using Nginx, you can efficiently route requests to the appropriate application, simplifying access to various services.

## Prerequisites 📋
Ensure you have the following:
- A Linux server (tested with CentOS/RHEL)
- Nginx installed (installation steps below if needed)
- Firewall access to the designated ports

## Setup Instructions

### Step 1: Install Nginx
If Nginx is not already installed on your server, install it using the following command:

```bash
sudo dnf install nginx
```

### Step 2: Create an Nginx Configuration File
Set up a configuration file to handle your applications. Run:

```bash
sudo nano /etc/nginx/conf.d/myapps.conf
```

### Step 3: Add Configuration for Each Application
Add configuration blocks to `myapps.conf` for each application. Here’s an example for three applications running on ports `8081`, `8082`, and `8083`:

```nginx
server {
    listen 8081;
    server_name 192.158.22.201;

    location / {
        proxy_pass http://localhost/WebSite1/; # Adjust the internal URL as needed
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

server {
    listen 8082;
    server_name 192.158.22.201;

    location / {
        proxy_pass http://localhost/WebSite2/; # Adjust the internal URL as needed
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

server {
    listen 8083;
    server_name 192.158.22.201;

    location / {
        proxy_pass http://localhost/WebSite3/; # Adjust the internal URL as needed
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Step 4: Test and Restart Nginx
Verify that your Nginx configuration is correct:

```bash
sudo nginx -t
```

If there are no errors, restart Nginx to apply the new configuration:

```bash
sudo systemctl restart nginx
```

### Step 5: Configure Firewall
Ensure your firewall allows traffic on the specified ports. Run these commands to open ports `8081`, `8082`, and `8083`:

```bash
sudo firewall-cmd --zone=public --add-port=8081/tcp --permanent
sudo firewall-cmd --zone=public --add-port=8082/tcp --permanent
sudo firewall-cmd --zone=public --add-port=8083/tcp --permanent
sudo firewall-cmd --reload
```

## Accessing Your Applications 🌐
You can now access your applications via the following URLs:
- [http://192.158.22.201:8081](http://192.158.22.201:8081) - Routes to `/WebSite1/`
- [http://192.158.22.201:8082](http://192.158.22.201:8082) - Routes to `/WebSite2/`
- [http://192.158.22.201:8083](http://192.158.22.201:8083) - Routes to `/WebSite3/`

## Troubleshooting 🛠️
If you encounter issues:
1. Double-check your Nginx syntax with `sudo nginx -t`.
2. Confirm firewall rules with `sudo firewall-cmd --list-all`.
3. Review Nginx and system logs for any errors.

## Additional Resources 📚
- [Nginx Documentation](https://nginx.org/en/docs/)
- [Nginx Reverse Proxy Guide](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)

---

Feel free to contribute, open issues, or submit pull requests to improve this guide! Happy configuring! 🎉
```