# Securing NGINX: A Comprehensive Guide

This guide covers essential steps to secure your NGINX web server. By implementing these configurations, you can enhance the security of your server, protect your applications, and control access effectively.

## Table of Contents

1. [Setting Secure Headers](#setting-secure-headers)
2. [Implementing Basic Authentication](#implementing-basic-authentication)
3. [Using Allow/Deny Directives](#using-allowdeny-directives)
4. [Additional Security Measures](#additional-security-measures)
5. [Conclusion](#conclusion)

## Setting Secure Headers

Setting secure headers can help protect your web server from various attacks, including XSS, clickjacking, and MIME sniffing.

1. **Edit the NGINX configuration file:**
   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```
2. **Add the following headers in the http block or specific server block:**
   ```bash
   http {
    ...
    add_header X-Content-Type-Options "nosniff";
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header Content-Security-Policy "default-src 'self'";
    server_token off;
    ...
   }
   ```
3. **Save the file and reload NGINX:**
   ```bash
   sudo systemctl reload nginx
   ```
## Implementing Basic Authentication
Basic authentication adds a layer of protection by requiring users to enter a username and password to access the web server.

#### Install the Apache utility tool:

   ```bash
   sudo apt-get install apache2-utils
   ```
Create a password file and add a user:
   ```bash
   sudo htpasswd -c /etc/nginx/.htpasswd username
   ```
Edit your NGINX configuration file to include the authentication settings:
```nginx
   server {
       ...
       location / {
           auth_basic "Restricted Content";
           auth_basic_user_file /etc/nginx/.htpasswd;
           ...
   }
       ...
   }
   ```
Save the file and reload NGINX:
   ```bash
   sudo systemctl reload nginx
   ```
## Using Allow/Deny Directives

Control access to your server or specific resources by using the allow and deny directives.

Edit your NGINX configuration file:

   ```bash
sudo nano /etc/nginx/nginx.conf
   ```
Configure allow/deny directives in the desired server or location block:

   ```nginx
   server {
       ...
       location /admin {
           allow 192.168.1.0/24;
           deny all;
       ...
    }
    ...
    }
   ```
Save the file and reload NGINX:
   ```bash
   sudo systemctl reload nginx
   ```
## Hiding Sensitive Files and Directories

Hide sensitive files and directories to prevent unauthorized access to critical resources.

Edit your NGINX configuration file:

   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```
Add location blocks to hide specific files and directories:

   ```nginx
   server {
       ...
       location ~ /\.(env|htaccess|git) {
           deny all;
           ...
       }
       ...
   }
   ```
Save the file and reload NGINX:
   ```bash
   sudo systemctl reload nginx
   ```
## Additional Security Measures
Disable unnecessary modules: Ensure only essential modules are enabled.
Limit request size: Prevent large payloads to avoid buffer overflow attacks.
   ```nginx
   http {
       ...
       client_max_body_size 1M;
       limit_req_zone $binary_remote_addr zone=mylimit:10m rate=10r/s;
       ...
   }

   server {
       ...
       location /login {
           limit_req zone=mylimit burst=5;
           ...
       }
       ...
   }
   ```
Conclusion
Securing your NGINX server is crucial for protecting your web applications and data. By following this guide, you can implement effective security measures, including setting secure headers, enabling basic authentication, using allow/deny directives to control access, and hiding sensitive files and directories.

For more detailed configurations and advanced security options, refer to the NGINX documentation.
