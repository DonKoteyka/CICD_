# Create server
``` bash
ssh root@79.174.92.142
```

## create user
``` bash
sudo adduser kot
```
``` bash
usermod kot -aG sudo
```
``` bash
su kot
```
``` bash
cd ~

```

## donwload app
``` bash
sudo apt update
```
``` bash
python3 --version
git --version
```
``` bash
sudo apt install python3-venv python3-pip postgresql nginx expect
sudo systemctl status postgresql
```
``` bash
pip install gunicorn
```

``` bash
sudo su postgres
psql
ALTER USER postgres WITH PASSWORD '123456';
CREATE DATABASE my_db;

\q
exit
```
``` bash
git clone https://github.com/DonKoteyka/CICD_.git
git checkout main
cd CICD_/

python3 -m venv env
source env/bin/activate
which python

pip install -r requirements.txt
pip freeze
```
``` bash
nano logistic/settings.py
```
``` bash
sudo systemctl start nginx
```
``` bash
gunicorn main.wsgi -b 0.0.0.0:8000
```


## create unicorn deamon
``` bash
sudo nano /etc/systemd/system/gunicorn.service
```
```
[Unit]
Description=Gunicorn service	
After=network.target
[Service]
User=kot
Group=www-data
WorkingDirectory=/home/kot/CICD_/ 
ExecStart=/home/kot/CICD_/env/bin/gunicorn main.wsgi:application --workers=3 -b unix:/home/kot/CICD_/logistic/project.sock
[Install]
WantedBy=multi-user.target
```
``` bash
sudo systemctl start gunicorn
sudo systemctl enable gunicorn
```


## Nginx
``` bash
sudo nano /etc/nginx/sites-available/project
```
```
server {
listen 80;
server_name 79.174.92.60;
location /static/ {
root /home/kot/CICD_/;
}
location / {
include proxy_params;
proxy_pass http://unix:/home/kot/CICD_/logistic/project.sock;
}
}
```
``` bash
sudo ln -s /etc/nginx/sites-available/project /etc/nginx/sites-enabled/
```
``` bash
sudo systemctl restart nginx
sudo systemctl status nginx
```
``` bash
sudo nano /etc/nginx/nginx.conf
```

[
    os.path.join(BASE_DIR, 'static'),
]
