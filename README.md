# Steps to Configure Nginx as Load Balancer

> Click ⭐ if you like the project. Pull Request are highly appreciated. Follow me (https://x.com/mahesh_lorenz) for technical updates.


---
Configuring Nginx as a load balancer primarily involves defining an "upstream" group of backend servers and then directing incoming requests to that group using a "proxy_pass" directive within a server block.



Open the Nginx Configuration File:
Access the main Nginx configuration file 
(e.g., nginx.conf or a site-specific configuration file within sites-available).
    
1. Define the Upstream Group:
    
    Create an upstream block to define your backend servers. This block lists the IP addresses and ports of the servers that will handle the requests.

CODE

    upstream backend_servers {
        server 192.168.1.10:8080; # Backend server 1
        server 192.168.1.11:8080; # Backend server 2
        # Add more servers as needed
    }

2. Configure the Server Block: Within the http block, define a server block that listens for incoming requests and uses the proxy_pass directive to forward them to the upstream group.

CODE

    server {
        listen 80; # Nginx listens on port 80 for incoming requests
        server_name your_domain.com; # Replace with your domain or IP

        location / {
            proxy_pass http://backend_servers; # Directs traffic to the upstream group
            # Optional proxy settings for headers, timeouts, etc.
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }


3. Choose a Load Balancing Method (Optional): Nginx uses the Round Robin method by default. Other methods like least_conn (least active connections), ip_hash (sticky sessions based on client IP), or hash (consistent hashing) can be specified within the upstream block.

CODE

    upstream backend_servers {
        least_conn; # Example: Use least_conn method
        server 192.168.1.10:8080;
        server 192.168.1.11:8080;
    }

4. Add Health Checks (Optional): Configure max_fails and fail_timeout parameters within the upstream block to define health checks for backend servers.

CODE

    upstream backend_servers {
        server 192.168.1.10:8080 max_fails=3 fail_timeout=30s;
        server 192.168.1.11:8080;
    }


5. Test and Restart Nginx: After making changes, test the configuration for syntax errors and then reload or restart Nginx to apply the changes.

Code

    sudo nginx -t
    sudo systemctl reload nginx

[⬆ Back to Top](#steps-to-configure-nginx-as-load-balancer)