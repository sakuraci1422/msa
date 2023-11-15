# Docker Command
## search
```
docker search ubuntu
```
## run
```
docker run hello-world
docker run -it ubuntu
    whoami
    pwd
    cat /etc/*lease
    exit
docker run -it  --name u1 ubuntu     # u1이 컨테이너 이름.
docker run -it --name u2 --rm  ubuntu # 컨테이너가 죽을때 지워짐.
```
### Volume Mapping
```
mkdir df
docker run -it --name u2 --rm  -v c:\\Users\\KITRI\\df:/df ubuntu
```
### Port Mapping
```
#docker run -it --name n1 -p 80:80 nginx
# 백그라운드로 동작하라고 터미널을 detach 시킴.. 계속 살아 있겠죠. 
docker run -d --name n1 -p 80:80 nginx
```

## attach
* 컨테이너에 터미널 붙이기, 단, 1개만 의미 있음.
* ctl+p,q를 누르면 컨테이너 종료 안하고 바로 나옴. 
```
docker run -it --name u3 --rm  ubuntu
    # ctl+p,q
docker attach u3    
```
## exec 
* 명령 전달
```
docker exec u3 mkdir /xx
```
* attach처럼 붙이기(여러 세션 가능)
```
docker exec -it u3 bash
```
## start/stop
```
docker run -it --name u4  ubuntu
    exit
docker start u4
docker exec -it u4 bash
```
## images / ps
```
docker images
docker ps -a
```
## rm/rmi
* rm먼저
```
docker rm 컨테이너명
docker rmi 이미지 명
```
## load/save
```
docker run -it --name u5 ubuntu
    apt update -y
    apt install -y tree
    cd /dev
    tree
    exit
docker commit u5 ubuntu:v1
docker images
# REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
# ubuntu       v1        71a54909b84c   8 seconds ago   124MB
# ubuntu       latest    e4c58958181a   5 weeks ago     77.8MB
docker save ubuntu:v1 -o ubuntu_v1.docker
docker rm -f u5
docker rmi ubuntu:v1
docker load -i ubuntu_v1.docker
docker images
docker run -it --name u6 --rm ubuntu:v1
    tree /dev
```
## Push
* hub.docker.com에 계정 만들어야 함. 
```
docker rm -f u1
docker run -it --name u1 ubuntu
    apt update -y && apt install -y tree
docker commit u1 아이디/ubuntu:v1
docker login
    아이디/비번 입력
docker push 아이디/ubuntu:v1

```


# 연습문제
## 1. -v 옵션을 통해 c:\\Users\\KITRI\\df를   /usr/share/nginx/html/ 에 연동해서 윈도우에서 html파일을 수정하면 nginx에서 적용되도록 컨테이너를 만들어 보세요. host os Port는 8880임.
```
docker run -d -v c:\\Users\\KITRI\\df:/usr/share/nginx/html/ -p 8880:80 --name n9 nginx
#linux or mac : docker run -d -v ~/df:/usr/share/nginx/html/ -p 8880:80 --name n9 nginx
```
