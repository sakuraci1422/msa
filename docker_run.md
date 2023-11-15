# Base OS
## ubuntu
* Info : ubuntu 기본 이미지
* Usage :
```
docker run -it --rm                   \
    --name=u1                         \
    -v ~/df:/df           \
    -p 8000:8000                      \
    ubuntu
```
## centos
* Info : ubuntu 기본 이미지
* Usage :
```
docker run -it --rm                   \
    --name=c1                         \
    -v ~/df:/df           \
    -p 8000:8000                      \
    centos
```
# DataRepoitory & Repo
## Oracle 11
* command
```
sqlplus system/oracle
create user hr identified by hr;
grant dba to hr;
```
* Info :
* Link : https://hub.docker.com/r/sath89/oracle-xe-11g/
* Usage :
```
docker rm -f oracle
rm -rf ~/df/Oracle
mkdir ~/df/Oracle
docker run -d  --name=oracle                   \
    -p 8080:8080                               \
    -p 1521:1521                               \
    -v ~/df/Oracle:/u01/app/oracle \
    -e DBCA_TOTAL_MEMORY=2048                  \
    sath89/oracle-xe-11g
docker logs  -tf oracle
```

### Oracle+spark
* Info : from sath89/oracle-xe-11g
    * sath89/oracle-ee-12c는 checksum 문제로 docker save 명령 안되는 문제 있어서 해결 되기 전까지는 sath89/oracle-xe-11g 사용하기로 함.
* Usage : → to Try sath89/oracle-xe-11g
```
docker rm -f ospark;docker run -d  --name=ospark  \
    -p 8080:8080                                  \
    -p 9999:9999                                  \
    -p 1521:1521                                  \
    -h idb                                        \
    -v ~/df/Oracle:/u01/app/oracle    \
    -v ~/df/Spark:/work               \
    -v ~/df/Data:/data2               \
    -e DBCA_TOTAL_MEMORY=4096                     \
    -e WEB_CONSOLE=false                          \
    nowage/oracle_spark;docker exec -it ospark service ssh start
docker exec -it ospark /spark/spark-2.2.0/sbin/start-all.sh
docker logs  -tf ospark
```
    - 9999 port : Spark WebUI
    - 8080 port : Dictionary service


## Oracle12c : to try1
* hostname: localhost
* port: 1521
* sid: xe
* service name: xe
* username: system
* password: oracle
  * sqlplus system/oracle@//localhost:1521/xe
* Connect to Oracle Application Express web management console with following settings:
    * http://localhost:8080/apex
    * workspace: INTERNAL
    * user: ADMIN
    * password: 0Racle$ Ktnowage!1
* Link : https://hub.docker.com/r/sath89/oracle-12c/
* Usage :1
```
docker run -it -p 8080:8080 --name=oracle -p 1521:1521 sath89/oracle-12c
```
* command
```
sqlplus system/oracle
create user hr identified by hr;
grant dba to hr;
```
* Info :
* Usage :
```
docker rm -f oracle
rm -rf ~/df/Oracle
mkdir ~/df/Oracle
docker run -d  --name=oracle                   \
    -p 8080:8080                               \
    -p 1521:1521                               \
    -v ~/df/Oracle:/u01/app/oracle \
    -e DBCA_TOTAL_MEMORY=2048                  \
    sath89/oracle-12c
docker logs  -tf oracle
```
## Oracle12c : to try2
* Info
  * hostname: localhost
  * port: 1521
  * sid: xe
  * username: system
  * password: oracle
  * Password for SYS & SYSTEM:
  * Connect to Oracle Application Express web management console with
    * http://localhost:8080/apex
    * workspace: INTERNAL
    * user: ADMIN
    * password: 0Racle$
* Link : https://hub.docker.com/r/pengbai/docker-oracle-12c-r1/

