# 빅데이터 분석 환경 구축

## [ Java 8 설치 ]

- Java 8 설치

: apt-get을 이용해 Java 8을 설치한다.

```bash
# EC2 Ubuntu terminal

# Java 8 설치
sudo apt-get install -y openjdk-8-jdk
# Java 버전 확인
java -version
# Java 경로 확인
sudo find / -name java-8-openjdk-amd64 2>/dev/null
# /usr/lib/jvm/java-8-openjdk-amd64
```

## [ Java 환경설정 ]

- Java 환경 변수 설정

```bash
# EC2 Ubuntu terminal

# Java 시스템 환경변수 등록 및 활성화
sudo vim /etc/environment

# 아래 내용 추가 후 저장
PATH 뒤에 ":/usr/lib/jvm/java/bin" 추가
JAVA_HOME="/usr/lib/jvm/java"

# 시스템 환경변수 활성화
source /etc/environment

# 사용자 환경변수 등록
sudo echo 'export JAVA_HOME=/usr/lib/jvm/java' >> ~/.bashrc

# 사용자 환경변수 활성화
source ~/.bashrc
```

Apache Hadoop 3.2.3를 설치하고 환경설정을 진행

하둡 클러스터를 사용하기 위해서는 hdfs-site.xml, hdfs-site.xml, core-site.xml, yarn-site.xml, mapred-site.xml, [hadoop-env.sh](http://hadoop-env.sh/), workers, masters 를 편집

## [ Hadoop 설치 ]

- Apache Hadoop 3.2.3 설치 및 압축 해제

```bash
# EC2 Ubuntu terminal

# 설치파일 관리용 디렉토리 생성
sudo mkdir /install_dir && cd /install_dir
# Hadoop 3.2.2 설치
sudo wget https://dlcdn.apache.org/hadoop/common/hadoop-3.2.3/hadoop-3.2.3.tar.gz
# Hadoop 3.2.2 압축 해제
sudo tar -zxvf hadoop-3.2.3.tar.gz -C /usr/local
# Hadoop 디렉토리 이름 변경
sudo mv /usr/local/hadoop-3.2.3 /usr/local/hadoop
```

## [ Hadoop 환경설정 ]

- Hadoop 환경 변수 설정

```bash
# EC2 Ubuntu terminal

# Hadoop 시스템 환경변수 설정
sudo vim /etc/environment

# 아래 내용 추가 후 저장
PATH 뒤에 ":/usr/local/hadoop/bin" 추가
PATH 뒤에 ":/usr/local/hadoop/sbin" 추가
HADOOP_HOME="/usr/local/hadoop"

# 시스템 환경변수 활성화
source /etc/environment

# Hadoop환 사용자 환경변수 설정
sudo echo 'export HADOOP_HOME=/usr/local/hadoop' >> ~/.bashrc
sudo echo 'export HADOOP_COMMON_HOME=$HADOOP_HOME' >> ~/.bashrc
sudo echo 'export HADOOP_HDFS_HOME=$HADOOP_HOME' >> ~/.bashrc
sudo echo 'export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop' >> ~/.bashrc
sudo echo 'export YARN_CONF_DIR=$HADOOP_HOME/etc/hadoop' >> ~/.bashrc
sudo echo 'export HADOOP_YARN_HOME=$HADOOP_HOME' >> ~/.bashrc
sudo echo 'export HADOOP_MAPRED_HOME=$HADOOP_HOME' >> ~/.bashrc

# 사용자 환경변수 활성화
source ~/.bashrc
```

- hdfs-site.xml 파일 편집

: HDFS에서 사용할 환경 정보를 설정하는 파일이다. hdfs-site.xml 에 설정 값이 없을 경우 hdfs-default.xml 을 기본으로 사용한다.

```bash
# EC2 Ubuntu terminal

# hdfs-site.xml 편집
sudo vim $HADOOP_HOME/etc/hadoop/hdfs-site.xml

# 아래 내용으로 수정 후 저장
<configuration>
    **<!-- configuration hadoop -->**
    <property>
        <name>dfs.replication</name>
        <value>2</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/usr/local/hadoop/data/nameNode</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/usr/local/hadoop/data/dataNode</value>
    </property>
    <property>
        <name>dfs.journalnode.edits.dir</name>
        <value>/usr/local/hadoop/data/dfs/journalnode</value>
    </property>
    <property>
        <name>dfs.nameservices</name>
        <value>my-hadoop-cluster</value>
    </property>
    <property>
        <name>dfs.ha.namenodes.my-hadoop-cluster</name>
        <value>namenode1,namenode2</value>
    </property>
    <property>
        <name>dfs.namenode.rpc-address.my-hadoop-cluster.namenode1</name>
        <value>nn1:8020</value>
    </property>
    <property>
        <name>dfs.namenode.rpc-address.my-hadoop-cluster.namenode2</name>
        <value>nn2:8020</value>
    </property>
    <property>
        <name>dfs.namenode.http-address.my-hadoop-cluster.namenode1</name>
        <value>nn1:50070</value>
    </property>
    <property>
        <name>dfs.namenode.http-address.my-hadoop-cluster.namenode2</name>
        <value>nn2:50070</value>
    </property>
    <property>
        <name>dfs.namenode.shared.edits.dir</name>
        <value>qjournal://nn1:8485;nn2:8485;dn1:8485/my-hadoop-cluster</value>
    </property>
    <property>
        <name>dfs.client.failover.proxy.provider.my-hadoop-cluster</name>
        <value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
    </property>
    <property>
        <name>dfs.ha.fencing.methods</name>
~~~~        <value>shell(/bin/true)</value>
    </property>
    <property>
        <name>dfs.ha.fencing.ssh.private-key-files</name>
        <value>/home/ubuntu/.ssh/id_rsa</value>
    </property>
    <property> 
        <name>dfs.ha.automatic-failover.enabled</name>
        <value>true</value>
    </property>
    <property>
        <name>dfs.name.dir</name>
        <value>/usr/local/hadoop/data/name</value>
    </property>
    <property>
        <name>dfs.data.dir</name>
        <value>/usr/local/hadoop/data/data</value>
    </property>
</configuration>
```

- hdfs-site.xml 속성
    - dfs.replication : HDFS 파일 블록 복제 개수 지정한다.
    - dfs.namenode.name.dir : NameNode에서 관리할 데이터 디렉토리 경로 지정한다.
    - dfs.datanode.data.dir : DataNode에서 관리할 데이터 디렉토리 경로 지정한다.
    - dfs.journalnode.edits.dir : JournalNode는 NameNode의 동기화 상태를 유지한다. 특정 시점에 구성된 fsimage snapshot 이후로 발생된 변경 사항을 editlog라 하며, 해당 데이터의 저장 위치를 설정한다.
    - dfs.nameservices : Hadoop 클러스터의 네임서비스 이름을 지정한다.
    - dfs.ha.namenodes.my-hadoop-cluster : Hadoop 클러스터 네임서비스의 NameNode 이름을 지정한다.( “,”콤마로 구분하여 기재한다.)
    - dfs.namenode.rpc-address.my-hadoop-cluster.namenode1 : 클러스터 네임서비스에 포함되는 NameNode 끼리 RPC 통신을 위해 NameNode의 통신 주소를 지정한다.(여기서는 8020포트 사용)
    - dfs.namenode.rpc-address.my-hadoop-cluster.namenode2 : 클러스터 네임서비스에 포함되는 NameNode 끼리 RPC 통신을 위해 NameNode의 통신 주소를 지정한다.(여기서는 8020포트 사용)
    - dfs.namenode.http-address.my-hadoop-cluster.namenode1 : NameNode1(nn1)의 WEB UI 접속 주소를 지정한다.(여기서는 50070포트 사용)
    - dfs.namenode.http-address.my-hadoop-cluster.namenode2 : NameNode2(nn2)의 WEB UI 접속 주소를 지정한다.(여기서는 50070포트 사용)
    - dfs.namenode.shared.edits.dir : NameNoderk editlog를 쓰고/읽을 JournalNode URL 이다. Zookeeper가 설치된 서버와 동일하게 JournalNode를 설정하면 된다.
    (예 : qjournal://nn1:8485;nn2:8485;dn1:8485/my-hadoop-cluster)
    - dfs.client.failover.proxy.provider.my-hadoop-cluster : HDFS 클라이언트가 Active NameNode에 접근할 때 사용하는 Java class 를 지정한다.
    - dfs.ha.fencing.methods : Favilover 상황에서 기존 Active NameNode를 차단할 때 사용하는 방법을 기재한다.
    (예 : sshfence 그러나 여기서는 shell(/bin/true)를 이용한다.)
    - dfs.ha.fencing.ssh.private-key-files : ha.fencing.method를 sshfence로 지정하였을 경우, ssh를 경유하여 기존 Active NameNode를 죽이는데. 이 때, passpharase를 통과하기 위해 SSH Private Key File을 지정해야한다.
    - dfs.ha.automatic-failover.enabled : 장애 복구를 자동으로 할 지에 대한 여부를 지정한다.
    
- core-site.xml 파일 편집

: Hadoop 시스템 설정 파일이다. 네트워크 튜닝, I/O 튜닝, 파일 시스템 튜닝, 압축 설정 등 Hadoop 시스템 설정을 할 수 있다. HDFS와 MapReduce에서 공통적으로 사용할 환경 정보를 입력할 수 있다. core-site.xml 설정 값이 없으면, core-default.xml 을 기본으로 사용한다.

```bash
# EC2 Ubuntu terminal

# core-site.xml 편집
sudo vim $HADOOP_HOME/etc/hadoop/core-site.xml

# 아래 내용으로 수정 후 저장
<configuration>
        <property>
                <name>fs.default.name</name>
                <value>hdfs://nn1:9000</value>
        </property>
        <property>
                <name>fs.defaultFS</name>
                <value>hdfs://my-hadoop-cluster</value>
        </property>
        <property>
                <name>ha.zookeeper.quorum</name>
                <value>nn1:2181,nn2:2181,dn1:2181</value>
        </property>
</configuration>
```

- core-site.xml 파일 속성
    - [fs.default.name](http://fs.default.name) : HDFS의 기본 통신 주소를 지정한다.
    - fs.defaultFS : HDFS 기본 파일시스템 디렉토리를 지정한다.
    - ha.zookeeper.quorum : Zookeeper가 설치되어 동작할 서버의 주소를 기재한다.(여기서 포트는 2181)
    
- yarn-site.xml 파일 편집

: Resource Manager 및 Node Manager에 대한 구성을 정의한다.

```bash
# EC2 Ubuntu terminal

# Hadoop yarn-site.xml 파일 설정
sudo vim $HADOOP_HOME/etc/hadoop/yarn-site.xml

# 아래 내용으로 수정 후 저장
<configuration>
        <!-- Site specific YARN configuration properties -->
        <property>
                <name>yarn.nodemanager.aux-services</name>
                <value>mapreduce_shuffle</value>
        </property>
        <property>
                <name>yarn.nodemanager.aux-services.mapreduce_shuffle.class</name>
                <value>org.apache.hadoop.mapred.ShuffleHandler</value>
        </property>
        <property>
                <name>yarn.resourcemanager.hostname</name>
                <value>nn1</value>
        </property>
        <property>
                <name>yarn.nodemanager.vmem-check-enabled</name>
                <value>false</value>
        </property>
</configuration>
```

- mapred-site.xml 파일 편집

: MapReduce 어플리케이션 설정 파일이다.

```bash
# EC2 Ubuntu Terminal

# Hadoop mapred-site.xml 파일 설정
sudo vim $HADOOP_HOME/etc/hadoop/mapred-site.xml

# 아래 내용으로 수정 후 저장
<configuration>
        <property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
        </property>
        <property>
                <name>yarn.app.mapreduce.am.env</name>
                <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
        </property>
        <property>
                <name>mapreduce.map.env</name>
                <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
        </property>
        <property>
                <name>mapreduce.reduce.env</name>
                <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
        </property>
</configuration>
```

- [hadoop-env.sh](http://hadoop-env.sh) 파일 편집

```bash
# EC2 Ubuntu terminal

# Hadoop hadoop-env.sh 파일 설정
sudo vim $HADOOP_HOME/etc/hadoop/hadoop-env.sh

~~~~# 아래 내용 수정 후 저장
# Java
export JAVA_HOME=/usr/lib/jvm/java

~~~~# Hadoop
export HADOOP_HOME=/usr/local/hadoop

##필요에 따라 추가해주어야 하는 환경변수
export HDFS_NAMENODE_USER=root
export HDFS_DATANODE_USER=root
export HDFS_SECONDARYNAMENODE_USER=root
export YARN_RESOURCEMANAGER_USER=root
export YARN_NODEMANAGER_USER=root
export HDFS_JOURNALNODE_USER=root
export HDFS_ZKFC_USER=root
```

- Hadoop workers 편집

: Hadoop의 worker로 동작할 서버 호스트 이름을 설정한다.

```bash
# EC2 Ubuntu terminal

# Hadoop workers 편집
sudo vim $HADOOP_HOME/etc/hadoop/workers

# 아래 내용 수정 후 저장
# localhost << 주석 처리 또는 제거
dn1
dn2
dn3
```

- Hadoop masters 편집

: Hadoop의 master로 동작할 서버 호스트 이름을 설정한다.

```bash
# EC2 Ubuntu terminal

# Hadoop masters 편집
sudo vim $HADOOP_HOME/etc/hadoop/masters

# 아래 내용 수정 후 저장
nn1
nn2
```

 

## Apache Spark 3.2.1를 설치하고 환경설정을 진행

스파크 클러스터를 사용하기 위해서는 [spark-env.sh](http://spark-env.sh/), spark-defaults.conf, workers 를 편집하면 된다.

## [ Spark 설치 ]

- Apache Spark 3.2.1 설치 및 압축 해제

```bash
# EC2 Ubuntu terminal

# 설치 관리용 디렉토리 이동
cd /install_dir
# Spark 3.2.1 설치
sudo wget https://dlcdn.apache.org/spark/spark-3.2.1/spark-3.2.1-bin-hadoop3.2.tgz
# Spark 3.2.1 압축 해제
sudo tar -xzvf spark-3.2.1-bin-hadoop3.2.tgz -C /usr/local
# Spark 디렉토리 이름 변경
sudo mv /usr/local/spark-3.2.1-bin-hadoop3.2 /usr/local/spark
```

## [ Python & PySpark 설치 ]

- Python3 설치 및 파이썬 라이브러리 설치

```bash
# EC2 Ubuntu terminal

# Python 설치
sudo apt-get install -y python3-pip
# Python 버전 확인
python3 -V
# PySpark 설치
sudo pip3 install pyspark findspark
```

## [ Spark 환경설정 ]

```bash
# EC2 Ubuntu terminal

# Hadoop 시스템 환경변수 설정
sudo vim /etc/environment

# 아래 내용 추가 후 저장
PATH 뒤에 ":/usr/local/spark/bin" 추가
PATH 뒤에 ":/usr/local/spark/sbin" 추가
SPARK_HOME="/usr/local/spark"

# 시스템 환경변수 활성화
source /etc/environment

#  Spark 사용자 환경변수 설정
echo 'export SPARK_HOME=/usr/local/spark' >> ~/.bashrc

# 사용자 환경변수 활성화
source ~/.bashrc
```

- spark-env.sh 파일 편집

```bash
# EC2 Ubuntu terminal

# spark-env.sh 파일 카피
cd $SPARK_HOME/conf
sudo cp spark-env.sh.template spark-env.sh

# spark-env.sh 파일 편집
sudo vim spark-env.sh

# 아래 내용 수정 후 저장
export SPARK_HOME=/usr/local/spark
export SPARK_CONF_DIR=/usr/local/spark/conf
export JAVA_HOME=/usr/lib/jvm/java
export HADOOP_HOME=/usr/local/hadoop
export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
export SPARK_MASTER_WEBUI_PORT=18080
```

- spark-defaults.conf 파일 편집

```bash
# EC2 Ubuntu terminal

# Spark spark-defaults.conf.template 파일 복사
sudo cp /usr/local/spark/conf/spark-defaults.conf.template /usr/local/spark/conf/spark-defaults.conf

# Spark spark-defaults.conf 파일 설정
sudo vim /usr/local/spark/conf/spark-defaults.conf

# 아래 설정 후 저장 
# 클러스터 매니저 정보
spark.master              yarn
# 스파크 이벤트 로그 수행 유무
# true시 spark.eventLog.dir에 로깅 경로 지정해야합니다 - 스파크 UI에서 확인 가능합니다.
spark.eventLog.enabled    true
# 스파크 이벤트 로그 저장 경로
spark.eventLog.dir        /usr/local/spark/logs
```

- Spark logs 디렉토리 생성

```bash
# EC2 Ubuntu terminal

sudo mkdir -p /usr/local/spark/logs && sudo chown -R $USER:$USER /usr/local/spark/
```

- workers 파일 편집

: HDFS의 workers 를 설장 하였던 것과 같이, Spark 의 workers도 설정한다.(단, localhost는 주석 처리한다.)

```bash
# EC2 Ubuntu terminal

# Spark workers 파일 생성
sudo cp /usr/local/spark/conf/workers.template /usr/local/spark/conf/workers

# Spark workers 파일 설정
sudo vim /usr/local/spark/conf/workers

# 아래 설정 후 저장
dn1
dn2
dn3
```

## [ Python 환경설정 ]

- Python 환경 변수 설정

```bash
# EC2 Ubuntu terminal

# 시스템 환경변수 편집
sudo vim /etc/environment

# 아래 내용 추가 후 저장
PATH 뒤에 ":/usr/bin/python3" 추가

# 시스템 환경변수 활성화
source /etc/environment

#  Python & PySpark 사용자 환경변수 설정
sudo echo 'export PYTHONPATH=/usr/bin/python3' >> ~/.bashrc
sudo echo 'export PYSPARK_PYTHON=/usr/bin/python3' >> ~/.bashrc

# 사용자 환경변수 활성화
source ~/.bashrc
```

## Apache Zookeeper 3.8.0를 설치하고 환경설정을 진행

주키퍼 클러스터를 사용하기 위해서는 zoo.cfg, myid를 편집

## [ Zookeeper 설치 ]

- Apache Zookeeper 3.8.0 설치 및 압축 해제

```bash
# EC2 Ubuntu terminal

# 설치 관리용 디렉토리 이동
cd /install_dir
# Zookeeper 3.8.0 설치
sudo wget https://dlcdn.apache.org/zookeeper/zookeeper-3.8.0/apache-zookeeper-3.8.0-bin.tar.gz
# Zookeeper 3.8.0 압축 해제
sudo tar -xzvf apache-zookeeper-3.8.0-bin.tar.gz -C /usr/local
# Zookeeper 디렉토리 이름 변경
sudo mv /usr/local/apache-zookeeper-3.8.0-bin /usr/local/zookeeper
```

## [ Zookeeper 환경설정 ]

- Zookeeper 환경 변수 설정

```bash
# EC2 Ubuntu terminal

# Hadoop 시스템 환경변수 설정
sudo vim /etc/environment

# 아래 내용 추가 후 저장
ZOOKEEPER_HOME="/usr/local/zookeeper"

# 시스템 환경변수 활성화
source /etc/environment

#  Spark 사용자 환경변수 설정
echo 'export ZOOKEEPER_HOME=/usr/local/zookeeper' >> ~/.bashrc

# 사용자 환경변수 활성화
source ~/.bashrc
```

- zoo.cfg 파일 편집

```bash
# EC2 Ubuntu terminal

# Zookeeper 설정 경로 이동
cd /usr/local/zookeeper
# Zookeeper 설정 파일 복사
sudo cp ./conf/zoo_sample.cfg ./conf/zoo.cfg 

# zoo.cfg 편집
sudo vim ./conf/zoo.cfg

# 아래 내용 수정 후 저장
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/usr/local/zookeeper/data
dataLogDir=/usr/local/zookeeper/logs
clientPort=2181
maxClientCnxns=0
maxSessionTimeout=180000
server.1=nn1:2888:3888
server.2=nn2:2888:3888
server.3=dn1:2888:3888
```

- myid 설정

```bash
# EC2 Ubuntu terminal

# Zookeeper 데이터 디렉토리 생성
sudo mkdir -p /usr/local/zookeeper/data
sudo mkdir -p /usr/local/zookeeper/logs

# Zookeeper 디렉토리 사용자 그룹 변경
sudo chown -R $USER:$USER /usr/local/zookeeper

# myid 파일 편집
sudo vim /usr/local/zookeeper/data/myid

# 아래 내용 수정 후 저장
1
```

## [ SSH 설정 ]

- ssh key 생성

```bash
# EC2 Ubuntu terminal

# ssh key 생성
ssh-keygen -t rsa # 이후 Enter만 세 번 입력 탁! 탁! 탁!

# authorized_keys 생성
cat >> ~/.ssh/authorized_keys < ~/.ssh/id_rsa.pub

# localhost 접속 테스트
ssh localhost
# Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

## [ Docker network 설정 ]

: 특정 네트워크 대역을 지정하여 도커 네트워크를 생성한다.

```bash
docker network create --gateway 172.19.0.1 --subnet 172.19.0.0/16 testnet
```

- Docker network 리스트 확인

```bash
docker network ls
```

- Docker network 정보 확인

```bash
docker inspect testnet
```

- Docker 컨테이너 1번 생성

```bash
docker run \
-it -d \
-p 18888:18888
--network testnet \
--ip 172.19.0.11 \
--hostname nn1\
--name nn1 \
ubuntu:latest \
/bin/bash
```

- Docker 컨테이너 2번 생성

```bash
docker run \
-it -d \
--network testnet \
--ip 172.19.0.12 \
--hostname nn2\
--name nn2 \
ubuntu:latest \
/bin/bash

...

# ~ip 172.19.0.15(dn1, dn2, dn3)까지 만들어준다
```

- Docker 컨테이너 호스트 이름 확인

: server01 ~ 02 컨테이너에 접속하여 각각 호스트이름이 정상적으로 지정 되었는지 확인한다.

```bash
docker exec -it server01 /bin/bash

$ cat /etc/hostname
```

---

### [ server01 ~ 02 컨테이너 모두 하위 작업 반복 ]

- Ubuntu apt-get 업데이트 및 라이브러리 설치

: 이 때 openssh-server 설치 중 time zone은 아시아/서울로 설정하였다. (주석 참고)

```bash
$ apt-get update && \
apt-get upgrade -y && \
apt-get install -y curl &&\
apt-get install -y openssh-server openssh-client &&\
apt-get install -y rsync wget vim iputils-ping htop

# time zone 설정
# 6. Asia
# 69. Seoul
```

- SSH KEY 생성

```bash
$ ssh-keygen -t rsa
*# Enter 세 번 탁!탁!탁!*
```

- SSH KEY를 인증 KEY로 복사

```bash
$ cp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys
```

- SSH 디렉토리 확인

```bash
$ cd ~/.ssh && ls
>>> authorized_keys  id_rsa  id_rsa.pub
```

- SSH 인증키 확인

```bash
$ cat authorized_keys

# ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDGFz2na47vkmzxeqISPycd0HzySVCFengqBUji9VlzYOb6gf155GrPU/iX8FPzikD0ziVG9RYagmvtEyiegzCtMQRoomnm1QYz9JLjYjf6hxQZLnKEIpK+fC7n+Jxn7iEs7jyC/zf0uqQyirPfyTlNvcoY1z7pb0rfxVNT7oCL8FhVCFFCBtFpphnK5dCv0J15cqr+NN7eo7OfHvDqVBKD4LEhS3hpQLP3meB/YCdqw6WEi6lrb6uUvyIftKe65Jmm/B5WqTI4dJUhFCtyIXqpyYRWdXRubb0QEXIPvQk2sxHBlaYTyacdJD5B5+uaAt083eqwNBuJhtkpLTAHPgwz2ThwixGSl1i+pJuLEhSK3vcZXJvC8TgU6UfEI3wDejkTtCDtRMj9HIjulLjBBJ+/+O0ms8+TpkGX0lUvIoE31jdBN/Otg3GAn+WGtsAY61Fcz1cwGx6o/MAL99FBXXljYkXyKYZ82IHBJn3mbf4NRYM90R0MoTFLUnB0EN0a+m8= root@server01
```

---

- 통신할 Docker 컨테이너끼리 SSH 인증키 등록

예 : server01 과 server02 컨테이너가 양방향 SSH 인증 통신을 하기 위해서 server01의 인증키를 server02에 등록하고, server02의 인증키를 server01에 등록한다.

- server01 컨테이너

```bash
$ cd ~/.ssh
$ vim authorized_keys
"""
# server01
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDGFz2na47vkmzxeqISPycd0HzySVCFengqBUji9VlzYOb6gf155GrPU/iX8FPzikD0ziVG9RYagmvtEyiegzCtMQRoomnm1QYz9JLjYjf6hxQZLnKEIpK+fC7n+Jxn7iEs7jyC/zf0uqQyirPfyTlNvcoY1z7pb0rfxVNT7oCL8FhVCFFCBtFpphnK5dCv0J15cqr+NN7eo7OfHvDqVBKD4LEhS3hpQLP3meB/YCdqw6WEi6lrb6uUvyIftKe65Jmm/B5WqTI4dJUhFCtyIXqpyYRWdXRubb0QEXIPvQk2sxHBlaYTyacdJD5B5+uaAt083eqwNBuJhtkpLTAHPgwz2ThwixGSl1i+pJuLEhSK3vcZXJvC8TgU6UfEI3wDejkTtCDtRMj9HIjulLjBBJ+/+O0ms8+TpkGX0lUvIoE31jdBN/Otg3GAn+WGtsAY61Fcz1cwGx6o/MAL99FBXXljYkXyKYZ82IHBJn3mbf4NRYM90R0MoTFLUnB0EN0a+m8= root@server01

## server02
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCYNzyBRDNXlthOa+GX37jUk/OCAKex520ckfxohYKNqGo0tP0vb2eTXKNXpECdp7wxI89/YUuPdcnyUF4Lu7kjuQ5MvSwg2hLaLe8gNiaU50YpnePHL4zkHAJ7DdWmKNqYmWpXJ8cRSpKp4vYhi9L9MZ7FPYYtvAYm8Lr56+dT2ea73Z/mJ5LnZRYdZRTtR9DN6/OQEQU0WQQvV1C3Nrs8O2MwSIbfrVyrYQ6gDt0DTzusp8d92vFiHsbhstsFnv4yWQbS4oL+KXUUKkGsZnFrS7ChuF0fSsSuxQc3ycg2Q5h/xzmyp22EVSeJRNGrQvmGNIjP278SuDfPhTm7t0VXT0bhNlo9K8ih27Kj66SseHp4ept0QrnxDSZyec1pE5kkHnOVm9kCCZrNklUUQ5JOmXDGb2bxBgoZurefYikztUzyVCv6rYG5K6D5DxC0ukAg9cOAPBl5MBqKkW5mxeEaNKAXbLFO34FDGwPqNs4eW5IhxCGND2AJo/teBO7gnG0= root@server02
"""

## server03 ... server05
```

- server02 컨테이너

```bash
$ cd ~/.ssh
$ vim authorized_keys
"""
# server01
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDGFz2na47vkmzxeqISPycd0HzySVCFengqBUji9VlzYOb6gf155GrPU/iX8FPzikD0ziVG9RYagmvtEyiegzCtMQRoomnm1QYz9JLjYjf6hxQZLnKEIpK+fC7n+Jxn7iEs7jyC/zf0uqQyirPfyTlNvcoY1z7pb0rfxVNT7oCL8FhVCFFCBtFpphnK5dCv0J15cqr+NN7eo7OfHvDqVBKD4LEhS3hpQLP3meB/YCdqw6WEi6lrb6uUvyIftKe65Jmm/B5WqTI4dJUhFCtyIXqpyYRWdXRubb0QEXIPvQk2sxHBlaYTyacdJD5B5+uaAt083eqwNBuJhtkpLTAHPgwz2ThwixGSl1i+pJuLEhSK3vcZXJvC8TgU6UfEI3wDejkTtCDtRMj9HIjulLjBBJ+/+O0ms8+TpkGX0lUvIoE31jdBN/Otg3GAn+WGtsAY61Fcz1cwGx6o/MAL99FBXXljYkXyKYZ82IHBJn3mbf4NRYM90R0MoTFLUnB0EN0a+m8= root@server01

## server02
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCYNzyBRDNXlthOa+GX37jUk/OCAKex520ckfxohYKNqGo0tP0vb2eTXKNXpECdp7wxI89/YUuPdcnyUF4Lu7kjuQ5MvSwg2hLaLe8gNiaU50YpnePHL4zkHAJ7DdWmKNqYmWpXJ8cRSpKp4vYhi9L9MZ7FPYYtvAYm8Lr56+dT2ea73Z/mJ5LnZRYdZRTtR9DN6/OQEQU0WQQvV1C3Nrs8O2MwSIbfrVyrYQ6gDt0DTzusp8d92vFiHsbhstsFnv4yWQbS4oL+KXUUKkGsZnFrS7ChuF0fSsSuxQc3ycg2Q5h/xzmyp22EVSeJRNGrQvmGNIjP278SuDfPhTm7t0VXT0bhNlo9K8ih27Kj66SseHp4ept0QrnxDSZyec1pE5kkHnOVm9kCCZrNklUUQ5JOmXDGb2bxBgoZurefYikztUzyVCv6rYG5K6D5DxC0ukAg9cOAPBl5MBqKkW5mxeEaNKAXbLFO34FDGwPqNs4eW5IhxCGND2AJo/teBO7gnG0= root@server02
"""

## server03 ... server05
```

- SSH 서비스 활성화(모든 컨테이너에서 실행)

```bash
service ssh start # 이미 실행 되어 있다면 restart
```

- 통신 테스트

```bash
# server01 에서
$ ssh server02

# server02 에서
$ ssh server01
```

- SSH 디렉토리 권한 설정(모든 컨테이너에서 실행)

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub  
chmod 644 ~/.ssh/authorized_keys
chmod 644 ~/.ssh/known_hosts
```

- SSH 서비스 자동 실행 등록(모든 컨테이너에서 실행)

```bash
vim ~/.bashrc

# 아래 내용을 파일 맨 하단에 기입하고 저장
```
service ssh start
```

source ~/.bashrc
```

## [ Hosts 및 Hostname 설정 ]

- nn1 서버 hosts 파일 편집

: nn1 서버로 접속해서 hosts 파일에 각 서버의 Private IP와 hostname을 등록한다.

```bash
# My Mac terminal

# 로컬에서 nn1 서버 접속
ssh nn1
```

```bash
# EC2 Ubuntu terminal(nn1)

# hosts 파일 편집
sudo vim /etc/hosts

# 아래 내용으로 추가 후 저장
172.19.0.11 nn1
172.19.0.12 nn2
172.19.0.13 dn1
172.19.0.14 dn2
172.19.0.15 dn3
```

- 모든 인스턴스에 Hosts 파일 복제

: nn1 에서만 진행한다. (수동으로 내용 복붙가능)

```bash
# EC2 Ubuntu terminal(nn1)

# 복제
cat /etc/hosts | ssh nn2 "sudo sh -c 'cat >/etc/hosts'"
cat /etc/hosts | ssh dn1 "sudo sh -c 'cat >/etc/hosts'"
cat /etc/hosts | ssh dn2 "sudo sh -c 'cat >/etc/hosts'"
cat /etc/hosts | ssh dn3 "sudo sh -c 'cat >/etc/hosts'"
```

- hdfs-site.xml 파일 복제

: nn1 에서만 진행한다. (수동으로 내용 복붙가능)

```bash
# EC2 Ubuntu terminal(nn1)

# 복제
cat $HADOOP_HOME/etc/hadoop/hdfs-site.xml | ssh nn2 "sudo sh -c 'cat >$HADOOP_HOME/etc/hadoop/hdfs-site.xml'"
```

## Zookeeper 클러스터 실행

## [ Zookeeper 클러스터 설정 ]

- Zookeeper myid 파일 편집

: nn1, nn2, dn1 서버에서 myid를 각각 1, 2, 3으로 편집한다. nn1 서버는 이미 지정했기 때문에 nn2, dn1에서 진행하면 된다.

```bash
# EC2 Ubuntu terminal(nn1)

# nn2 서버로 이동
ssh nn2
sudo vim /usr/local/zookeeper/data/myid
# 아래 내용으로 수정 후 저장
2
# nn1으로 이동
exit

# dn1 서버로 이동
ssh dn1
sudo vim /usr/local/zookeeper/data/myid
# 아래 내용으로 수정 후 저장
3
# nn1으로 이동
exit
```

- Zookeeper 실행

: nn1, nn2, dn1 서버에서 각각 실행한다.

```bash
# EC2 Ubuntu terminal(nn1)

# nn1 zookeeper 시작
sudo /usr/local/zookeeper/bin/zkServer.sh start

# nn2 zookeeper 시작
ssh nn2
sudo /usr/local/zookeeper/bin/zkServer.sh start
exit

# dn1 zookeeper 시작
ssh dn1
sudo /usr/local/zookeeper/bin/zkServer.sh start
exit
```

- Zookeeper 상태 확인

: nn1, nn2, dn1 서버에서 각각 실행한다.

```bash
# EC2 Ubuntu terminal(nn1)

# nn1 zookeeper 시작
sudo /usr/local/zookeeper/bin/zkServer.sh status

# nn2 zookeeper 시작
ssh nn2
sudo /usr/local/zookeeper/bin/zkServer.sh status
exit

# dn1 zookeeper 시작
ssh dn1
sudo /usr/local/zookeeper/bin/zkServer.sh status
exit
```

- HDFS ZKFC 초기화

: nn1 에서만 진행한다.

```bash
# EC2 Ubuntu terminal(nn1)

source /etc/environment
# zkfc 초기화
hdfs zkfc -formatZK
```

- HDFS ZKFC 초기화 확인

: nn1 에서만 진행한다.

```bash
# EC2 Ubuntu terminal(nn1)

# zkCli 실행
cd /usr/local/zookeeper
./bin/zkCli.sh

# Hadoop 클러스터 확인
ls /hadoop-ha

# [my-hadoop-cluster] 확인 후 quit 명령으로 종료

# 종료
quit
```

- Journalnode 실행

: nn1, nn2, dn1 에서 실행한다.

```bash
# EC2 Ubuntu terminal(nn1)

# nn1에서 실행
hdfs --daemon start journalnode

# nn2에서 실행
ssh nn2
hdfs --daemon start journalnode
exit

# dn1에서 실행
ssh dn1
hdfs --daemon start journalnode
exit
```

## Hadoop & Yarn 클러스터 실행

## [ Namenode ]

- NameNode 초기화

: nn1 에서만 실행한다.

```bash
# EC2 Ubuntu terminal(nn1)

# hdfs namenode 포맷
hdfs namenode -format
```

- NameNode 실행

: nn1 에서만 실행한다.

```bash
# EC2 Ubuntu terminal(nn1)

# hdfs namenode 실행
hdfs --daemon start namenode
```

## [ Standby NameNode ]

- Standby NameNode 실행

: nn2 에서만 실행한다.

```bash
# EC2 Ubuntu terminal(nn2)

# hdfs standby namenode 실행
ssh nn2
hdfs namenode -bootstrapStandby
```

## [ Hadoop 실행 ]

- [start-dfs.sh](http://start-dfs.sh) 실행

: nn1 에서만 실행한다. 해당 단계에서 “DFSZKFailoverController” 프로세스가 실행 된다.

```bash
# EC2 Ubuntu terminal(nn1)

start-dfs.sh
```

## [ Yarn 실행 ]

- [start-yarn.sh](http://start-yarn.sh) 실행

: nn1 에서만 실행한다. 해당 단계에서 “ResourceManager” 프로세스가 실행된다. 나머지 DataNode 서버에서는 “NodeManager” 프로세스가 실행된다.

```bash
# EC2 Ubuntu terminal(nn1)

start-yarn.sh
```

## [ JobHistory 실행 ]

- historyserver 실행

: nn1 에서만 실행한다. 해당 단계에서 “JobHistoryServer” 프로세스가 실행된다.

```bash
# EC2 Ubuntu terminal(nn1)

mapred --daemon start historyserver
```

- Active, Standby NameNode 확인

```bash
# EC2 Ubuntu terminal(nn1)

hdfs haadmin -getServiceState namenode1 
hdfs haadmin -getServiceState namenode2
```

![Untitled](%E1%84%87%E1%85%B5%E1%86%A8%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20%E1%84%87%E1%85%AE%E1%86%AB%E1%84%89%E1%85%A5%E1%86%A8%20%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%E1%86%BC%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%20eb0b4a24b530437aaf628ef3f5b404cb/Untitled.png)

- Hadoop Word Count 예제 테스트

: nn1 에서 진행한다.

```bash
# EC2 Ubuntu terminal(nn1)

# HDFS test 디렉토리 생성
hdfs dfs -mkdir /test
# HDFS LICENSE.txt 파일을 test 디렉토리에 삽입
hdfs dfs -put /usr/local/hadoop/LICENSE.txt /test/
# Word Count 예제 실행
yarn jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.3.jar wordcount hdfs:///test/LICENSE.txt /test/output
# Worn Count 결과 확인
hdfs dfs -text /test/output/*
```

## 지금까지 설정한 Hadoop, Yarn, Spark, Zookeeper 를 모두 start, stop, restart 할 수 있는 스크립트를 생성

## [ 쉘 스크립트 생성 ]

- cluster-start-all.sh

: Hadoop, Yarn, Spark, Zookeeper 를 모두 실행 시키는 쉘 스크립트를 생성한다.

```bash
# EC2 Ubuntu terminal

# 쉘 스크립트 편집
vim cluster-start-all.sh

# 아래 내용 추가 후 저장
# nn1 Zookeeper Run
sudo /usr/local/zookeeper/bin/zkServer.sh start

# nn2 Zookeeper Run
ssh nn2 "sudo /usr/local/zookeeper/bin/zkServer.sh start"

# dn1 Zookeeper Run
ssh dn1 "sudo /usr/local/zookeeper/bin/zkServer.sh start"

# Hadoop Run
$HADOOP_HOME/sbin/start-all.sh

# Jobhistoryserver Run
mapred --daemon start historyserver

# Spark Run
$SPARK_HOME/sbin/start-all.sh

# Zeppelin --> Zeppelin 설치 후 추가할 것
/usr/local/zeppelin/bin/zeppelin-daemon.sh start

# 쉡 스크립트 접근 권한 설정
sudo chmod 777 cluster-start-all.sh
```

- cluster-stop-all.sh

: Hadoop, Yarn, Spark, Zookeeper 를 모두 중단 시키는 쉘 스크립트를 생성한다.

```bash
# EC2 Ubuntu terminal

# 쉘 스크립트 편집
vim cluster-stop-all.sh

# 아래 내용 추가 후 저장
# Zeppelin --> Zeppelin 설치 후 추가할 것
/usr/local/zeppelin/bin/zeppelin-daemon.sh stop

# Spark stop
$SPARK_HOME/sbin/stop-all.sh

# Jobhistory stop
mapred --daemon stop historyserver

# nn1 Zookeeper stop
sudo /usr/local/zookeeper/bin/zkServer.sh stop

# nn2 Zookeeper stop
ssh nn2 "sudo /usr/local/zookeeper/bin/zkServer.sh stop"

# dn1 Zookeeper stop
ssh dn1 "sudo /usr/local/zookeeper/bin/zkServer.sh stop"

# Hadoop stop
$HADOOP_HOME/sbin/stop-all.sh

# 쉡 스크립트 접근 권한 설정
sudo chmod 777 cluster-stop-all.sh
```

- cluster-restart-all.sh

: Hadoop, Yarn, Spark, Zookeeper 를 모두 재실행 시키는 쉘 스크립트를 생성한다.

```bash
 # EC2 Ubuntu terminal

# 쉘 스크립트 편집
vim cluster-restart-all.sh

# 아래 내용 추가 후 저장
# Zeppelin --> Zeppelin 설치 후 추가할 것
/usr/local/zeppelin/bin/zeppelin-daemon.sh stop

# Spark stop
$SPARK_HOME/sbin/stop-all.sh

# Jobhistory stop
mapred --daemon stop historyserver

# nn1 Zookeeper stop
sudo /usr/local/zookeeper/bin/zkServer.sh stop

# nn2 Zookeeper stop
ssh nn2 "sudo /usr/local/zookeeper/bin/zkServer.sh stop"

# dn1 Zookeeper stop
ssh dn1 "sudo /usr/local/zookeeper/bin/zkServer.sh stop"

# Hadoop stop
$HADOOP_HOME/sbin/stop-all.sh

# nn1 Zookeeper Run
sudo /usr/local/zookeeper/bin/zkServer.sh start

# nn2 Zookeeper Run
ssh nn2 "sudo /usr/local/zookeeper/bin/zkServer.sh start"

# dn1 Zookeeper Run
ssh dn1 "sudo /usr/local/zookeeper/bin/zkServer.sh start"

# Hadoop Run
$HADOOP_HOME/sbin/start-all.sh

# Jobhistoryserver Run
mapred --daemon start historyserver

# Spark Run
$SPARK_HOME/sbin/start-all.sh

# Zeppelin --> Zeppelin 설치 후 추가할 것
/usr/local/zeppelin/bin/zeppelin-daemon.sh start

# 쉡 스크립트 접근 권한 설정
sudo chmod 777 cluster-restart-all.sh
```

## [ Systemctl 등록 방법 ]

```bash
# EC2 Ubuntu terminal

# 실행 파일 편집
vim hadoop-cluster-run.sh

# 아래 내용 기재 후 저장
#!/bin/bash
lines="==========================================================="

case "$1" in
        start)
                # Zookeeper 실행
                ssh nn1 'sudo /usr/local/zookeeper/bin/zkServer.sh start'
                ssh nn2 'sudo /usr/local/zookeeper/bin/zkServer.sh start'
                ssh dn1 'sudo /usr/local/zookeeper/bin/zkServer.sh start'
                echo $lines

                # Hadoop 실행
                /usr/local/hadoop/sbin/start-all.sh
                echo $lines

                # JobHistoryServer 실행
                /usr/local/hadoop/bin/mapred --daemon start historyserver
                echo $lines

                # Spark 실행
                /usr/local/spark/sbin/start-all.sh
                echo $lines

                # Zeppelin 실행
                /usr/local/zeppelin/bin/zeppelin-daemon.sh start
                echo $lines
                ;;
        stop)
								# Zeppelin 중지
                /usr/local/zeppelin/bin/zeppelin-daemon.sh stop
                echo $lines

                # Spark stop 중지
                /usr/local/spark/sbin/stop-all.sh
                echo $lines

                # Jobhistory 중지
                /usr/local/hadoop/bin/mapred --daemon stop historyserver
                echo $lines

               # Hadoop 중지
                /usr/local/hadoop/sbin/stop-all.sh
                echo $lines

                # Zookeeper 중지
                ssh nn1 'sudo /usr/local/zookeeper/bin/zkServer.sh stop'
                ssh nn2 'sudo /usr/local/zookeeper/bin/zkServer.sh stop'
                ssh dn1 'sudo /usr/local/zookeeper/bin/zkServer.sh stop'
                echo $lines

                ;;
esac
```

```bash
# EC2 Ubuntu terminal

# 시스템 파일 편집
sudo vim /etc/systemd/system/hadoop-cluster.service

# 아래 내용 기재 후 저장
Description=Hadoop Cluster Service

[Service]
Type=oneshot
User=ubuntu
Group=ubuntu

ExecStart=/home/ubuntu/proc_manage/hadoop-cluster-run.sh start
ExecStop=/home/ubuntu/proc_manage/hadoop-cluster-run.sh stop
RemainAfterExit=yes
#Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
# EC2 Ubuntu terminal

# system 등록 및 시작
sudo systemctl enable hadoop-cluster.service
sudo systemctl start hadoop-cluster.service
```

- systemctl log 확인

```bash
# EC2 Ubuntu terminal

journalctl -u hadoop-cluster.service -b
```

## [ Zeppelin 설치 ]

지금까지 설정한 Hadoop, Yarn, Spark, Zookeeper 클러스터 환경에 Zeppelin을 연동하기 위해 설치 및 환경설정을 진행한다.

- Zeppelin 0.10.1 설치

```bash
# EC2 Ubuntu terminal

# 디렉토리 이동
cd /install_dir

# Zeppelin 다운로드
sudo wget https://dlcdn.apache.org/zeppelin/zeppelin-0.10.1/zeppelin-0.10.1-bin-all.tgz

# Zeppelin 압축 해제
sudo tar -zxvf zeppelin-0.10.1-bin-all.tgz -C /usr/local/

# Zeppelin 디렉토리 이름 변경
sudo mv /usr/local/zeppelin-0.10.1-bin-all/ /usr/local/zeppelin

# Zeppelin 디렉토리 소유자 변경
sudo chown -R $USER:$USER /usr/local/zeppelin
```

## [ Zeppelin 환경설정 ]

- Zeppelin 환경 변수 설정

```bash
# EC2 Ubuntu terminal

# 시스템 환경변수 편집
sudo vim /etc/environment

# 아래 내용 추가 후 저장
PATH 뒤에 ":/usr/local/zeppelin/bin" 추가
ZEPPELIN_HOME="/usr/local/zeppelin"

# 시스템 환경변수 활성화
source /etc/environment

# 사용자 환경변수 편집
sudo echo 'export ZEPPELIN_HOME=/usr/local/zeppelin' >> ~/.bashrc

# 사용자 환경변수 활성화
source ~/.bashrc
```

- zeppelin-site.xml 파일 설정

```bash
# EC2 Ubuntu termianl

# Zeppelin 환경설정 디렉토리 이동
cd /usr/local/zeppelin/conf

# zeppelin-site.xml 파일 복사
cp zeppelin-site.xml.template zeppelin-site.xml

# zeppelin-site.xml 파일 편집
vim zeppelin-site.xml

# 아래 내용 수정 후 저장
<property>
  <name>zeppelin.server.addr</name>
  <value>0.0.0.0</value>
  <description>Server binding address</description>
</property>

<property>
  <name>zeppelin.server.port</name>
  <value>18888</value>
  <description>Server port.</description>
</property>
```

- [zeppelin-env.sh](http://zeppelin-env.sh) 파일 설정

```bash
# EC2 Ubuntu terminal

# zeppelin-env.sh 파일 복사
cp zeppelin-env.sh.template zeppelin-env.sh

# zeppelin-env.sh 파일 편집
vim zeppelin-env.sh

# 아래 내용 수정 후 저장
export JAVA_HOME=/usr/lib/jvm/java
export HADOOP_HOME=/usr/local/hadoop
export YARN_CONF_DIR=$HADOOP_HOME/etc/hadoop
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export SPARK_HOME=/usr/local/spark
export SPARK_MASTER=yarn
export ZEPPELIN_PORT=18888
export PYTHONPATH=/usr/bin/python3
export PYSPARK_PYTHON=/usr/bin/python3
export PYSPARK_DRIVER_PYTHON=/usr/bin/python3
```

```bash
/usr/local/zeppelin/bin/zeppelin-daemon.sh start

# Zeppelin 실행
Zeppelin start                                             [  OK  ]
```

## [ Zeppelin WEB UI ]

- Zeppelin WEB UI 확인

localhost:18888  (기본 계정 : admin / admin)

- Zeppelin WEB UI 환경설정

: Zeppelin WEB UI의 인터프립터를 편집한다. 우측 상단에서 “interpreter” 클릭 후 검색창에 “spark”를 검색하고 우측에 있는 “edit” 버튼을 눌러 편집창을 실행하고 “spark.submit.deployMode” 값을 “cluster” 로 바꾼다. 그리고 “PYSPARK_DRIVER_PYTHON” 값을 python에서 “/usr/bin/python3” 으로 바꾼다

## [ PySpark 예제 테스트 ]

- Create new note

: Zeppelin 홈 화면에서 “Create new note” 클릭 후 Note Name을 “/jmkim/zeppelin_pyspark_example.py”로 지정하고 Default Interpreter를 “spark-submit”으로 설정한다.

[KC_KOBIS_BOX_OFFIC_MOVIE_INFO_202105.csv](%E1%84%87%E1%85%B5%E1%86%A8%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20%E1%84%87%E1%85%AE%E1%86%AB%E1%84%89%E1%85%A5%E1%86%A8%20%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%E1%86%BC%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%20eb0b4a24b530437aaf628ef3f5b404cb/KC_KOBIS_BOX_OFFIC_MOVIE_INFO_202105.csv)

- Zeppelin note 실행

: 셀에 아래와 같이 내용 입력 후 실행한다.

```bash
%spark.pyspark

from pyspark.sql import SparkSession
from pyspark.sql.functions import col

sc = SparkSession.builder\
        .master("yarn")\
        .appName("Jmkim Test")\
        .getOrCreate()

df = sc.read.csv("hdfs:///test/KC_KOBIS_BOX_OFFIC_MOVIE_INFO_202105.csv", header=True)
df.select(col("MOVIE_NM"), col("MNG_NM"), col("IMPORT_CMPNY_NM"), col("GRAD_NM")).show()
```

```bash
%spark.pyspark

df.printSchema()
```

```bash
%spark.pyspark

df.createOrReplaceTempView("movie")

%spark.sql

select * from movie
```

```bash
%spark.sql

select MOVIE_NM, MNG_NM, DISTB_CMPNY_NM, OPEN_DE, GENRE_NM, GRAD_NM from movie
```

```bash
%spark.sql

select NLTY_NM, count(*) from movie group by NLTY_NM
```

dfs가 실행이 안될 때

```bash
#실행
source /etc/envirnment
```