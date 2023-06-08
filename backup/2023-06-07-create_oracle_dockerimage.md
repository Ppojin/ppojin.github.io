---
title:  "create oracle dockerimage"
date:   2023-06-07 18:22:00 +0900
categories: 
  - infrastructure
  - cheet_sheet
tags: 
  - docker
  - oracle
---

# download on oracle archive
- https://edelivery.oracle.com/

# clone @oracle/docker-images repository on github
```bash
git clone https://github.com/oracle/docker-images.git
cp LINUX.X64_112020_db_home.zip ~/docker-images/OracleDatabase/SingleInstance/dockerfiles/11.2.0.2
cd docker-images/OracleDatabase/SingleInstance/dockerfiles

```

# 

```
ca -silent \
    -createDatabase \
    -templateName General_Purpose.dbc \
    -gdbName EOPSDEV.localhost.com \
    -sid EOPSDEV \
    -sysPassword sys \
    -systemPassword sys \
    -emConfiguration NONE \
    -datafileDestination /u02/oradata \
    -storageType FS \
    -characterSet AL32UTF8
```