```
docker rm -f oracle12c
docker run -d --name=oracle12c                          \
    -p 8081:8080                                        \
    -p 1522:1521                                        \
    -v ~/df/Oracle12c/:/entrypoint-initdb.d \
    pengbai/docker-oracle-12c-r1
docker logs  -tf oracle
```
* Usage :
```
docker rm -f ospark;docker run -d  --name=ospark  \
    -p 8080:8080                                  \
    -p 9999:9999                                  \
    -p 1521:1521                                  \
    -h idb                                        \
    -v ~/df/Oracle:/u01/app/oracle    \
    -v ~/df/Spark:/work               \
    -v ~/df/Data:/data2               \
    -e DBCA_TOTAL_MEMORY=4096                     \
    -e WEB_CONSOLE=false                          \
    nowage/oracle_spark;docker exec -it ospark service ssh start
docker exec -it ospark /spark/spark-2.2.0/sbin/start-all.sh
docker logs  -tf ospark
```

## Hadoop
* Info :
    * Hadoop : 2.7.1
* Link : https://github.com/sequenceiq/hadoop-docker
* Command
  /usr/local/hadoop-2.7.1/bin/hadoop
* Limitance  : /tmp 폴더에 데이터 저장되는데 Volume맵핑 안됨

* Usage :1
```
docker rm -f hadoop
docker run     -it                    \
    --name=hadoop                     \
    --hostname=h1                     \
    -v ~/df/Hadoop:/work  \
    -p 50070:50070                                    \
    -p 8020:8020                                      \
    -p 9000:9000                                      \
    -p 9922:22                                        \
    nowage/hadoop
    docker exec -it hadoop bash

```
* Usage :2 : Special Version
```
docker run -it --rm                                   \
    --name=hadoop                                     \
    -p 50070:50070                                    \
    -p 8020:8020                                      \
    -p 9000:9000                                      \
    -p 9922:22                                        \
    -v ~/df/Hadoop:/work  \
    sequenceiq/hadoop-docker:2.7.1                    \
    nowage/hadoop
```


## nowage/spark_mysql_p3
* Info : 생빌드, NCIAB프로젝트를 위해 만들어짐. (python2)
    *
* Link :
* Usage :
```
docker rm -f s3
docker run -it                      \
--name s3                           \
--hostname s3                       \
-v ~/df/Spark:/df       \
-p 8081:8081                        \
-p 8080:8080                        \
-p 7077:7077                        \
nowage/spark_mysql_p3:0.1
service ssh start
start-all.sh
pyspark --master spark://s3:7077

docker logs  -tf s3
```

* install Script from nowage/spark_mysql
```
apt-get -y update

apt-get install -y openssh-server
# apt-get install --fix-missing -y python-pip3
pip3 install pyspark findspark numpy pandas jinja2 cx_Oracle httplib2

ssh-keygen -t rsa -q -f ~/.ssh/id_rsa -N ''
cp -f ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys
chmod 600  ~/.ssh/authorized_keys


```

### nowage/spark_mysql
* Info : 생빌드, NCIAB프로젝트를 위해 만들어짐. (python2)
    *
* Link :
* Usage :
```
docker rm -f s2
docker run -it                      \
--name s2                           \
--hostname s3                       \
-e USER=nowage -e PASSWD=nowage     \
-v ~/df/Spark:/df             \
-p 8084:8081                        \
-p 8083:8080                        \
-p 7073:7077                        \
nowage/spark_mysql
```



## Splunk
* Info : 현 방식의 문제는 Volume Mapping이 안된다는 것.
    * 아래 Failed Usage 방법으로는 아직 안됨.
* Link : https://hub.docker.com/r/splunk/splunk/
* Usage :
```
git clone https://github.com/splunk/docker-splunk.git
cd docker-splunk/enterprise
docker rm -f enterprise_splunkenterprise_1;docker rm -f enterprise_vsplunk_1;docker-compose up
```

* Failed Usage
```
docker rm -f sp1
docker run                                            \
     -d                                               \
     --hostname sp1                                   \
     -e "SPLUNK_START_ARGS=--accept-license"          \
     -e "SPLUNK_USER=root"                            \
     -p "8000:8000"                                   \
     -v ~/df/splunk/etc:/opt/splunk/etc   \
     -v ~/df/splunk/var:/opt/splunk/var   \
     --name sp1                                       \
     splunk/splunk
docker exec sp1 entrypoint.sh splunk version
    # open -a Safari http://127.0.0.1:8000
```



