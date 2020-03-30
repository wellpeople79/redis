# redis
redis

$ wget http://download.redis.io/releases/redis-5.0.8.tar.gz
$ tar xzf redis-5.0.8.tar.gz
$ cd redis-5.0.8
$ make

$ src/redis-server

$ src/redis-cli
redis> set foo bar
OK
redis> get foo
"bar"

사양보는법
[ec2-user@ip주소]$ cat /etc/*-release | uniq
