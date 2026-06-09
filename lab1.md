
# 1. Verify Docker Installation

## Check Docker Version

```bash
docker version
```

### Output

```text
Client:
 Version:           29.1.3
 API version:       1.52
 Go version:        go1.24.4

Server:
 Engine:
  Version:          29.1.3
  API version:      1.52
  Experimental:     false

containerd:
  Version:          2.2.1

runc:
  Version:          1.3.4-0ubuntu1~24.04.1
```

---

## Check Docker Service Status

```bash
systemctl status docker
```

### Output

```text
docker.service - Docker Application Container Engine
Loaded: loaded
Active: active (running)
Main PID: 1974 (dockerd)
```

Docker daemon is running successfully.

---

# 2. Pull Images from Docker Hub

## Pull Nginx

```bash
docker pull nginx
```

### Result

```text
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```

---

## Pull Apache HTTPD

```bash
docker pull httpd
```

### Result

```text
Status: Downloaded newer image for httpd:latest
docker.io/library/httpd:latest
```

---

## Pull BusyBox

```bash
docker pull busybox
```

### Result

```text
Status: Downloaded newer image for busybox:latest
docker.io/library/busybox:latest
```

---

## Pull Node.js

```bash
docker pull node
```

### Result

```text
Status: Downloaded newer image for node:latest
docker.io/library/node:latest
```

---

## Pull MySQL

```bash
docker pull mysql
```

### Result

```text
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest
```

---

## Pull phpMyAdmin

```bash
docker pull phpmyadmin/phpmyadmin
```

### Result

```text
Status: Downloaded newer image for phpmyadmin/phpmyadmin:latest
docker.io/phpmyadmin/phpmyadmin:latest
```

---

## Pull Jenkins LTS

```bash
docker pull jenkins/jenkins:lts
```

### Result

```text
Status: Downloaded newer image for jenkins/jenkins:lts
docker.io/jenkins/jenkins:lts
```

---

# 3. View Downloaded Images

```bash
docker images
```

### Output

```text
IMAGE                          ID             DISK USAGE
busybox:latest                 c6348fa86ba0       4.45MB
httpd:latest                   bb74261f37ff        117MB
jenkins/jenkins:lts            dfe2d90c6ac4        482MB
mysql:latest                   094aec64961b        950MB
nginx:latest                   7aaca76c508f        161MB
node:latest                    fd3944d959a8       1.24GB
phpmyadmin/phpmyadmin:latest   e66b1f5a8c58        742MB
```

---

# 4. Run Nginx Container

```bash
docker run -d --name nginx-container -p 8080:80 nginx
```

### Output

```text
26480981bab6d6796e71578c5ce383f8ceaf658f2ee9e4b91ed56859de648ac0
```

---

## Verify Running Container

```bash
docker ps
```

### Output

```text
CONTAINER ID   IMAGE   PORTS
26480981bab6   nginx   0.0.0.0:8080->80/tcp
```

---

## Test Nginx

```bash
curl localhost:8080
```

### Output

```html
<title>Welcome to nginx!</title>
<h1>Welcome to nginx!</h1>
```

Nginx is successfully running and accessible on port 8080.

---

# 5. Run Apache HTTPD Container

```bash
docker run -d --name httpd-container -p 8081:80 httpd
```

### Output

```text
54444da156fa90eaf528b7980e9c47c110556aa6ee617c3072ac88735721f684
```

---

## Test Apache

```bash
curl localhost:8081
```

### Output

```html
<title>It works! Apache httpd</title>
<p>It works!</p>
```

Apache HTTP Server is working correctly.

---

# 6. Run Jenkins Container

```bash
docker run -d \
--name jenkins-container \
-p 8082:8080 \
-p 50000:50000 \
jenkins/jenkins:lts
```

### Output

```text
50d7a7b6a7416b86c39a8dd791536c842eb74a9fea21676a27a47bf0f7e0299a
```

---

## Test Jenkins

```bash
curl localhost:8082
```

### Output

```html
<title>Starting Jenkins</title>
Jenkins is getting ready to work
```

Jenkins initialization page is displayed successfully.

---

# 7. Attempt phpMyAdmin Deployment

