# nginx-2420 - Part2: adds a firewall using UFW and a reverse proxy server to the backend

## Service Prerequisite - ufw Installation

```bash
sudo pacman -S ufw
```

Instructions
1. ufw Enable

- Enable UFW: sudo ufw enable
- Allow SSH: sudo ufw allow ssh
- Allow HTTP: sudo ufw allow http
- Allow HTTPS: sudo ufw allow https
- Check firewall status: sudo ufw status numbered
2. Service Configuration

## Place backend binary on your system ##
- Open the terminal on your local machine, navigate to the Downloads directory, and execute:

```bash
git clone https://gitlab.com/cit2420/2420_notes_w24.git
```

- Connect to the server using SFTP:
```bash
sftp linux
```
- Upload the hello-server file:

```bash
put C:\Users\alext\Downloads\2420_notes_w24\attachments\hello-server

```
- Exit SFTP:
```bash
exit
```

### The hello-server file is now present in your home directory. ###
3. Create a New Service File to Run the Backend
- Move hello-server to the desired directory:
```bash
sudo mv hello-server /web/html/nginx-2420/
```
- Make the backend file executable:

```bash
cd /web/html/nginx-2420
sudo chmod +x hello-server
```
- Create a new service file:
```bash
sudo vim /etc/systemd/system/backend.service
```
- Add the following content:
```bash
[Unit]
Description=Backend Service
After=network.target

[Service]
ExecStart=/web/html/nginx-2420/hello-server
Restart=always

[Install]
WantedBy=multi-user.target
```
- Reload systemd manager configuration:

```bash
sudo systemctl daemon-reload
```
- Enable and start the backend service:

```bash
sudo systemctl enable --now backend
```
- Check the status of the backend service:

```bash
sudo systemctl status backend
```
4. Edit Nginx Server Block, Add Reverse Proxy to Backend
- Remove the existing server block configuration:

```bash
sudo rm /etc/nginx/sites-enabled/nginx-2420
```
- Edit the Nginx server block configuration file:

```bash
sudo vim /etc/nginx/sites-available/nginx-2420
```
#### Add the following content: ####
```bash
location /hey {
    proxy_pass http://127.0.0.1:8080;
}

location /echo {
    proxy_pass http://127.0.0.1:8080;
}
```
- Create a symbolic link to enable the server block:

```bash
sudo ln -s /etc/nginx/sites-available/nginx-2420 /etc/nginx/sites-enabled/nginx-2420
```
#### Restart the Server and Run Nginx Again ####
- Stop Nginx:
```bash
sudo systemctl stop nginx
```
- Disable Nginx:
```bash
sudo systemctl disable nginx
```
- Enable and start Nginx again:
```bash
sudo systemctl enable --now nginx
```
- Check the backend by sending a request:
```bash
curl 64.23.245.112/hey
```
