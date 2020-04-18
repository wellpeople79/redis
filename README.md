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



cd /src

make test 성공후

cd ..

cd /utils

sudo ./install_server.sh

default로 엔터

마지막 redis executable path 설정함.

/home/current79ing/redis-stable/src/redis-server

서비스 등록
----------
sudo systemctl start redis_6379

.bashrc파일을 수정함.
--------------------
vim ~/.bashrc

문자의 끝에 alias를 등록함.
--------------------------
<pre>
alias redis-cli=~/redis-stable/src/redis-cli
alias redis-server=~/redis-stable/src/redis-server
</pre>

redis-cli 실행시 auth관련 에러시.
--------------------------------
redis-cli -p 6379 실행후

auth관련 에러가 나면

AUTH 설정한 password 입력하면됨.

redis-cli 관련 옵션
------------------
* -p : port
* -a : password


redis-cli 명령어로 shutdown후 master, slave재실행함.
---------------------------------------------------
<pre>
# redis shutdown
redis-cli -p 6379 shutdown
redis-cli -p 6380 shutdown
redis-cli -p 6381 shutdown
</pre>

redis 실행중인 process 조회
--------------------------
<pre>
ps -ef | grep redis
</pre>

센티널 설정
----------
51001.conf, 51002.conf, 51003.conf
 
센티널이 실행될 포트 설정
------------------------
port 51001: 설정파일마다 다르게 함.

센티널이 감시할 레디스 Master 인스턴스 정보설정.
----------------------------------------------
quorum: 의사결정에 필요한 최소 Sentinel 노드수임.( 2개로 과반수 설정함 )
<pre>
sentinel monitor mymaster [redis master host] [redis master port] [quorum]
sentinel monitor mymaster 127.0.0.1 10000 2
</pre>

센티널이 Master 인스턴스에 접속하기 위한 패스워드설정.
---------------------------------------------------
sentinel auth-pass mymaster foobared

센티널이 Master 인스턴스와 접속이 끊겼다는 것을 알기 위한 최소한의 시간설정.
--------------------------------------------------------------------------
sentinel down-after-milliseconds mymaster 30000

페일오버 작업 시간의 타임오버 시간[ 기본값으로 3분 ]
--------------------------------------------------
sentinel failover-timeout mymaster 180000

Master로부터 동기화 할 수 있는 slave의 개수를 설정함.
---------------------------------------------------
* 값이 클수록 Master에 부하가 가중됨.
* 값이 1이라면 Slave는 한대씩 Master와 동기화를 진행함.
* sentinel parallel-syncs mymaster 1


sentinel 실행
-------------
* redis-sentinel ../sentinel_51001.conf
* redis-sentinel ../sentinel_51002.conf
* redis-sentinel ../sentinel_51003.conf


Redis 외부접속방법
------------------

* make하기위해 gcc를 다운로드함.
* sudo yum install -y gcc

* redis-cli 설치한 후 make를 실행함.
* wget http://download.redis.io/redis-stable.tar.gz && tar xvzf redis-stable.tar.gz && cd redis-stable && make

* 외부에서 redis-server로 연결실행함.
* redis-cli -h 외부ip주소


* redis-cli를 어느곳에나 실행가능하게 설정변경함. 
* sudo cp src/redis-cli /usr/bin/
* redis-cli -h <redis 서버 ip> -p <redis port> -a <password>

Redis 암호설정방법
-----------------

* redis-cli -p {포트번호} 로 실행

* 패스워드가 있는지 확인한다.

<pre>
* config get requirepass
1) "requirepass"
2) ""
</pre>

암호설정이 안된경우이므로 암호설정을 해준다.
-------------------------------------------
<pre>
* config set requirepass xxxxx
* (error) NOAUTH Authentication required.
</pre>

위와 같은 에러가 발생하면 이전에 설정된 암호로 인증을 해준다.
----------------------------------------------------------
<pre>
* auth xxxxx 

* config get requirepass 
1) "requirepass"
2) "xxxxx"
</pre>
