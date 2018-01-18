title: memcached安装
date: 2015-05-16
categories : 
  - tools
tags : 
  - memcache
---

在MyBatis集成第三方缓存的时候，用到了Memcached，不过自己的电脑上只有MongoDB，没有Memcached，所以装了一个Memcached，在这里记录一下

使用homebrew进行安装，在安装之前可以看一下homebrew已经安装了哪些软件

```sh

lyy:~ liuyiyou$ brew list

```

安装了运行如下方法


```sh

lyy:~ liuyiyou$ brew install memcached
==> Installing dependencies for memcached: openssl, libevent
==> Installing memcached dependency: openssl
==> Downloading https://homebrew.bintray.com/bottles/openssl-1.0.2a-1.mavericks.bottle.tar.gz

curl: (22) The requested URL returned error: 404 Not Found
Error: Failed to download resource "openssl"
Download failed: https://homebrew.bintray.com/bottles/openssl-1.0.2a-1.mavericks.bottle.tar.gz
Warning: Bottle installation failed: building from source.
Error: /usr/local/opt/makedepend not present or broken
Please reinstall makedepend. Sorry :(
lyy:~ liuyiyou$ brew install memcached

```

可见需要依赖openssl和 libevent，而前者无法```依赖```下载，只好先单独下载

```sh

lyy:~ liuyiyou$ brew install openssl
==> Downloading https://homebrew.bintray.com/bottles/openssl-1.0.2a-1.mavericks.bottle.tar.gz

curl: (22) The requested URL returned error: 404 Not Found
Error: Failed to download resource "openssl"
Download failed: https://homebrew.bintray.com/bottles/openssl-1.0.2a-1.mavericks.bottle.tar.gz
Warning: Bottle installation failed: building from source.
==> Installing openssl dependency: makedepend
==> Downloading https://homebrew.bintray.com/bottles/makedepend-1.0.5.mavericks.bottle.tar.gz
######################################################################## 100.0%
==> Pouring makedepend-1.0.5.mavericks.bottle.tar.gz
🍺  /usr/local/Cellar/makedepend/1.0.5: 7 files, 92K
==> Downloading https://www.openssl.org/source/openssl-1.0.2a.tar.gz

curl: (28) Operation timed out after 5605 milliseconds with 0 out of 0 bytes received
Trying a mirror...
==> Downloading https://raw.githubusercontent.com/DomT4/LibreMirror/master/OpenSSL/openssl-1.0.2a.ta
######################################################################## 100.0%
==> perl ./Configure --prefix=/usr/local/Cellar/openssl/1.0.2a-1 --openssldir=/usr/local/etc/openssl
==> make depend
==> make
==> make test
==> make install MANDIR=/usr/local/Cellar/openssl/1.0.2a-1/share/man MANSUFFIX=ssl
==> Caveats
A CA file has been bootstrapped using certificates from the system
keychain. To add additional certificates, place .pem files in
  /usr/local/etc/openssl/certs

and run
  /usr/local/opt/openssl/bin/c_rehash

This formula is keg-only, which means it was not symlinked into /usr/local.

Mac OS X already provides this software and installing another version in
parallel can cause all kinds of trouble.

Apple has deprecated use of OpenSSL in favor of its own TLS and crypto libraries

Generally there are no consequences of this for you. If you build your
own software and it requires this formula, you'll need to add to your
build variables:

    LDFLAGS:  -L/usr/local/opt/openssl/lib
    CPPFLAGS: -I/usr/local/opt/openssl/include

==> Summary
🍺  /usr/local/Cellar/openssl/1.0.2a-1: 463 files, 18M, built in 4.7 minutes
lyy:~ liuyiyou$ brew list
gradle	 makedepend	openssl
lyy:~ liuyiyou$ brew install memcached
==> Installing memcached dependency: libevent
==> Downloading https://homebrew.bintray.com/bottles/libevent-2.0.22.mavericks.bottle.tar.gz
######################################################################## 100.0%
==> Pouring libevent-2.0.22.mavericks.bottle.tar.gz
🍺  /usr/local/Cellar/libevent/2.0.22: 48 files, 1.8M
==> Installing memcached
==> Downloading https://homebrew.bintray.com/bottles/memcached-1.4.20.mavericks.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring memcached-1.4.20.mavericks.bottle.1.tar.gz
==> Caveats
To have launchd start memcached at login:
    ln -sfv /usr/local/opt/memcached/*.plist ~/Library/LaunchAgents
Then to load memcached now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.memcached.plist
Or, if you don't want/need launchctl, you can just run:
    /usr/local/opt/memcached/bin/memcached
==> Summary
🍺  /usr/local/Cellar/memcached/1.4.20: 10 files, 184K
lyy:~ liuyiyou$ brew list
gradle	 libevent	makedepend	memcached	openssl

```


启动memcached

```sh

lyy:~ liuyiyou$ /usr/local/bin/memcached -p 11211

```

一般情况下，不会使用这个参数，因为会占用终端，而是使用一个-d参数 ，作为一个守护进程来使用，具体如何使用，可以通过
```memcached -h```查看

