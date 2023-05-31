---
title:  "pip3 install mysqlclient on mac without MySQL server"
date:   2020-04-22 16:10:37 +0900
categories: java
---


```shell
$ pip3 install mysqlclient
Password:
WARNING: The directory '/Users/hwanghyojin/Library/Caches/pip' or its parent directory is not owned or is not writable by the current user. The cache has been disabled. Check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
Collecting mysqlclient
  Downloading mysqlclient-1.4.6.tar.gz (85 kB)
     |████████████████████████████████| 85 kB 92 kB/s 
    ERROR: Command errored out with exit status 1:
     command: /usr/local/bin/python3 -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/private/tmp/pip-install-kir99tn_/mysqlclient/setup.py'"'"'; __file__='"'"'/private/tmp/pip-install-kir99tn_/mysqlclient/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' egg_info --egg-base /private/tmp/pip-install-kir99tn_/mysqlclient/pip-egg-info
         cwd: /private/tmp/pip-install-kir99tn_/mysqlclient/
    Complete output (12 lines):
    /bin/sh: mysql_config: command not found
    /bin/sh: mariadb_config: command not found
    /bin/sh: mysql_config: command not found
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/private/tmp/pip-install-kir99tn_/mysqlclient/setup.py", line 16, in <module>
        metadata, options = get_config()
      File "/private/tmp/pip-install-kir99tn_/mysqlclient/setup_posix.py", line 61, in get_config
        libs = mysql_config("libs")
      File "/private/tmp/pip-install-kir99tn_/mysqlclient/setup_posix.py", line 29, in mysql_config
        raise EnvironmentError("%s not found" % (_mysql_config_path,))
    OSError: mysql_config not found
    ----------------------------------------
ERROR: Command errored out with exit status 1: python setup.py egg_info Check the logs for full command output.
```

```shell
$ brew install mysql-client
Updating Homebrew...
==> Auto-updated Homebrew!
Updated 2 taps (homebrew/core and homebrew/cask).
==> Updated Formulae
ammonite-repl      cypher-shell       frps               paket              rust
ansible            diff-pdf           fselect            pdfpc              terragrunt
awscli             dita-ot            gatsby-cli         pdftoipe           vgmstream
bgpdump            dnstwist           git-trim           platformio         wasm3
bitrise            erlang@21          gjs                poppler            wtf
bochs              exploitdb          http-server        pylint
consul-template    firebase-cli       languagetool       qbs
cups               frpc               logtalk            robot-framework
==> Updated Casks
axe-electrum            google-chrome           metasploit              teamviewer
axure-rp                jetbrains-toolbox       netron                  upic
chalk                   keka                    reaper                  vlc
cyberduck               listen1                 signal                  webcatalog

==> Downloading https://homebrew.bintray.com/bottles/mysql-client-8.0.19.catalina.bottle.tar.g
==> Downloading from https://akamai.bintray.com/43/43a72c20ae7b146f6dc7d01b9f63bef605972c26ecf
######################################################################## 100.0%
==> Pouring mysql-client-8.0.19.catalina.bottle.tar.gz
==> Caveats
mysql-client is keg-only, which means it was not symlinked into /usr/local,
because it conflicts with mysql (which contains client libraries).

If you need to have mysql-client first in your PATH run:
  echo 'export PATH="/usr/local/opt/mysql-client/bin:$PATH"' >> ~/.zshrc

For compilers to find mysql-client you may need to set:
  export LDFLAGS="-L/usr/local/opt/mysql-client/lib"
  export CPPFLAGS="-I/usr/local/opt/mysql-client/include"

==> Summary
🍺  /usr/local/Cellar/mysql-client/8.0.19: 135 files, 149.4MB
Removing: /Users/hwanghyojin/Library/Caches/Homebrew/mysql-client--8.0.18.catalina.bottle.tar.gz... (37.7MB)
 
$ echo 'export PATH="/usr/local/opt/mysql-client/bin:$PATH"' >> ~/.bash_profile
 
$ source ~/.bash_profile
 
$ pip3 install mysqlclient
Collecting mysqlclient
  Using cached mysqlclient-1.4.6.tar.gz (85 kB)
Installing collected packages: mysqlclient
    Running setup.py install for mysqlclient ... done
Successfully installed mysqlclient-1.4.6
 
```