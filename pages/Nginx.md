- ## How-To
	- ### Create and enable Nginx SSL Certificate
		- #### Create Nginx SSL Certificate
			- ```
			  openssl req -new -x509 -days 9999 -nodes \
			          -newkey rsa:2048 -out kdash.smartnation.kz.pem \
			          -keyout kdash.smartnation.kz.key
			  ```
		- #### Sample nginx.conf with enabled SSL
			- ```
			  worker_processes  1;
			  
			  events {
			      worker_connections  1024;
			  }
			  
			  http {
			      server {
			          listen 80;
			          server_name localhost;
			          rewrite ^ https://$http_host$request_uri? permanent;
			  
			          server_tokens off;
			      }
			  
			      server {
			          listen 443 ssl;
			          ssl_certificate /etc/ssl/aifc.pem;        # path to your cacert.pem
			          ssl_certificate_key /etc/ssl/aifc.key;    # path to your privkey.pem
			          server_name localhost;
			          server_tokens off;
			          proxy_read_timeout  1200s;
			  
			          root   /usr/share/nginx/html;
			          index  index.html index.htm;
			          include /etc/nginx/mime.types;
			  
			          gzip on;
			          gzip_min_length 1000;
			          gzip_proxied expired no-cache no-store private auth;
			          gzip_types text/plain text/css application/json application/javascript application/x-javascript text/xml application/xml application/xml+rss text/javascript;
			  
			          location ~* /api {
			              proxy_set_header   Host $host;
			              proxy_set_header   X-Real-IP $remote_addr;
			              proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
			              proxy_set_header   X-Forwarded-Host $server_name;
			              proxy_set_header   X-Forwarded-Proto https;
			              proxy_pass http://kong:8000;
			          }
			  
			          location ~* /webapp {
			              proxy_set_header   Host $host;
			              proxy_set_header   X-Real-IP $remote_addr;
			              proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
			              proxy_set_header   X-Forwarded-Host $server_name;
			              proxy_set_header   X-Forwarded-Proto https;
			              proxy_pass http://webapp:80;
			          }
			      }
			  }
			  ```