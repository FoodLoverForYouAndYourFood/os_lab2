# Ansible #
## Задача: ##
### Попрактиковаться в написании плейбука, со следующими условиями:
- подготовить стенд на Vagrant,
- настроить ansible для доступа к стенду
- написать плейбук, который устанавливает NGINX в конфигурации по умолчанию, с
применением модуля yum
- подготовить шаблон jinja2 новой конфигурации для nginx, чтобы сервис слушал на
нестандартном порту 8080. В шаблоне для номера порта использовать переменные
ansible
- добавить в плейбук копирование новой конфигурации сервиса на стенд
- должен быть использован механизм notify для рестарта nginx после установки или
изменения конфигурации 
[lab1](https://github.com/FoodLoverForYouAndYourFood/os_lab2/tree/master/lab1)
##  Установка Ansible #
>Проверьте версию Python. Для работы с Ansible нам нужна версия выше 2.6

Для проверки вводим команду: python3 -V
### • Если необходимо, установите ansible (yum install или apt install...)
Для проверки вводим команду: ansible --version
##  Подготовка Стенда
>Создаём каталог Ansible и копируем туда vagrant файл.

Вводим команду: vagrant up

А после: vagrant ssh-config
##  [Inventory](https://github.com/FoodLoverForYouAndYourFood/os_lab2/blob/master/lab1/inventory)
Для подключения к хосту nginx нам необходимо будет передать множество параметров - это особенность Vagrant. Узнать эти параметры можно с помощью команды vagrant ssh-config. После прописывания этой команды вводим необходимые данные в inventory файл. Указываем имя хоста, ip-адрес, путь до приватного ключа ansible
## [Playbook epel](https://github.com/FoodLoverForYouAndYourFood/os_lab2/blob/master/lab1/epel.yml)
>Написан epel playbook, который выполняет задачу установки пакетов репозиториев epel
## [Playbook nginx](https://github.com/FoodLoverForYouAndYourFood/os_lab2/blob/master/lab1/nginx.yml)
>Скачивает epel репозитории и создает конфигурационный файл по шаблону. В качестве заголовка указывается перезапуск.
## [Шаблон для конфига nginx](https://github.com/FoodLoverForYouAndYourFood/os_lab2/blob/master/lab1/nginx.conf.j2)
Был создан шаблон для конфигурационного файла. В нем указаны такие параметры сервера как root, location, прослушиваемый порт.
# Запуск скриптов
## Этап 1. Нужно удостовериться, что виртуальная мащина работает
Для этого написать команду "vagrant status" Если результат "running", то этот пункт выполнен.
## Этап 2. Проверить, что конфигурационный файл и inventory заполенен корректными данными
Для этого можно прописать команду "vagrant ssh-config" и проверить соответсвие данных с тем, что написано в ansible.cfg и inventory
## Этап 3. Запуск epel.yml
Для этого необходимо прописать в терминале команду "ansible-playbook epel.yml" Результат должен быть: "nginx : ok=2 changed=0 unreachable=0 failed=0 skipped=0 rescued=0 ignored=0"
## Этап 4. Запуск nginx.yml
Для запуска файла нужно прописать команду "ansible-playbook nginx.yml" Результат должен быть: "nginx : ok=4 changed=0 unreachable=0 failed=0 skipped=0 rescued=0 ignored=0"
# Результат
Открываем сайт по адресу http://192.168.11.150:8080 Если сайт открывается, то код работает корректно 
![](https://github.com/FoodLoverForYouAndYourFood/os_lab2/blob/master/lab1/photo_2023-04-04_13-08-28.jpg)