```bash
docker run -d \
--name phpmyadmin-container \
--link mysql-container:db \
-p 8083:80 \
phpmyadmin/phpmyadmin
```

### Output

```text
docker: Error response from daemon:
No such container: mysql-container
```

### Reason

phpMyAdmin requires an existing MySQL container.

---

# 8. Run MySQL Container

```bash
docker run -d \
--name mysql-container \
-e MYSQL_ROOT_PASSWORD=root123 \
-p 3306:3306 \
mysql
```

### Output

```text
93902ba7ca2ed3f2ca55d4c2cd5c561d1a8370c8a22c8f8e9c993238349d13f4
```

---

# 9. Access MySQL Container

```bash
docker exec -it mysql-container bash
```

### Login

```bash
mysql -u root -proot123
```

### Output

```sql
show databases;
```

Result:

```text
information_schema
mysql
performance_schema
sys
```

MySQL is functioning properly.

---

# 10. Execute Commands Inside Containers

## Nginx Container

```bash
docker exec -it nginx-container bash
```

### Verify Hostname

```bash
hostname
```

Output:

```text
26480981bab6
```

### Verify Nginx Version

```bash
nginx -v
```

Output:

```text
nginx version: nginx/1.31.1
```

### Check OS

```bash
cat /etc/os-release
```

Output:

```text
PRETTY_NAME="Debian GNU/Linux 13 (trixie)"
```

---

## Apache Container

```bash
docker exec -it httpd-container bash
```

### Check Version

```bash
httpd -v
```

Output:

```text
Server version: Apache/2.4.68 (Unix)
```

---

## Jenkins Container

```bash
docker exec -it jenkins-container bash
```

### Check Java Version

```bash
java -version
```

Output:

```text
openjdk version "21.0.11"
```

### Check Running Processes

```bash
ps -ef
```

Output:

```text
PID 1 -> tini
PID 6 -> java (Jenkins)
```

---

# 11. View Container Logs

## Nginx Logs

```bash
docker logs nginx-container
```

Important entries:

```text
Configuration complete; ready for start up
nginx/1.31.1
start worker processes
```

---

## Jenkins Logs

```bash
docker logs jenkins-container
```

Important entries:

```text
Jenkins initial setup is required.
```

Generated admin password:

```text
83f11dafaaf243f5bafb214f69a9e1f0
```

---

# 12. Stop and Start Containers

## Stop Containers

```bash
docker stop nginx-container
docker stop httpd-container
```

### Output

```text
nginx-container
httpd-container
```

---

## Start Containers

```bash
docker start nginx-container
docker start httpd-container
```

### Output

```text
nginx-container
httpd-container
```

---

# 13. View All Containers

```bash
docker ps -a
```

### Output

```text
mysql-container
jenkins-container
httpd-container
nginx-container
```

---

# 14. Restart Container

```bash
docker restart nginx-container
```

### Output

```text
nginx-container
```

---

# 15. Remove Containers

## Attempt Removal of Running MySQL Container

```bash
docker rm mysql-container
```

### Output

```text
container is running
```

---

## Stop Container First

```bash
docker stop mysql-container
```

---

## Remove Container

```bash
docker rm mysql-container
```

### Output

```text
mysql-container
```

---

# 16. Remove Stopped Containers

```bash
docker container prune
```

### Output

```text
WARNING! This will remove all stopped containers.
```

---

# 17. Remove Docker Image

## Remove MySQL Image

```bash
docker rmi mysql
```

### Output

```text
Untagged: mysql:latest
Deleted: sha256:094aec64961b...
```

MySQL image removed successfully.

---

# 18. Inspect Container

```bash
docker inspect nginx-container
```

Important information obtained:

### Container Name

```text
/nginx-container
```

### Running State

```text
"Status": "running"
```

### Port Mapping

```text
80/tcp -> 8080
```

### Network

```text
Gateway: 172.17.0.1
IPAddress: 172.17.0.2
```

### Image

```text
nginx
```

### Driver

```text
overlay2
```

---

# 19. Network Verification

```bash
ip a
```

Important interfaces:

### Host Network

```text
enp1s0 -> 172.30.1.2/24
```

### Docker Bridge

```text
docker0 -> 172.17.0.1/16
```

### Nginx Container Network

```text
IPAddress -> 172.17.0.2
```

---

