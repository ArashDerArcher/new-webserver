# New webserver

Scripts and config files to quickly start a new webserver that has:

- ufw
- letsenscrypt ssl cert
- Diffie-Hellman parameters
- nginx with ssl properly configured

Assumes you are logged in as root.

# UFW

    ufw default deny incoming
    ufw default allow outgoing
    ufw allow 22  # ssh
    ufw allow 443 # https
    ufw allow 80  # http
    # allow port from specific IP
    # ufw allow from 1.1.1.1 to any port 22
    # allow port from specific interface
    # ufw allow in on eth0 to any port 80
    ufw enable

# Letsencrypt

    mkdir /lego
    cd /lego
    wget https://github.com/go-acme/lego/releases/download/v2.5.0/lego_v2.5.0_linux_amd64.tar.gz
    tar -xf lego_v2.5.0_linux_amd64.tar.gz
    rm lego_v2.5.0_linux_amd64.tar.gz CHANGELOG.md LICENSE
    ./lego --email="EMAIL" --domains="DOMAIN" -a run
    # OR if nginx is already running:
    # ./lego --email="EMAIL" --domains="DOMAIN" -a run --webroot="/var/path-to-site"

# Nginx

    apt-get install nginx # todo: nginx official repo
    openssl dhparam -out /etc/nginx/dhparam.pem 2048 # Diffie-Hellman parameters
    cd /etc/nginx/conf.d
    wget https://raw.githubusercontent.com/askmike/new-webserver/master/site.conf
    # edit nginx conf with your site and api
    service nginx configtest
    service nginx restart
    
# nodejs

    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
    # follow instructions in terminal
    nvm install 11.6

# Build tools

    apt-get install python python3 make build-essential