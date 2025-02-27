# Домашнее задание к занятию "5. Оркестрация кластером Docker контейнеров на примере Docker Swarm"

## Как сдавать задания

Обязательными к выполнению являются задачи без указания звездочки. Их выполнение необходимо для получения зачета и диплома о профессиональной переподготовке.

Задачи со звездочкой (*) являются дополнительными задачами и/или задачами повышенной сложности. Они не являются обязательными к выполнению, но помогут вам глубже понять тему.

Домашнее задание выполните в файле readme.md в github репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

Любые вопросы по решению задач задавайте в чате учебной группы.

---


## Важно!

1. Перед отправкой работы на проверку удаляйте неиспользуемые ресурсы.
Это важно для того, чтоб предупредить неконтролируемый расход средств, полученных в результате использования промокода.
Подробные рекомендации [здесь](https://github.com/netology-code/virt-homeworks/blob/virt-11/r/README.md).

2. [Ссылки для установки открытого ПО](https://github.com/netology-code/devops-materials/blob/master/README.md)

---

## Задача 1

Дайте письменые ответы на следующие вопросы:

- В чём отличие режимов работы сервисов в Docker Swarm кластере: replication и global?

```
Если global это означает, что данный сервис будет запущен ровно в одном экземпляре 
на всех возможных нодах. А replicated означает, что n-ое кол-во контейнеров для 
данного сервиса будет запущено на всех доступных нодах.
```

- Какой алгоритм выбора лидера используется в Docker Swarm кластере?

```
Лидер нода выбирается из управляючих нод путем Raft согласованного алгоритма.
Сам Raft-алгоритм имеет ограничение на количество управляющих нод. 
Распределенные решения должны быть одобрены большинством управляющих 
узлов, называемых кворумом. Это означает, что рекомендуется нечетное 
количество управляющих узлов.
```

- Что такое Overlay Network?
```
Используйте оверлейные сети
Драйвер оверлейной сети создает распределенную сеть среди нескольких
Хосты демона Docker. Эта сеть находится поверх (перекрывает) хост-
определенные сети, позволяя подключать к ним контейнеры (включая swarm s
служебные контейнеры) для безопасной связи при включенном шифровании.
Docker прозрачно обрабатывает маршрутизацию каждого пакета в и из
правильный хост демона Docker и правильный целевой контейнер.

Когда вы инициализируете рой или присоединяете хост Docker к существующему рою,
на этом хосте Docker создаются две новые сети:

оверлейная сеть, называемая входом, которая обрабатывает управление и данные
трафик, связанный с роевыми сервисами. Когда вы создаете рой-сервис и
не подключайте его к определяемой пользователем оверлейной сети, он подключается к
входная сеть по умолчанию.
мостовая сеть под названием docker_gwbridge, которая соединяет отдельные
Демон Docker другим демонам, участвующим в рое.
Вы можете создавать пользовательские оверлейные сети, используя docker network create,
так же, как вы можете создавать определяемые пользователем мостовые сети. Услуги
или контейнеры могут быть подключены более чем к одной сети одновременно. Услуги
или контейнеры могут обмениваться данными только через сети, к которым они подключены.

Хотя можно подключить как swarm-сервисы, так и автономные контейнеры
к оверлейной сети поведение по умолчанию и проблемы с конфигурацией
разные. По этой причине остальная часть этой темы разделена на o
операции, применимые ко всем оверлейным сетям, те, что применяются к роевым
служебные сети, а также те, которые относятся к оверлейным сетям, используемым автономными
контейнеры.
```


## Задача 2

Создать ваш первый Docker Swarm кластер в Яндекс.Облаке

Для получения зачета, вам необходимо предоставить скриншот из терминала (консоли), с выводом команды:
```
docker node ls
```
![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/07b4de93-615a-4fec-a589-6a93d65690ed)


## Задача 3

Создать ваш первый, готовый к боевой эксплуатации кластер мониторинга, состоящий из стека микросервисов.

Для получения зачета, вам необходимо предоставить скриншот из терминала (консоли), с выводом команды:
```
docker service ls
```
![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/2fe82176-5891-434c-9b8a-44edf300beda)


## Задача 4 (*)

Выполнить на лидере Docker Swarm кластера команду (указанную ниже) и дать письменное описание её функционала, что она делает и зачем она нужна:
```
# см.документацию: https://docs.docker.com/engine/swarm/swarm_manager_locking/
docker swarm update --autolock=true
```
![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/0259a28d-d5a7-478f-9294-d7589e6d72b7)


