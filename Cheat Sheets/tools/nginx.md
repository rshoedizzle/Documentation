config file location: `/etc/nginx/sites-available`

### Step 1: Install Nginx

First, you need to have Nginx installed on your server. The installation process varies depending on your operating system. Here's how you can install Nginx on Ubuntu as an example:

bashCopy code

`sudo apt update sudo apt install nginx`

After installation, you can check if Nginx is running with:

bashCopy code

`sudo systemctl status nginx`

### Step 2: Configure Nginx for Each Game

You will create a separate server block (also known as a virtual host) for each game. Each server block will listen on port 80 (HTTP) or 443 (HTTPS if you have an SSL certificate) and will proxy the requests to the appropriate game server IP and port.

1. **Create Configuration Files**: For better organization, create a separate configuration file for each game server under `/etc/nginx/sites-available/`. For example, for `game1.domain.com`, create a file named `game1.domain.com`:

bashCopy code

`sudo nano /etc/nginx/sites-available/game1.domain.com`

2. **Edit the Configuration File**: Add the following server block, adjusting the `server_name`, `proxy_pass`, and other directives as needed for your setup:

nginxCopy code

`server {     listen 80; # Listen on port 80 for HTTP, use 443 for HTTPS     server_name game1.domain.com;      location / {         proxy_pass http://192.168.86.10:25565; # The IP and port of your game server         proxy_set_header Host $host;         proxy_set_header X-Real-IP $remote_addr;         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;     } }`

3. **Enable the Configuration**: Link the configuration file from `sites-available` to `sites-enabled` to enable it:

bashCopy code

`sudo ln -s /etc/nginx/sites-available/game1.domain.com /etc/nginx/sites-enabled/`

Repeat these steps for each game by creating a separate configuration file for each subdomain and adjusting the `server_name` and `proxy_pass` accordingly.

### Step 3: Test Nginx Configuration

After setting up your configurations, it’s important to test the Nginx configuration for syntax errors:

bashCopy code

`sudo nginx -t`

If the test is successful, you’ll see a message that says the syntax is okay and the test is successful.

### Step 4: Reload Nginx

Finally, apply the changes by reloading Nginx:

bashCopy code

`sudo systemctl reload nginx`

### Step 5: Configure DNS

Make sure your DNS records are set up to point each subdomain (`game1.domain.com`, `game2.domain.com`, etc.) to the IP address of the server where Nginx is running. This usually involves adding A records through your domain registrar’s DNS management panel, as mentioned previously.

### Additional Considerations

- **HTTPS and SSL**: To secure your subdomains, consider setting up SSL certificates with Let’s Encrypt and adjusting your Nginx configurations to use HTTPS (port 443) instead of HTTP.
- **Firewall Settings**: Ensure your server’s firewall settings allow traffic on the necessary ports (80 and 443 for web traffic).
- **Nginx Optimization**: Depending on your server's traffic, you may need to optimize Nginx settings for performance, such as adjusting worker processes, connections, and buffers.

By following these steps, you'll have a reverse proxy setup with Nginx that directs traffic to your game servers based on subdomains, making it easier for players to connect to your games.