## Mysql
* Info : mysql 기본 이미지
* Link : https://hub.docker.com/_/mysql/
* Usage :
```
cname=mys
docker rm -f $cname
rm -rf ~/df/Mysql/work/*
rm -rf ~/df/Mysql/data/*
docker run                           \
    -d                               \
    --rm                             \
    --name $cname                       \
    -p 8081:8080                     \
    -p 3307:3306                     \
    -v ~/df/Mysql/data:/var/lib/mysql/   \
    -v ~/df/Mysql/work:/root/work        \
    -e MYSQL_ROOT_PASSWORD=nowage  \
    mysql
docker logs -f  $cname
#--------------------
cname=mys
docker exec -it $cname bash
```
### Root remote access enable
```
echo "alter user root identified by '$MYSQL_ROOT_PASSWORD';
flush privileges;"|mysql -uroot -p$MYSQL_ROOT_PASSWORD
```

### nowage/mysql
* Info : mysql 기본 이미지
* Link : https://github.com/Finfra/dockers/tree/master/mysql

* Usage :
```
cname=mys
docker rm -f $cname
rm -rf ~/df/Mysql/work/*
rm -rf ~/df/Mysql/data/*
docker run                           \
    -d                               \
    --rm                             \
    --name $cname                       \
    -p 8081:8080                     \
    -p 3307:3306                     \
    -v ~/df/Mysql/data:/var/lib/mysql/   \
    -v ~/df/Mysql/work:/root/work        \
    -e MYSQL_ROOT_PASSWORD=nowage  \
    nowage/mysql &
docker logs -f  $cname
#--------------------
cname=mys
docker exec -it $cname bash
```
### Root remote access enable
```
echo "alter user root identified by '$MYSQL_ROOT_PASSWORD';
flush privileges;"|mysql -uroot -p$MYSQL_ROOT_PASSWORD
```

## Mysql 5.6

