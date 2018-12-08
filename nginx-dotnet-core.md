# Настройки nginx для проекта .NET Core с включенным https

Ссылка, на которую я ориентировался: <https://www.digitalocean.com/community/tutorials/how-to-configure-nginx-with-ssl-as-a-reverse-proxy-for-jenkins>

### Install Nginx

Update your package lists and install Nginx:

```bash
sudo apt-get update
sudo apt-get install nginx
```

It's not crucial, but you may want to check Nginx's version in case you need to do any troubleshooting down the road. Newer versions of Nginx provide a few more features as well.

```bash
nginx -v
```

### Get a Certificate

Next, you will need to purchase or create an SSL certificate. These commands are for a self-signed certificate, but you should get an officially signed certificate if you want to avoid browser warnings.

Move into the proper directory and generate a certificate:

```bash
cd /etc/nginx
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/cert.key -out /etc/nginx/cert.crt
```

You will be prompted to enter some information about the certificate. You can fill this out however you'd like; just be aware the information will be visible in the certificate properties. We've set the number of bits to 2048 since that's the minimum needed to get it signed by a CA. If you want to get the certificate signed, you will need to create a CSR.

### Edit the Configuration

Next you will need to edit the default Nginx configuration file.

```bash
sudo nano /etc/nginx/sites-enabled/default
```

Here is what the final config might look like \[...\].

```config
server {
    listen 80;
    listen [::]:80;
    server_name dmalikov.rdvb.ru;
    return 301 https://$host$request_uri;
}

server {

    listen 443 ssl;
    server_name dmalikov.rdvb.ru;

    ssl_certificate           /etc/nginx/cert.crt;
    ssl_certificate_key       /etc/nginx/cert.key;

    ssl on;
    ssl_session_cache  builtin:1000  shared:SSL:10m;
    ssl_protocols  TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers HIGH:!aNULL:!eNULL:!EXPORT:!CAMELLIA:!DES:!MD5:!PSK:!RC4;
    ssl_prefer_server_ciphers on;

    location / {

        proxy_set_header        Host $host;
        proxy_set_header        X-Real-IP $remote_addr;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header        X-Forwarded-Proto $scheme;

        # Fix the “It appears that your reverse proxy set up is broken" error.
        proxy_pass          https://localhost:5001;
        proxy_read_timeout  90;

        proxy_redirect      https://localhost:5001 https://dmalikov.rdvb.ru;
    }
}
```

После установки сертификата от LE конфиг будет автоматически изменен

## Установка сертификата Let's Encrypt на сервер

Ссылка на мануал: <https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx>

### Install

On Ubuntu systems, the Certbot team maintains a PPA. You can add it to your list of repositories and install Certbot by running the following commands.

```bash
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx 
```

Certbot's DNS plugins are also available for your system which can be used to automate obtaining a wildcard certificate from Let's Encrypt's ACMEv2 server. To use one of these plugins, you must have configured DNS for the domain you want to obtain a certificate for with a DNS provider that Certbot has a plugin for. A list of these plugins and more information about using them can be found here. To install one of these plugins, run the installation command above but replace python-certbot-nginx with the name of the DNS plugin you want to install.

### Get Started

Certbot has an Nginx plugin, which is supported on many platforms, and automates certificate installation.

```bash
sudo certbot --nginx
```

Running this command will get a certificate for you and have Certbot edit your Nginx configuration automatically to serve it. If you're feeling more conservative and would like to make the changes to your Nginx configuration by hand, you can use the certonly subcommand:

```bash
sudo certbot --nginx certonly
```

If you want to obtain a wildcard certificate using Let's Encrypt's new ACMEv2 server, you'll also need to use one of Certbot's DNS plugins. To do this, make sure the plugin for your DNS provider is installed using the instructions above and run a command like the following:

```bash
sudo certbot -a dns-plugin -i nginx -d "*.example.com" -d example.com --server https://acme-v02.api.letsencrypt.org/directory
```

You'll need to replace dns-plugin with the name of the DNS plugin you want to use. You may also need to provide additional flags such as the path your API credentials as described in the documentation for the DNS plugin linked above.

To learn more about how to use Certbot read our documentation.

### Automating renewal

The Certbot packages on your system come with a cron job that will renew your certificates automatically before they expire. Since Let's Encrypt certificates last for 90 days, it's highly advisable to take advantage of this feature. You can test automatic renewal for your certificates by running this command:

```bash
sudo certbot renew --dry-run
```

More detailed information and options about renewal can be found in the full documentation.