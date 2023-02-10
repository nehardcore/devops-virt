# Задача 1

Дайте письменые ответы на следующие вопросы:

>В чём отличие режимов работы сервисов в Docker Swarm кластере: replication и global?  

* Global: сервис распространяется на каждую ноду в кластере
* Replication: манаджер нода распространяет сервисы на какие-то конкретные ноды

>Какой алгоритм выбора лидера используется в Docker Swarm кластере?  

docker node ls:
в колонке MANAGER STATUS указан leader - главный менеджер, с ним идет постоянный обмен hearbeat-ами (период рандомный 150-300мс), если нода не доступна, 
то остальные менеджеры (которые в статусе reachable) становятся кандидатами на роль лидера и начинается голосование. Если в процессе голосования кандидаты получили heartbear от лидера, все возвращается как было, а если нет, то зависит от того какой кандидат получит больше голосов. 
Алгоритм выбора называется Raft Consensus Algorythm.

>Что такое Overlay Network?

Сеть внутри главной сети между докер контейнерами. Для безопасности. Если получить доступ к главной сети, то не будет доступа к зашифрованному каналу обмена данными внутри docker swarm.

# Задача 2
    [yc-user@fhmp44p7tnks6s13fu7a ~]$ docker node ls
    ID                            HOSTNAME                             STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
    77ubdfsn25phiuu5yq0tg9x2h     fhm5dcovdh2s66itr9qk.auto.internal   Ready     Active                          23.0.0
    m30kwhcdwuqmf847un0wsusgc     fhm35q3khoppa886g83a.auto.internal   Ready     Active                          23.0.0
    r6f6nqrz36ya0fqvrhm9qbijo *   fhmp44p7tnks6s13fu7a.auto.internal   Ready     Active         Leader           23.0.0 
    
# Задача 3
![image](https://user-images.githubusercontent.com/97674120/218045967-306faff5-2700-46ed-aab8-5f01d82d7e7d.png)


