
#!/bin/bash

sudo apt-get -y update 

sudo apt-get -y install nginx

sudo apt-get upgrade nginx

sudo systemctl start nginx

sudo mkdir -p /var/www/dks.com/html

sudo chown -R $USER:$USER /var/www/dks.com/html	

sudo chmod -R 755 /var/www


cat <<'EOF' >/var/www/dks.com/html/index.html
    <head>
        <title>powerup cloud.com !</title>
    </head>
    <body>
        <h1>Success!  success welcome to powerup cloud technologies!</h1>
    </body>
</html> 
EOF

#!/bin/bash
my_ip=$(curl ipconfig.in/ip)
echo "$my_ip"

echo "server {
        listen 80;
        listen [::]:80;

        root /var/www/dks.com/html;
        index index.html index.htm index.nginx-debian.html;

        server_name $my_ip" >> /etc/nginx/sites-available/dks.com

#dos2unix /etc/nginx/sites-available/dks.com
sed 's/^M/;/' /etc/nginx/sites-available/dks.com > vinay && mv vinay /etc/nginx/sites-available/dks.com

sed 's/3/3\;/2' /etc/nginx/sites-available/dks.com > vinay && mv vinay /etc/nginx/sites-available/dks.com

cat << 'EOF' >>/etc/nginx/sites-available/dks.com
location / {
                try_files $uri $uri/ =404;
        }
}
EOF


sudo ln -s /etc/nginx/sites-available/dks.com /etc/nginx/sites-enabled/

sudo nginx -t

sudo systemctl restart nginx
