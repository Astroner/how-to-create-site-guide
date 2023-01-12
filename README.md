# How to create web-app from scratch to production
Here I describe my experience in building web applications.

## Table of content

## Steps
 - Write an app. It can consist of a single service or several ones, backend and frontend for example.
 - Buy a VPS with public ip or just obtain public ip.
 - Deploy an app. To ensure that everything is OK you can simply make some requests to the public ip + port.
 - Buy a domain. I use REG.ru to buy and manage them.
 - Create subdomain for each peace of the app e.g. frontend.domain.com and backend.domain.com
 - Assign your public ip to each domain. Unfortunatly you cant assign port to it, so we have to manage domain routing on the server.
 - To unsure that everything is OK at this step you can make some requests to frontend.domain.com:PORT. 
 At this point we don't handle domain routing so you just make requests to the server port.
 - Install nginx and start it. By default, it will listen port 80 and will send simple text, 
 so to ensure that you setted it up correctly just open your public ip in browser.
 - Configure nginx as a reverse proxy. Example configuration:
 ```nginx
 events {}
 
 http {
  server {
    server_name 'frontend.domain.com' 'www.frontend.domain.com';
    location / {
      proxy_pass 'http://localhost:FRONTEND_PORT';
    }
  }
  server {
    server_name 'backend.domain.com' 'www.backend.domain.com';
    location / {
      proxy_pass 'http://localhost:BACKEND_PORT';
    }
  }
 }
 ```
 Here we are setting up 2 virtual hosts for frontend and backend with corresponding server_name-s and proxy_pass-es (Do not forget to restart nginx). 
 Now you can make requests to your domains and check that everything is fine.
 - Configure nginx SSL/HTTPs. 
 [Get more info here](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04).
   - Install certbot and python3-certbot-nginx. This package will do most of the work.
   - run `certbot --nginx -d example.com -d www.example.com` for each service. For example:
     - `certbot --nginx -d frontend.domain.com -d www.frontend.domain.com`
     - `certbot --nginx -d backend.domain.com -d www.backend.domain.com`
 - That's all
We started with nothing and finished with fully operationg web-app.
