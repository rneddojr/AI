# AI

Download Raspberry Pi Imager

## Set Up Raspberry Pi 4 8GB
Download Ubuntu 22.04 LTS


## Set Up Raspberry Pi 4 4GB
Download Raspberry PI OS

### Step 1: Install Nginx
```
sudo apt-get update
sudo apt-get install nginx
```

### Step 2: Install MySQL
During the installation, you’ll be asked to set a root password (leaving the spot blank means you’ll have no password).  
```
sudo apt-get install mysql-server
```

In this stage, you’ll have the option to change your root password. Since you only just set it, you might as well say no. Say yes to everything else.  
```
sudo mysql_secure_installation
```

### Step 3: Install PHP
```
sudo apt install php-fpm php-mysql
```
Let’s edit a file. These are PHP’s settings, and we’re going to make it more secure.
```
sudo nano /etc/php//7.4/fpm/php.ini
```
Find the line that says #cgi.fix_pathinfo=1 and change it to cgi.fix_pathinfo=0. You can find it with the search function (Ctrl+W). Then exit with Ctrl+X and save with Y.

Then you’ll just restart PHP:
```
sudo systemctl restart php5-fpm
```

### Step 4: Configure nginx to use PHP
Let’s get these two to play nice together. Time to edit another file:
```
sudo nano /etc/nginx/sites-available/default
```
Edit the file to look like this
```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;

    index index.php index.html index.htm index.nginx-debian.html;

    server_name [your public IP];

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php5-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

Next to server_name, where I have [your public IP], plug in your public IP address (you can find this by asking Google or another search engine).  
Then we’ll just test this and re-load nginx.  
```
sudo nginx -t
sudo systemctl reload nginx
```

### Step 5: Set up port forwarding
```
Service Port: 80
Internal Port: 80
IP Address: [your Pi's IP address]
Protocol: TCP
Common Service Port: HTTP
```
Replace [your Pi’s IP address] with – you guessed it – your Pi’s IP address, which you can find with the terminal command hostname -I. Use 80 for your ports and RCP for your protocol. Your options may look slightly different from this, but it should be more or less recognizable.
