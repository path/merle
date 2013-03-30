merle : An erlang based memcached client.

Version : 0.3

Author : Joe Williams <joe@joetify.com>
Contributors : Nick Gerakines <nick@gerakines.net>

Info : http://github.com/joewilliams/merle/

merle uses LShift's gen_server2 module/behavior for faster message queues.
http://hg.rabbitmq.com/rabbitmq-server/file/b95f2fd4e3f6/src/gen_server2.erl

This code is available as Open Source Software under the MIT license.


Features:
* Support for stats, version, getkey, getskey, delete, set, add, replace, cas, flushall, verbosity

Notes:
* ~~Uses term_to_binary and binary_to_term to serialize/deserialize Erlang terms before sending/receiving them. This allows for native Erlang terms to be returned from memcached but doesn't play well using other languages after setting values with merle or using merle to get values set by other languages.~~
* added interop with Python
* added simple connection pool with cleanup

Merle Based Projects:

http://github.com/cstar/merle
http://github.com/issuu/merle
http://github.com/0lvin/merle
http://github.com/ppolv/merle

Usage:

* Connecting to memcached *

Using defaults:

> merle:connect().

Set your own:

> merle:connect("HOSTNAME", 11211).


* A few operations *

> merle:set(a, asdf).
ok
> merle:getkey(a).
asdf

> merle:set(a, asdf).
ok
> merle:getskey(a).
[4,asdf]
> merle:cas(a, 4, asdfasdf).
ok
> merle:getskey(a).
[5,asdfasdf]

> merle:delete(a).
ok

* Informational commands *

> merle:version().
["VERSION 1.2.6"]

> merle:stats(slabs).
["STAT 1:chunk_size 104","STAT 1:chunks_per_page 10082",
 "STAT 1:total_pages 1","STAT 1:total_chunks 10082",
 "STAT 1:used_chunks 10081","STAT 1:free_chunks 1",
 "STAT 1:free_chunks_end 10080","STAT active_slabs 1",
 "STAT total_malloced 1048528","END"]

> merle:stats().
["STAT pid 27195","STAT uptime 497","STAT time 1232843046",
 "STAT version 1.2.6","STAT pointer_size 64",
 "STAT rusage_user 0.000000","STAT rusage_system 0.008000",
 "STAT curr_items 1","STAT total_items 5","STAT bytes 83",
 "STAT curr_connections 2","STAT total_connections 5",
 "STAT connection_structures 3","STAT cmd_get 5",
 "STAT cmd_set 5","STAT get_hits 5","STAT get_misses 0",
 "STAT evictions 0","STAT bytes_read 216",
 "STAT bytes_written 468","STAT limit_maxbytes 67108864",
 "STAT threads 1","END"]
