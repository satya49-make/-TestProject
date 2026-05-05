### Deploy Profile using EC2 -> Docker -> NGINX at Port 80
### 1. Create EC2 Instance
- Allow inbound ports:
  - 22 (SSH)
  - 80 (HTTP)
  - 8080 (optional for apps)

### 2. Run Docker Server in EC2
### Install latest stable Docker (if not installed)
```bash
# 1. Download the script
curl -fsSL https://get.docker.com -o install-docker.sh
# 2. Run the script
sudo sh install-docker.sh
```

### 3. Run Nginx Container from Docker
```bash
docker run --name satya-ngx -p 80:80 -d -v ~/docker-nginx/html:/usr/share/nginx/html nginx
docker run -d -p 80:80 -v /home/ubuntu/html:/usr/share/nginx/html nginx
```
- Default Nginx HTML page will be available at:
```
http://<EC2-Public-IP>/
```
### 4. Create Your Own HTML Page and Copy to EC2

### From local machine (Git Bash / Terminal):
```bash
scp -i /path/to/your-key.pem index.html ubuntu@<EC2-Public-IP>:/home/ubuntu/
scp -i /downloads/your-key.pem index.html ubuntu@<EC2-Public-IP>:/home/ubuntu/
 scp -i docker-4.pem index.html ubuntu@13.201.88.155:/home/ubuntu
```

### 5. Copy HTML from EC2 to Nginx Container Directory
### a. Connect to EC2
```bash
ssh -i /path/to/your-key.pem ubuntu@<EC2-Public-IP>
```
### b. Run Nginx container (if not already running)
```bash
docker run --name satya-ngx -p 80:80 -d -v ~/docker-nginx/html:/usr/share/nginx/html nginx
```
### c. Copy/replace index.html inside container
```bash
docker cp /home/ubuntu/index.html <nginx-container-id>:/usr/share/nginx/html/
 cp /home/ubuntu/index.html /home/ubuntu/html/
```

### 6. Reload / Refresh Page
Open in browser:
```
http://<EC2-Public-IP>/
```

### 📌 Notes Section

### Verify file inside container
```bash
docker exec -it <nginx-container-id> ls /usr/share/nginx/html/
```

###  Alternative: Mount Volume for Auto Updates
```bash
docker run -d -p 80:80 -v /home/ubuntu/html:/usr/share/nginx/html nginx
```
- Place your `index.html` in:
```
/home/ubuntu/html
```
- Nginx will serve it automatically (no need to copy again).

### 🔑 SSH Troubleshooting

### Check if `.pem` file exists
```bash
ls -l /path/to/docker-key-1.pem
```
### Fix permissions (Linux/Mac)
```bash
chmod 400 /path/to/docker-key-1.pem
```
### Correct username:
- Ubuntu AMI → `ubuntu`
- Amazon Linux → `ec2-user`

### ✅ Quick Vim Installation (if needed)
```bash
sudo apt update
sudo apt install vim
vim --version
```
```
docker -v

curl -fsSL https://get.docker.com -o install-docker.sh
sudo sh install-docker.sh

docker run --name satya-ngx -p 80:80 -d -v ~/docker-nginx/html:/usr/share/nginx/html nginx

scp -i /path/to/your-key.pem index.html ubuntu@<EC2-Public-IP>:/home/ubuntu/
scp -i /downloads/your-key.pem index.html ubuntu@<EC2-Public-IP>:/home/ubuntu/

ssh -i /path/to/your-key.pem ubuntu@<EC2-Public-IP>

docker run --name satya-ngx -p 80:80 -d -v ~/docker-nginx/html:/usr/share/nginx/html nginx

docker cp /home/ubuntu/index.html <nginx-container-id>:/usr/share/nginx/html/

docker exec -it <nginx-container-id> ls /usr/share/nginx/html/

docker run -d -p 80:80 -v /home/ubuntu/html:/usr/share/nginx/html nginx

ls -l /path/to/docker-key-1.pem

chmod 400 /path/to/docker-key-1.pem

sudo apt update
sudo apt install vim
vim --version
```
