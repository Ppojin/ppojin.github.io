참고
- https://jjeongil.tistory.com/1451
- https://iskra.sarang.net/178
- https://oracle-base.com/articles/misc/oui-silent-installations
- https://oracle-base.com/articles/12c/oracle-db-12cr2-installation-on-oracle-linux-6-and-7


커널 패러미터 	 내용 
 fs.aio-max-nr 	 비동기 파일 최대 열기
 fs.file-max 	 사용 가능한 파일 핸들의 최대 개수를 말하며, 동시에 열 수 있는 파일의 수다. 
 kernel.shmall 	 공유 메모리의 최대 크기
 kernel.shmmax 	 공유 메모리 세그먼트의 최대 크기
 kernel.shmmni 	 공유 메모리 세그먼트의 최대 숫자
 kernel.sem 	아래 네 개의 값을  차례로 설정한다.
     - semmsl   : 세마포어 세트당 최대 세마포어 수
     - semmns  : 시스템에 할당할 수 있는 최대 세마포어 개수
     - semopm : 시스템 호출당 수행할 수 있는 최대 세마포어 수
     - semmni : 세마포어 세트의 최대 수
 net.ipv4.ip_local_port_range 	 신규 접속시에 허용할 수 있는 포트의 사용 범위
 net.core.rmem_default 	 소켓이 사용하는 수신 버퍼(Window Size)의 기본값
 net.core.rmem_max 	 소켓이 사용하는 수신 버퍼(Window Size)의 최대값
 net.core.wmem_default 	 소켓이 사용하는 송신 버퍼(Window Size)의 기본값
 net.core.wmem_max 	 소켓이 사용하는 수신 버퍼(Window Size)의 최대값
`/etc/sysctl.conf`
```
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmall = 2097152
kernel.shmmax = 4056393728
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048586
```

```bash
sudo sysctl -p
sudo sysctl -a
```

# 
```bash
sudo groupadd oinstall
sudo groupadd dba
sudo useradd -g oinstall -G dba oracle
sudo passwd oracle #asdf
```

```
[oracle@ip-172-31-46-22 ~]$ ulimit -a
core file size          (blocks, -c) unlimited
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 31077
max locked memory       (kbytes, -l) 5000000
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1024
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 10240
cpu time               (seconds, -t) unlimited
max user processes              (-u) 4096
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```

`/etc/security/limits.conf`
```
oracle soft nproc 65536
oracle hard nproc unlimited
oracle soft nofile 65536
oracle hard nofile 65536
oracle soft stack 10240
oracle soft core unlimited
oracle hard core unlimited
oracle soft memlock 50000000
oracle hard memlock 50000000
```

```
INFO: *********************************************
INFO: Hard Limit: maximum open file descriptors: This is a prerequisite condition to test whether the hard limit for maximum open file descriptors is set correctly.
INFO: Severity:CRITICAL
INFO: OverallStatus:VERIFICATION_FAILED
INFO: *********************************************
```

```bash
sudo mkdir -p /opt/oracle12/app
sudo mkdir -p /opt/oracle12/oraInventory
sudo chown -R oracle:oinstall /opt/oracle12/app
sudo chown -R oracle:oinstall /opt/oracle12/oraInventory
sudo chmod -R 775 /opt/oracle12/app
sudo chmod -R 775 /opt/oracle12/oraInventory
sudo chmod g+s /opt/oracle12/app
sudo chmod g+s /opt/oracle12/oraInventory
```

```bash
TMPDIR=$TMP; 
export TMPDIR

ORACLE_BASE=/opt/oracle12/app/; 
export ORACLE_BASE

ORACLE_HOME=$ORACLE_BASE/product/12.2.0/dbhome_1; 
export ORACLE_HOME

ORACLE_HOME_LISTNER=$ORACLE_HOME/bin/lsnrctl;
export ORACLE_HOME_LISTNER

ORACLE_SID=orcl; 
export ORACLE_SID

LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib:/usr/lib64; 
export LD_LIBRARY_PATH

CLASSPATH=$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib; 
export CLASSPATH

# export NLS_LANG=KOREAN_KOREA.AL32UTF8
export NLS_LANG=AMERICA.AL32UTF8

PATH=$ORACLE_HOME/bin:$PATH:$HOME/.local/bin:$HOME/bin; 
export PATH
```

`12cR2.rsp`
```bash
oracle.install.responseFileVersion=/oracle/install/rspfmt_dbinstall_response_schema_v12.2.0
oracle.install.option=INSTALL_DB_SWONLY
UNIX_GROUP_NAME=oinstall
INVENTORY_LOCATION=/opt/oracle12/oraInventory
ORACLE_HOME=/opt/oracle12/app/product/12.2.0/dbhome_1
ORACLE_BASE=/opt/oracle12/app
oracle.install.db.InstallEdition=EE
oracle.install.db.OSDBA_GROUP=dba
oracle.install.db.OSOPER_GROUP=dba
oracle.install.db.OSBACKUPDBA_GROUP=dba
oracle.install.db.OSDGDBA_GROUP=dba
oracle.install.db.OSKMDBA_GROUP=dba
oracle.install.db.OSRACDBA_GROUP=dba
SECURITY_UPDATES_VIA_MYORACLESUPPORT=false
DECLINE_SECURITY_UPDATES=true
```

```bash
sudo fallocate -l 1G /swapfile
sudo dd if=/dev/zero of=/swapfile bs=1024 count=1048576
sudo chmod 600 /swapfile

sudo mkswap /swapfile
sudo swapon /swapfile
```

`/etc/fstab`
```
...
/swapfile swap swap defaults 0 0
```

스왑 확인
```bash
sudo swapon --show
# NAME      TYPE  SIZE   USED PRIO
# /swapfile file 1024M 507.4M   -1Copy

sudo free -h
#               total        used        free      shared  buff/cache   available
# Mem:           488M        158M         83M        2.3M        246M        217M
# Swap:          1.0G        506M        517M
```

```bash
su - oracle
./database/runInstaller -silent -executePrereqs -waitforcompletion -ignoreSysPrereqs -responseFile ~/db_install.rsp
# Oracle Universal Installer 시작 중...

# 임시 공간 확인 중: 500MB 이상이어야 합니다..   실제 91690MB    성공
# 스왑 공간 확인 중: 150MB 이상이어야 합니다..   실제 1023MB    성공
# 다음에서 Oracle Universal Installer의 시작을 준비하는 중 /tmp/OraInstall2023-06-07_03-43-18PM. 기다리십시오.
```