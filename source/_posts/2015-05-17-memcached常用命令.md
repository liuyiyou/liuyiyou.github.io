title: memcached常用命令
date: 2015-05-17
categories : 
  - tools
tags : 
  - memcache
---

转载自：[Memcached常用命令](http://www.cnblogs.com/jeffwongishandsome/archive/2011/11/06/2238265.html)

首先，以下的这些命令都是在连接Memcached之后才进行

```sh

lyy:~ liuyiyou$ /usr/local/bin/memcached -d 11211
lyy:~ liuyiyou$ /telnet   localhost  11211

```


### 存储命令

```

<command name> <key> <flags> <exptime> <bytes> <data block>

```

 ||  * &lt; command name>* || *set/add/replace* ||
 ||  &lt;key> || 查找关键字 ||
 ||  &lt;flags> || 客户机使用它存储关于键值对的额外信息 ||
 ||  &lt;exptime> || 该数据的存活时间，0表示永远 ||
 ||  &lt;bytes> || 存储字节数 ||
 ||  &lt;data block> || 存储的数据块（可直接理解为key-value结构中的value） ||

 


###刷新缓存

flush_all


###状态命令stats 

```sh

stats 
STAT pid 674
STAT uptime 230
STAT time 1432217154
STAT version 1.4.20
STAT libevent 2.0.22-stable
STAT pointer_size 64
STAT rusage_user 0.004909
STAT rusage_system 0.009993
STAT curr_connections 10
STAT total_connections 11
STAT connection_structures 11
STAT reserved_fds 20
STAT cmd_get 2
STAT cmd_set 1
STAT cmd_flush 0
STAT cmd_touch 0
STAT get_hits 2
STAT get_misses 0
STAT delete_misses 1
STAT delete_hits 1
STAT incr_misses 0
STAT incr_hits 0
STAT decr_misses 0
STAT decr_hits 0
STAT cas_misses 0
STAT cas_hits 0
STAT cas_badval 0
STAT touch_hits 0
STAT touch_misses 0
STAT auth_cmds 0
STAT auth_errors 0
STAT bytes_read 176
STAT bytes_written 140
STAT limit_maxbytes 67108864
STAT accepting_conns 1
STAT listen_disabled_num 0
STAT threads 4
STAT conn_yields 0
STAT hash_power_level 16
STAT hash_bytes 524288
STAT hash_is_expanding 0
STAT malloc_fails 0


```


### 状态命令stats  items

执行stats items，可以看到STAT items行，如果memcached存储内容很多，那么这里也会列出很多的STAT items行。

stats items


```sh

//设置用户名
set username 0 0 8
liuyiyou
STORED

//得到用户名
get username
VALUE username 0 8
liuyiyou

//删除用户名
delete username
DELETED
delete username
NOT_FOUND




```

 1.  添加（）

```sh

//设置用户名
set username 0 0 8
liuyiyou
STORED

//得到用户名
get username
VALUE username 0 8
liuyiyou

//删除用户名
delete username
DELETED
delete username
NOT_FOUND


```







