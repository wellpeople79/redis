redis
=====
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


설치주소 추가
------------
wget http://download.redis.io/redis-stable.tar.gz

tar xvzf redis-stable.tar.gz

cd redis-stable

make

sudo make install

sudo yum install -y tcl
 
redis-server로 서버실행함.


설치시 에러난 경우
-----------------

1. gcc   
sudo yum install gcc


2. jemalloc
* centOS 7.0 이전버전
-> yum -y install jemalloc
* centOS 7.0 이상버전
-> yum -y install epel-release
-> yum -y install varnish

위 경우에도 안되는 경우
----------------------

cd deps

sudo make hiredis jemalloc linenoise lua

cd ..

sudo make install

sudo yum install -y tcl

make rebuild방법
<pre>
#make distclean
#make
</pre>
