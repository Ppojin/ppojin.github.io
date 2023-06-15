---
title:  "upload file using fstp"
date:   2023-06-07 18:34:00 +0900
categories: 
  - linux
tags: 
  - bash
  - sftp
  - cheet_sheet
---

# local
```bash
$ TARGET_DIRECTORY=$(pwd)
$ ls
```

```bash
sftp target_server
sftp> lcd $TARGET_DIRECTORY
sfpt> lpwd
Local working directory: $TARGET_DIRECTORY
sftp> lls
$TARTEG_FILE
sftp> put $TARGET_DIRECTORY/$TARGET_FILE .
```

> 참고: [How to Use SFTP (SSH File Transfer Protocol)](https://www.hostinger.com/tutorials/how-to-use-sftp-to-safely-transfer-files/)