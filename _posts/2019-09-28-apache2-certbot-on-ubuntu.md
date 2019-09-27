---
layout: post

title: apache2 and certbot on Ubuntu

author: Meir Gabay

publication_date: 2019-Sep-28
---

# Apache

## Apache service

### Start, stop and restart

```bash
sudo service apache2 start
```

```bash
sudo service apache2 stop
```

```bash
sudo service apache2 restart
```

## Log files

Default location

- `/var/log/apache2/access.log`
- `/var/log/apache2/error.log`

### Main config file

Main configuration file which defined which _.conf and _.load files will be used

`/var/apache2/apache.conf`

```
<Directory /var/www/>
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
</Directory>
```

### Config files

Configuration files path:

`/var/apache2/sites-enabled/000-default.conf`

```
<VirtualHost \*:80>
  ServerName meirg.co.il
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/html
  ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

  # Redirect all HTTP to HTTPS, only if not HTTPS already

  RewriteEngine on
  RewriteCond %{HTTPS} !=on
  RewriteRule ^/?(.\*) https://%{SERVER_NAME}/$1 [R=301,L]
</VirtualHost>
```

`/var/apache2/sites-enabled/000-default-le-ssl.conf`

```
<VirtualHost *:443>
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/html
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined

  ServerName meirg.co.il
  Include /etc/letsencrypt/options-ssl-apache.conf
  ServerAlias www.meirg.co.il
  ServerAlias learn.meirg.co.il
  ServerAlias blog.meirg.co.il
  SSLCertificateFile /../../../fullchain.pem
  SSLCertificateKeyFile /../../../privkey.pem

  RewriteEngine on

  # from www.meirg to learn.meirg.co.il/moodle/
  RewriteCond "%{HTTP_HOST}" "^(www\.)?meirg\.co\.il\/?$" [NC]
  RewriteRule ".*" "https://learn.meirg.co.il/moodle/" [R=301,L]

  # redirect  subdomain blog to github pages
  RewriteCond "%{HTTP_HOST}" "^blog\.meirg\.co\.il$"
  RewriteRule "^" "https://unfor19.github.io" [R=301,L]
</VirtualHost>
</IfModule>
```

# Certbot

## Update existing certificate

### Add more domains/subdomains

```bash
sudo certbot --expand -d existing.com -d example.com -d newdomain.com
```

[effbot docs](https://certbot.eff.org/docs/using.html#re-creating-and-updating-existing-certificates)

### Automatic renewal

Automatically renew certificate twice a day

```bash
sudo crontab -e
```

Add: `0 */12 * * * certbot renew`

```bash
sudo crontab -l
```