cname=mys
docker rm -f $cname
rm -rf ~/df/Mysql/work/*
rm -rf ~/df/Mysql/data/*
docker run                           \
    -it                               \
    --rm                             \
    --name $cname                       \
    -p 8081:8080                     \
    -p 3307:3306                     \
    -v ~/df/Mysql/data:/var/lib/mysql/data   \
    -v ~/df/Mysql/work:/root/work        \
    -e MYSQL_USER=user                 \
    -e MYSQL_PASSWORD=nowage      \
    -e MYSQL_DATABASE=db          \
    centos/mysql-56-centos7
docker logs -f  $cname
#--------------------
cname=mys
docker exec -it $cname bash

# Bigdata
##  docker-hive
```
wget https://raw.githubusercontent.com/Finfra/spark_quickstart/master/hiveScript.sh
git clone https://github.com/big-data-europe/docker-hive
cd docker-hive
docker-compose up&
docker cp ../hiveScript.sh docker-hive_hive-server_1:/root/
docker exec -it docker-hive_hive-server_1 bash /root/hiveScript.sh
docker exec -it docker-hive_hive-server_1 hive --hiveconf hive.metastore.uris=thrift://192.168.50.1:9083 -e "select * from dept;"
```
## hadoop-hive-spark
* Info :
```
Builds on cdecl/hadoop-live with a single node spark cluster built with hive and yarn support.

This instance of spark is configured to bind to the eth0 ipv4 ip address.

Run with the below command to give you a bash session on a server running a spark cluster.

docker run -it --rm -p 8080:8080 -p 7077:7077 gsoertsz/hadoop-hive-spark -bash

Test your script by: %> spark-submit --master spark://:7077 %> export MASTER=spark://:7077 %> run-example SparkPi
```
* Link : https://hub.docker.com/r/gsoertsz/hadoop-hive-spark/
* Usage :
```
docker run              \
-it                     \
--rm                    \
-p 8082:8080            \
-p 7077:7077            \
--name s1               \
-v ~/df/spark:/root/df  \
nowage/hadoop-hive-spark \
-bash
```

## CDH6 : fail in "latest shell"
* Info :
  - latest shell: /opt/cloudera/cm/schema/scm_prepare_database.sh mysql scm scm
  - passwd:root
  - systemctl start cloudera-scm-server
  - systemctl start cloudera-scm-agent
* Link : https://hub.docker.com/r/jsjchai/cdh6
* Usage :
```
docker run                \
--rm                      \
-it                       \
--name=c1                 \
-v ~/df/spark:/root/df    \
jsjchai/cdh6
```

# Analytic

## jupyter/datascience-notebook
* Info :
    *
* Link :
* Usage :
```
docker rm -f d3; docker run  -it                      \
              --name=d3                               \
              -v ~/work:/home/jovyan/work \
              -p 8889:8888                            \
              -e PASSWORD=xx                        \
              -e GRANT_SUDO=yes                       \
              jupyter/datascience-notebook
```
* cf)
```
sudo su -
```

## tensorflow/tensorflow
* Info : tensorflow 기본 이미지
* Link : https://hub.docker.com/r/tensorflow/tensorflow/
* Usage :
```
docker run -it --rm                   \
    --name=t1                         \
    -v ~/df:/notebooks/df \
    -v ~/__git/finfra/TensorflowStudyExample:/notebooks/TensorflowStudyExample \
    -p 8888:8888                      \
    -p 2222:22                        \
    -p 6006:6006                      \
    tensorflow/tensorflow
```

## nowage/tensorflow
* Info : tensorflow 기본 이미지
* Link : https://hub.docker.com/r/tensorflow/tensorflow/
* Usage :
```
docker rm -f t1
docker run -it                            \
       --name t1                          \
       -v /Users/nowage/df:/notebook/df   \
       -p 8888:8888                       \
       -p 6006:6006                       \
       tensorflow/tensorflow:latest-py3-jupyter
```





## jupyter/tensorflow-notebook
* Info : tensorflow 기본 이미지(Python3,tensorflow1.3)
* Link : https://hub.docker.com/r/jupyter/tensorflow-notebook
* Usage :
```
docker run -it --rm                   \
    --name=t1                         \
    -v ~/df:/home/jovyan/df \
    -v ~/__git/finfra/TensorflowStudyExample/:/home/jovyan/TensorflowStudyExample \
    -p 8888:8888                      \
    -p 2222:22                        \
    -p 6006:6006                      \
    jupyter/tensorflow-notebook
```




## bi_deeplearning
* PC
docker run -it --rm                  \
    --name=dl                        \
    -v ~/df:/root/df     \
    -p 8888:8888                     \
    -p 2222:22                       \
    -p 6006:6006                     \
    imcomking/bi_deeplearning
* MAC
docker run -it --rm --name=dl -v ~/folder:/root/df -p 8888:8888 -p2222:22 imcomking/bi_deeplearning
* Jupyter Script
python setup_nbserver.py
nowage
nowage
ipython notebook --no-browser --ip=172.17.0.3


## all-spark-notebook
* Link : https://github.com/jupyter/docker-stacks/tree/master/all-spark-notebook
* Usage :
```
docker rm spark
docker run                               \
 -it --rm                                \
 -p 8888:8888                            \
 -v ~/df/spark:/root/df      \
 --name=spark                            \
 jupyter/all-spark-notebook
```
### Spark
* Link : https://hub.docker.com/r/p7hb/docker-spark/
* Usage :
```
docker rm spark
docker run                               \
 -d                                      \
 --rm                                    \
 -p 4040:4040                            \
 -p 8080:8080                            \
 -p 8081:8081                            \
 -v ~/df/spark:/root/df      \
 --name=spark                            \
 gettyimages/spark
docker exec -it spark bash

spark-shell
pyspark
```

* To Test
```
start-master.sh
start-slave.sh spark://spark:7077
!? spark-submit --class org.apache.spark.examples.SparkPi --master spark://spark:7077 $SPARK_HOME/examples/jars/spark-examples*.jar 100
```






## datascienceschool/rpython
* For What : 딥러닝 강의용, conda설치 되어 있음. https://datascienceschool.net/view-notebook/03c5b5a96a614ee588a74f05c720e67c/
* Link :
* Usage :
```
docker run           \
--name=d1  --rm        \
-P -it               \
-p 8888:8888         \
-p 8000:8000          \
-p 8787:8787         \
-p 6006:6006         \
-v ~/df/ds:/home/dockeruser/df \
datascienceschool/rpython
```
* Git : https://github.com/datascienceschool/docker_rpython
* Boot2Docker Account
    - user id: docker
    - password: tcuser

* docker Account
    - 계정 이름: dockeruser
    - 계정 암호: dockeruserpass

## nowage/pyspark-notebook
* Info :
    1. 스팍 강의용, sudo su - 가능한 버전 ← --user옵션 사용하면 되니까 의미 없음.
    2. Original : jupyter/all-spark-notebook
* from :
* Link :
* Usage :
```
docker rm sp2
docker run                               \
 -it --rm                                \
 -p 8888:8888                            \
 -v ~/df:/root/fd            \
 --name=sp2                              \
 nowage/pyspark-notebook
```




## Shiny
* Info :
    *
* Link :
* Usage :
```
docker run --rm -p 3838:3838                                   \
    --name s1                                                  \
    -v ~/df/Shiny/shinyapps/:/srv/shiny-server/    \
    -v ~/df/Shiny/shinylog/:/var/log/shiny-server/ \
    rocker/shiny >> /Volumes/_Data/z_tmp/log/shiny_s1.log &
    open http://localhost:3838/
    docker exec -it s1 bash
```

## R Studio
* Usage :
```
docker run --rm        \
-v ~/df:/df            \
-p 8787:8787           \
-e DISABLE_AUTH=true   \
 rocker/rstudio
```
* with Shiny
```
docker run --rm        \
-v ~/df:/df            \
-p 8787:8787           \
-e PASSWORD=rstudio    \
-p 3838:3838           \
-e ADD=shiny           \
 rocker/rstudio
```
# Programming
## Nginx
```
docker rm -f n1;docker run -d  \
--name n1                      \
-p 80:80                       \
-v ~/df/nginx:/usr/share/nginx/html  \
nginx
```
## elm
* Link : https://github.com/maport/docker-elm
* Usage :
```bash
alias elm='docker run -it --rm -v "$(pwd):/code" -w "/code" -e "HOME=/tmp" -u $UID:$GID -p 8000:8000 codesimple/elm:0.18'
elm make
elm package
elm reactor -a 0.0.0.0
elm repl
elm test
```

## Vue.js -- toDo
* Info :  Vue.js
* Link :
  - https://hub.docker.com/r/ebiven/vue-cli/
  - https://github.com/ebiven/docker-vue-cli
* Usage :
```
docker run -it --rm \
     -v ~/df/vue.js:"$PWD" \
     -w "$PWD" \
     -p 8080:8080 \
     ebiven/vue-cli vue
```


# Cloud

## Terraform
* inline command 방식
* 실행 파일 만들어서 실행
* provider.docker가 안되는 것이 한계
          https://gist.github.com/lgallard/6f394791c76dadf990b5403b46d453c6
cat <<EOF>  tfdocker
#/bin/sh

`grep -A 2 "\[$1\]" $HOME/.aws/credentials  | tail -n 2 |  awk '{ print toupper($1)"="$3 }' | sed '/^AWS_/s/^/export /'`

docker run -it -v $PWD:/data  -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY  --workdir=/data/$2  hashicorp/terraform:$3 $4
EOF

* 간단버전
```
docker run -it  --rm      \
  --name t1           \
  -v $PWD:/data   \
  --workdir=/data   \
hashicorp/terraform:0.11.11  init/apply
```

## Rancher
### 1.6.26
* https://www.joinc.co.kr/w/man/12/docker/rancher/about?fbclid=IwAR3PISq-y7NxNsH3v_HaVBdTx3n8d8789BW9fEUpCfUgHH1Uu954gthmT-0
```
docker run -d               \
--name=r1                   \
--restart=unless-stopped    \
-v ~/df:/df                 \
-p 8080:8080                \
rancher/server
open http://127.0.0.1:8080
```
### 2.x
* https://github.com/rancher/rancher
```
docker run rancher/rancher --help

docker rm -f r2
docker run -d               \
--name=r2                   \
--restart=unless-stopped    \
-v ~/df:/df                 \
-p 1443:443        \
rancher/rancher

open https://127.0.0.1:1443
```


## Ansible
```
docker rm -f a1
docker run -it         \
  --name a1           \
  -v ~/df:/df         \
  ansible/centos7-ansible
docker exec -it a1 bash
```
### Ansible Tower
* login : admin/password
* home:  https://github.com/ybalt/ansible-tower

```
docker rm -f at1
docker run -d         \
  --name at1          \
  -p 8888:80          \
  -p 1443:443         \
  -v ~/df:/df         \
ybalt/ansible-tower
open https://lcoalhost:1443
docker exec -it at1 bash
```



## jenkins
### Jenkins Server
```
cat <<EOF> docker-compose.yaml
version: '2'
services:
  jenkins:
    image: 'jenkins/jenkins:lts'
    labels:
      kompose.service.type: nodeport
    ports:
      - '80:8080'
      - '443:8443'
      - '50000:50000'
    volumes:
      - 'jenkins_data:/jenkins_config'
volumes:
  jenkins_data:
    driver: local
EOF
docker-compose up -d
open http://0.0.0.0
```

* Wait for "unlock Jenkins"
```
docker exec -it $(basename $(pwd))_jenkins_1 cat /var/jenkins_home/secrets/initialAdminPassword
```




### Artifactory
* https://www.jfrog.com/confluence/display/RTF/Installing+with+Docker
mkdir ~/df/artifactory
docker rm -f artifactory ;docker run -d  \
   --name artifactory                    \
  -p 8081:8081                           \
-v ~/df/artifactory:/var/opt/jfrog/artifactory \
docker.bintray.io/jfrog/artifactory-oss:latest

docker exec -d artifactory chmod 777 /var/opt/jfrog/artifactory
docker exec -d artifactory chown -R 1030:1030 /var/opt/jfrog/artifactory

echo '
echo "
172.22.101.101  jenkins1
172.22.101.111  jm1
172.22.101.1    jm2
">> /etc/hosts
'> /tmp/tnowage.sh
docker cp /tmp/tnowage.sh artifactory:/tmp/
docker exec -d --user root artifactory bash /tmp/tnowage.sh

cf) login as root
docker exec -it --user root artifactory bash


## Prometheus
* Info :
    *
* Link : https://hub.docker.com/r/prom/prometheus/
* Usage :
```
docker run -d                           \
--name p1                               \
-v /Users/nowage/df/prometeous:/root/df \
-p 9090:9090                            \
prom/prometheus

docker exec -it p1 sh
```


# Network
## Mininet
* https://github.com/iwaseyusuke/docker-mininet
* http://mininet.org/download/
```
wget https://github.com/iwaseyusuke/docker-mininet/raw/master/docker-compose.yml
docker-compose run --rm mininet
```
```
mn
mn --topo tree,4,4
mn --test pingall --topo torus,N,M
mn --test pingall --topo single,3

```
# etc
## opencv+jupyter
philipz/jupyter-opencv3
```
	docker run -it --name opencv_p -e USER=admin -e PASSWD=1234  -v /_data/docker:/home/admin -p 8000:8000 -p 2222:22 philipz/jupyter-opencv3
```



## tadpoledbhub
* Docker Script
```
docker run      \
-it             \
--rm            \
--name t1       \
-p 32768:8080   \
hyunjongcho/tadpoledbhub:latest
```
* Connect
    1.7.1 <= Version : http://{yourIP}:32768/tadpole/tadpole/




## -
* Info :
    *
* Link :
* Usage :
```

```
