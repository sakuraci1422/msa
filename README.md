
   6 docker commit ubuntu:soo
   
   7 docker commit soo2 ubuntu:soo
   
   8 docker images
   
  10 docker save ubuntu:soo -o ubuntu_soo.docker
  
  16 docker ps -a
  
  25 docker load ubuntu_soo.docker
  
  41 docker load -i ubuntu_soo.docker
  
  42 docker images
  
  43 docker run exec -it --name sooo ubuntu

  47 docker exex -it sooo bash

  54 docker run -it --name soo1 --rm -v C://Users//kITRI//df:/df ubuntu

  67 docker run -it --name n1 -p 80:80 nginx


 101  docker run -d --name soo1 --rm -v C://Users//kITRI//df:/df -p 8080:80 nginx
 
 102 docker exec -it soo1
 
 103 docker exec -it soo1 bash


 121 docker commit u1 sakuraci81/ubuntu:v1

 123 docker login
 124 docker push sakuraci81/ubuntu:v1

 130 history
 131 docker commit u1 sakuraci81/ubuntu:v2
 132 docker push sakuraci81/ubuntu:v2
 133 docker login
 134 docker push sakuraci81/ubuntu:v2

