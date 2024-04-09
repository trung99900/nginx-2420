# nginx-2420 - Part2

## Table of Contents

## Installation

`sudo pacman -S ufw`

## Instruction

1. Firewall Enable

- Enable UFW: `sudo ufw enable`
- Allow SSH: `sudo ufw allow ssh`
- Allow HTTP: `sudo ufw allow http`
- Allow HTTPS: `sudo ufw allow https`
- Check firewall status: `sudo ufw status numbered`

2. Service configuration

#### Place backend binary on your system \

- Open the terminal in your local, go to `Downloads` directory and type `git clone https://gitlab.com/cit2420/2420_notes_w24.git`
- `sftp linux` This is my server name to connect to
- `put C:\Users\alext\Downloads\2420_notes_w24\attachments\hello-server`
- `exit` to exit sftp \
*** `hello-server` file now is already existed in your home directory ***

3. Create a new service file to run into the backend
- `sudo mv hello-server /web/html/nginx-2420/`
- `cd /web/html/nginx-2420` and `sudo chmod +x hello-server` to make the backend file executable
- `sudo vim /etc/systemd/system/backend.service` to create a new service
- put these commands to the service file

``` bash
[Unit]
Description=Backend Service
After=network.target

[Service]
ExecStart=/web/html/nginx-2420/hello-server
Restart=always

[Install]
WantedBy=multi-user.target
```
- `sudo systemctl daemon-reload`
- `sudo systemctl enable --now backend`
- `sudo systemctl status backend`

4. Edit nginx server block, add reverse proxy to backend
- `sudo rm /etc/nginx/sites-enabled/nginx-2420` 
- `sudo vim /etc/nginx/sites-available/nginx-2420`

``` bash
location /hey {
    proxy_pass http://127.0.0.1:8080;
}

location /echo {
    proxy_pass http://127.0.0.1:8080;

}

```
- `sudo ln -s /etc/nginx/sites-available/nginx-2420 /etc/nginx/sites-enabled/nginx-2420`
#### Restart the server and run the nginx again ####.
- `sudo systemctl stop nginx` to stop nginx
- `sudo systemctl disable nginx` to disable nginx
- `sudo systemctl enable --now nginx` to enable nginx
- `curl 64.23.245.112/hey` to check the backup 