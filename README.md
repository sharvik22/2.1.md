# Домашнее задание к занятию «Хранение в K8s. Часть 1» Шарапат Виктор

### Цель задания

В тестовой среде Kubernetes нужно обеспечить обмен файлами между контейнерам пода и доступ к логам ноды.

------

### Чеклист готовности к домашнему заданию

1. Установленное K8s-решение (например, MicroK8S).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключенным GitHub-репозиторием.

------

### Дополнительные материалы для выполнения задания

1. [Инструкция по установке MicroK8S](https://microk8s.io/docs/getting-started).
2. [Описание Volumes](https://kubernetes.io/docs/concepts/storage/volumes/).
3. [Описание Multitool](https://github.com/wbitt/Network-MultiTool).

------

### Задание 1 

**Что нужно сделать**

Создать Deployment приложения, состоящего из двух контейнеров и обменивающихся данными.

1. Создать Deployment приложения, состоящего из контейнеров busybox и multitool.
2. Сделать так, чтобы busybox писал каждые пять секунд в некий файл в общей директории.
3. Обеспечить возможность чтения файла контейнером multitool.
4. Продемонстрировать, что multitool может читать файл, который периодоически обновляется.
5. Предоставить манифесты Deployment в решении, а также скриншоты или вывод команды из п. 4.

------

### Задание 2

**Что нужно сделать**

Создать DaemonSet приложения, которое может прочитать логи ноды.

1. Создать DaemonSet приложения, состоящего из multitool.
2. Обеспечить возможность чтения файла `/var/log/syslog` кластера MicroK8S.
3. Продемонстрировать возможность чтения файла изнутри пода.
4. Предоставить манифесты Deployment, а также скриншоты или вывод команды из п. 2.

------

### Правила приёма работы

1. Домашняя работа оформляется в своём Git-репозитории в файле README.md. Выполненное задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, а также скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

------

### Решение 1

* Создал манифест deployment.yaml с двумя контейнерами busybox и multitool, применил его.




![image](https://github.com/user-attachments/assets/5dca8de3-ca80-43d6-a01a-882754915cc8)

* busybox: контейнер, который каждые 5 секунд записывает текущую дату и время в файл /shared-data/output.log.
* multitool: контейнер, который следит за изменениями в файле /shared-data/output.log и выводит их в консоль.
* Volume: используется emptyDir для создания общей директории между контейнерами.

![image](https://github.com/user-attachments/assets/7a4d2044-aa61-4420-a7c4-40e00a0e9fec)

* контейнер multitool успешно читает файл, который периодически обновляется контейнером busybox

---

### Решение 2

* Создал манифест daemonSet.yaml, применил его.

* ![image](https://github.com/user-attachments/assets/17e8853f-153f-48cd-8fcf-4727b8c7e03c)

![image](https://github.com/user-attachments/assets/d29d8220-ed1a-4728-babb-c8196eecea66)

* multitool: контейнер, который следит за изменениями в файле /var/log/syslog и выводит их в консоль.
* volume: используется hostPath для монтирования директории /var/log с хостовой машины в контейнер.

![image](https://github.com/user-attachments/assets/64adeae8-ca68-496b-a406-53817279e0cf)

* вывод командлы kubectl logs -f multitool-syslog-reader-mzvh4 -c multitool
![image](https://github.com/user-attachments/assets/a8e68915-1f78-445c-a94c-90c5d773dd85)

* вывод команды  nano /var/log/syslog
![image](https://github.com/user-attachments/assets/5710fe2c-f4ff-490d-bf70-00cc53fa7659)




---
