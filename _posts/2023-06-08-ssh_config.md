---
title:  "ssh config"
date:   2023-06-07 18:07:00 +0900
categories: 
  - bash
  - linux
  - cheet_sheet
tags: 
  - ssh
---

```
host amazing-instance
	Hostname ec2-********.ap-northeast-2.compute.amazonaws.com
	Port 22
	User ec2-user
	IdentityFile ~/.ssh/********.pem

host amazing-pf
	Hostname ec2-********.ap-northeast-2.compute.amazonaws.com
	Port 22
	User ec2-user
	IdentityFile ~/.ssh/********.pem
	StrictHostKeyChecking no
	BindAddress yes
	RemoteCommand cat
	LogLevel Debug
	  
	LocalForward 1521 localhost:1521
```

```bash
ssh amazing-instance
#    ,     #_
#    ~\_  ####_        Amazon Linux 2023
#   ~~  \_#####\
#   ~~     \###|
#   ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
#    ~~       V~' '->
#     ~~~         /
#       ~~._.   _/
#          _/ _/
#        _/m/'
# Last login: Wed Jun  7 13:13:42 2023 from ********
# [ec2-user@ip-******** ~]$ 

ssh amazing-pf
# debug1: Authenticator provider $SSH_SK_PROVIDER did not resolve; disabling
# debug1: Connecting to ec2-********.ap-northeast-2.compute.amazonaws.com port 22.
# debug1: Connection established.
# debug1: identity file /Users/********/.ssh/********.pem type -1
# debug1: identity file /Users/********/.ssh/********.pem-cert type -1
# debug1: Local version string SSH-2.0-OpenSSH_9.0
# debug1: Remote protocol version 2.0, remote software version OpenSSH_8.7
# debug1: compat_banner: match: OpenSSH_8.7 pat OpenSSH* compat 0x04000000
# debug1: Authenticating to ec2-********.ap-northeast-2.compute.amazonaws.com:22 as 'ec2-user'
```