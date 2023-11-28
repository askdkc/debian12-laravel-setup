# Laravel Dev Environment Set Up for Debian 12

## Install Basic Package
On Fresh Installed Debian, just run the following Setup Script
```bash
# PHP

sudo apt -y -V install wget curl php php8.2-fpm php8.2-cli php8.2-dev php8.2-pgsql \
php8.2-sqlite3 php8.2-gd php8.2-curl php8.2-imap php8.2-mysql \
php8.2-mbstring php8.2-xml php8.2-zip php8.2-bcmath php8.2-soap \
php8.2-intl php8.2-readline php8.2-gmp php8.2-redis php8.2-memcached \
php8.2-msgpack php8.2-igbinary

curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer

sudo apt install -y --force-yes libmagickwand-dev
echo "extension=imagick.so" > /etc/php/8.2/mods-available/imagick.ini
yes '' | apt install php8.2-imagick


# Nginx Setup
sudo apt install -y -V nginx

sudo systemctl enable nginx.service

# Node Setup

sudo apt update
sudo apt install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg

NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list

sudo apt update

sudo apt install -y -V nodejs

sudo npm install -g pm2
sudo npm install -g gulp
sudo npm install -g yarn
sudo npm install -g bun

# Laravel Valet
sudo apt install -y -V network-manager libnss3-tools jq xsel
composer global require laravel/installer
composer global require cpriego/valet-linux

echo PATH=$PATH:$HOME/.config/composer/vendor/bin >> ~/.bashrc
source ~/.bashrc

mkdir Sites
valet install
cd Sites
valet park
```

## Check If Laravel Valet Works
Now you can check if Laravel Valet works just by following.
```bash
cd ~/Sites
laravel new laravel
```
Open your browser and access to http://laravel.test/

## Additional Package: Install PGroonga
You can install PostgreSQL and PGroonga using the following Setup Script
```bash
cd /tmp
sudo apt install -y -V lsb-release
wget https://apache.jfrog.io/artifactory/arrow/$(lsb_release --id --short | tr 'A-Z' 'a-z')/apache-arrow-apt-source-latest-$(lsb_release --codename --short).deb
sudo apt install -y -V ./apache-arrow-apt-source-latest-$(lsb_release --codename --short).deb
wget https://packages.groonga.org/debian/groonga-apt-source-latest-$(lsb_release --codename --short).deb
sudo apt install -y -V ./groonga-apt-source-latest-$(lsb_release --codename --short).deb
sudo apt update

echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release --codename --short)-pgdg main" | sudo tee /etc/apt/sources.list.d/pgdg.list
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

sudo apt update
sudo apt install -y -V postgresql-16
sudo apt install -y -V postgresql-16-pgdg-pgroonga
```
