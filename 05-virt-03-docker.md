Задание 1:  
  
Забрал официальный образ nginx и отправил docker push в свой репозиторий на docker hub (тем самым получил форк)
Смонтировал новый образ на базе взятого, но заменив index.html. Отправил получившийся образ через docker push в репозиторий.  
  
https://hub.docker.com/repository/docker/nehardcore/netology_nginx/general  

Задание 2:  
Высоконагруженное монолитное java веб-приложение - контейнеры лучше использовать для микросервисов, монолитное лучше резвернуть на отдельной виртуалке
Nodejs веб-приложение - докер, удобно администрировать, масштабировать
Мобильное приложение c версиями для Android и iOS - VM, т.к. в докере нет GUI
Шина данных на базе Apache Kafka - по факту это микро сервис занимающийся передачей данных, значит докер
Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana - не особо с ними
 знаком, но пусть будет виртуацлизация, т.к. уже реализован кластер.
Мониторинг-стек на базе Prometheus и Grafana - докер, легко масштабировать
MongoDB, как основное хранилище данных для java-приложения - виртуалка/физ + кластер
Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry. - разбить все на микросервисы, реализовать в докере

Задание 3:  
```bash
cch@MBP-Costas netology % docker run -it --rm -d --name mycentos -v $(pwd)/data:/data centos
Unable to find image 'centos:latest' locally
latest: Pulling from library/centos
52f9ef134af7: Pull complete
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
7a9bb58882ec35d5f88dcc92b5207b5aee0fcfede236231ef4846477199e8b66

    cch@MBP-Costas netology % docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED              STATUS              PORTS     NAMES
7a9bb58882ec   centos    "/bin/bash"   About a minute ago   Up About a minute             mycentos

cch@MBP-Costas netology % docker run -it --rm -d --name mydebian -v $(pwd)/data:/data debian
Unable to find image 'debian:latest' locally
latest: Pulling from library/debian
c345c9e441f5: Pull complete
Digest: sha256:534da5794e770279c889daa891f46f5a530b0c5de8bfbc5e40394a0164d9fa87
Status: Downloaded newer image for debian:latest
50a7c3afc5f48c4e446b0efe51db77fed488de4d4b6a222b7d702c04efed5ce1

cch@MBP-Costas netology % docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
50a7c3afc5f4   debian    "bash"        6 minutes ago   Up 6 minutes             mydebian
7a9bb58882ec   centos    "/bin/bash"   8 minutes ago   Up 8 minutes             mycentos

cch@MBP-Costas netology % docker exec -it mycentos bash
[root@7a9bb58882ec /]# ls
bin  data  dev	etc  home  lib	lib64  lost+found  media  mnt  opt  proc  root	run  sbin  srv	sys  tmp  usr  var
[root@7a9bb58882ec /]# echo "123" >> /data/file_one.txt
[root@7a9bb58882ec /]# ls /data
file_one.txt
[root@7a9bb58882ec /]# exit

cch@MBP-Costas data % echo "wdwef" >> file_local.txt

cch@MBP-Costas netology % docker exec -it mydebian bash
root@50a7c3afc5f4:/# ls /data
file_local.txt	file_one.txt
root@50a7c3afc5f4:/# cat /data/file_local.txt
wdwef
root@50a7c3afc5f4:/# cat /data/file_one.txt
123
```