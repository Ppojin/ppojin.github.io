---
title:  "Install `docker` on `Amazon_Linux_2023`"
date:   2023-06-07 17:38:00 +0900
categories: 
  - infrastructure
  - cheet_sheet
tags: 
  - docker-compose
  - docker
  - aws
  - ec2
---

# 1. Install docker
```bash
sudo yum update -y
sudo yum install docker -y
sudo service docker start
sudo systemctl enable docker
sudo usermod -aG docker ec2-user
```

정상 작동 확인
```bash
docker run hello-world
docker rm $(docker stop $(docker ps -aq))
```

# 2. Install docker-compose
```bash
sudo curl \
  -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) \
  -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

정상 실행 확인
```bash
docker-compose -v
```