upstream node_backend {
#    ip_hash;
    server 127.0.0.1:8080;
    server 127.0.0.1:8081;
}
server {
        listen 80 default_server;
        listen [::]:80 default_server;
        
        root /home/harshit/Desktop/node-mongodb-todo/assets;

        # Add index.php to the list if you are using PHP
        #index index.html index.htm index.nginx-debian.html;

        server_name localhost;
        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 

                try_files $uri $uri/ =404;
        }
        location /api{
                proxy_pass http://node_backend;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        

        }
    }
