```bash
sudo apt update
sudo apt install -y nginx mysql-server php8.0-fpm php8.0-cli php8.0-mbstring php8.0-xml php8.0-curl php8.0-zip php8.0-bcmath php8.0-mysql unzip curl git composer
```

```bash
sudo mkdir -p /var/www
sudo chown -R $USER:$USER /var/www
cd /var/www
git clone <YOUR_REPOSITORY_URL> esellbook-deploy-fix
cd esellbook-deploy-fix
cp .env.example .env
composer install --no-dev --prefer-dist --optimize-autoloader
php artisan key:generate --force
php artisan config:cache
php artisan route:cache
php artisan view:cache
php artisan migrate --force
php artisan storage:link
```

```bash
sudo chown -R www-data:www-data /var/www/esellbook-deploy-fix
sudo find /var/www/esellbook-deploy-fix -type f -exec chmod 644 {} \;
sudo find /var/www/esellbook-deploy-fix -type d -exec chmod 755 {} \;
sudo chmod -R ug+rwx /var/www/esellbook-deploy-fix/storage /var/www/esellbook-deploy-fix/bootstrap/cache
```

```bash
sudo cp /var/www/esellbook-deploy-fix/deploy/nginx.esellbook.conf /etc/nginx/sites-available/esellbook
sudo ln -s /etc/nginx/sites-available/esellbook /etc/nginx/sites-enabled/esellbook
sudo rm -f /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl restart php8.0-fpm
sudo systemctl restart nginx
```
