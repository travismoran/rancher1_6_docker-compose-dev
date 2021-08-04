Host Setup

# patch & install docker, mark docker to prevent upgrade.
```
apt-get -y update
apt-get -y upgrade
curl https://releases.rancher.com/install-docker/18.03.sh | sh
systemctl enable docker
curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
apt list --upgradable
apt-mark hold docker-ce
apt upgrade
shutdown -r now
```

# setup .ssh/config and key then clone repo
```
git clone git@bitbucket.org:travis_moran/sg-nginx-rancher-mysql-compose.git
```

# fresh setup only steps
```
mkdir -p /root/sg-nginx-rancher-mysql-compose/mysql/data
docker run --name mysql -v /root/sg-nginx-rancher-mysql-compose/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag

mysql -h localhost -u root -pROOTPASSWORD 
CREATE DATABASE IF NOT EXISTS cattle COLLATE = 'utf8_general_ci' CHARACTER SET = 'utf8';
GRANT ALL ON cattle.* TO 'cattle'@'%' IDENTIFIED BY 'PASSWORD';
GRANT ALL ON cattle.* TO 'cattle'@'localhost' IDENTIFIED BY 'PASSWORD';
quit;
```

# stop and rm mysql container

# launch stack
```
cd /root/sg-nginx-rancher-mysql-compose/
docker-compose up -d
```

# Once Rancher initializes it will be available on http://<hostip>:8080

# NGINX config included to add SSL currently but not enable, you will have to manually add certs or integrate letsencrypt to the stack

# NGINX -> Edit nginx/build/Dockerfile
# Replace YOURURLHERE with your domain - www.example.com

```
FROM nginx:stable-alpine
COPY ./conf/rancher.conf /etc/nginx/conf.d/rancher.conf
RUN sed -i "s/FQDN/YOURURLHERE/g" /etc/nginx/conf.d/rancher.conf
```