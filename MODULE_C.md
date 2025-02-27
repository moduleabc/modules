# Б3: Делаем Бэкапы

* На виртуалках 3 и 4 (base) создаём папки для бэкапов веба
  ```bsah
  cd ~
  mkdir backups
  cd backups
  mkdir web
  ```
* На виртуалках 1 и 2 создаём скрипт backup.sh в папке scripts (/home/team_x/scripts/backup.sh)
  ```bash
  nano /home/team_x/scripts/backup.sh
  ```
* В фалйе прописываем скрипт
  # НЕ ЗАБЫВАЕМ УКАЗАТЬ КОРРЕКТНЫЙ АЙПИ 3 И 4 ВИРТУАЛКИ СООТВЕТСТВЕННО
  ```bash
  #!/bin/bash
  DATE=$(date +"%Y-%m-%d-%H-%M-%S")
  rsync -avz /var/www/html/ team_1@192.168.2.X:/home/team_x/backups/web/$DATE
  ```
* Выдаём права на скрипт
  ```bash
  sudo chmod 777 /home/team_x/scripts/backup.sh
  ```
* # НЕ ЗАБЫВАЕМ ПРОКИНУТЬ КЛЮЧИ ЧЕРЕЗ SSS-COPY-ID (БЫЛО В ПРОШЛОМ ГАЙДЕ)
* Открываем cron через команду crontab -e (если попросит выбрать редактор, то выбираем 1)
* Вписываем нужное расписание
  ```bash
  0 0 * * * sh /home/team_x/scripts/backup.sh
  ```

* На виртуалках 1 и 2 (web) создаём папки для бэкапов базы
  ```bsah
  cd ~
  mkdir backups
  cd backups
  mkdir base
  ```

  * На виртуалках 3 и 4 создаём скрипт backup.sh в папке scripts (/home/team_x/scripts/backup.sh)
  ```bash
  nano /home/team_x/scripts/backup.sh
  ```
* В фалйе прописываем скрипт
  # НЕ ЗАБЫВАЕМ УКАЗАТЬ КОРРЕКТНЫЙ АЙПИ 1 И 2 ВИРТУАЛКИ СООТВЕТСТВЕННО
  ```bash
  #!/bin/bash
  DATE=$(date +"%Y-%m-%d-%H-%M-%S")
  rsync -avz /var/lib/pgpro/1c-15/data/ team_1@192.168.2.2:/home/team_1/backups/base/$DATE
  ```
* Выдаём права на скрипт
  ```bash
  sudo chmod 777 /home/team_x/scripts/backup.sh
  ```
* # НЕ ЗАБЫВАЕМ ПРОКИНУТЬ КЛЮЧИ ЧЕРЕЗ SSS-COPY-ID (БЫЛО В ПРОШЛОМ ГАЙДЕ)
* Выдадим себе права на папку постгреса
  ```bash
  sudo chmod 770 -R /var/lib/pgpro/1c-15/data/
  sudo gpasswd -a team_x postgres
  newgrp postgres
  ```
* Открываем cron через команду crontab -e (если попросит выбрать редактор, то выбираем 1)
* Вписываем нужное расписание
  ```bash
  0 0 * * * sh /home/team_x/scripts/backup.sh
  ```

# В: Ставим 1C

* Создаём новую виртуальную машину по характеристикам, которые указаны в задании (ВЫБИРАЕМ ISO ОБРАЗ WINDOWS)
* Ставим винду на виртуалку (не забываем установить имя пользователя и пароль на user_x и pass_x)
* Когда установили винду, нажимаем Win + R и пишем ncpa.cpl
* Выбираем наш LAN адаптер (у него не будет доступа в сеть, так что найти его лекго)
* Дальше открывааем по видео:
<img src="./img/c/vlc_KjICXiUwdN.gif">

* # Естественно ставим свои локальные айпишники, в винде маска подсети выглядит по другому, так что просто пишем 255.255.255.0 и не паримся
* Переходим в Параметры > Система > Удалённый Рабочий Стол > Включить Удалённый Рабочий Стол
* В поиске КЛИЕНТА пишем "Подключение к удалённому рабочему столу"
* Туда WAN айпи и логин, дальше попросит пароль
* На клиентской машине выделяем архив с 1C и нажимаем Ctrl + C
* На подключённом рабочем столе нажимаем Ctrl + V
* Когда файл перекачается туда, то можно закрывать удалённый рабочий стол
* Возвращаемся в окно виртуалки, которое в ESXI
* Разархивируем архив
* Открываем папку с архивом и заускаем vc_redistX64.exe
* Ставим галочку и устанавливаем:
<img src="./img/c/image3001.png">

* После установки редиста открываем setup.exe
* Доходим до выборочной установки, устанавливаем всё, кроме Коннектор ИБ, Дополнительно и Доп. Языков
<img src="./img/c/image3002.png">

* Выбираем "Создать Пользователя" и ставим пароль pass_x
<img src="./img/c/image3003.png">

* Далее нажимаем далее и устанавливаем

* После установки не забываем убрать галочку с "Установить драйвер аппаратных ключей защиты"
* Далее запускаем файлы в том порядке, который на скрине
<img src="./img/c/image3004.png">

* Раскрываем локальный кластер
<img src="./img/c/image3005.png">

* ПКМ по информационной базе > Создать > Информационная База
* Дальше согласно скриншоту:
<img src="./img/c/image3006.png">

# УКАЗЫВАЕМ ЛОКАЛЬНЫЙ АЙПИ НАШЕЙ БАЗЫ ДАННЫХ (base-teamX-01)

* Настраиваем 1C предприятие по видео (ЕСЛИ БУДЕТЕ ПЕРЕКИДЫВАТЬ ИНФОРМАЦИОННУЮ БАЗУ НА КОМП КЛИЕНТА, ТО УКАЗЫВАЕМ WAN АЙПИ ВМЕСТО localhost)
<img src="./img/c/vlc_CqaC3vwTRW.gif">

# Настраиваем Apache2

[Гайд/Источник](https://maxiplace.ru/blog/bitrix/veb-publikaciya-1s-osnovnye-aspekty/)

* Скачиваем Apache по [ссылке](https://www.apachelounge.com/download/) ФАЙЛ (httpd-2.4.63-250207-win64-VS17.zip)
* Закидываем папку Apache24 из архива на диск C
* Не забываем удалить файл index.html из C:\Apache24\htdocs
* Открываем PowerShell и прописываем
```bash
C:\Apache24\bin\httpd.exe -k install
New-NetFirewallRule -DisplayName "Apache 2.4" -Direction Inbound -Action Allow -EdgeTraversalPolicy Allow -Protocol TCP -LocalPort 80,443
```

* Тут обе галочки
<img src="./img/c/image3007.png">

* В поиске пишем "Службы" и запускаем службу Apache 2.4 (ПКМ ПО СЛУЖБЕ > Запустить)
* В проводнике переходим в Диск C > Program Files > 1cv8 > ТУТ_ВЕРСИЯ_1С > bin
* Копируем сверху путь до папки
* В cmd или powershell пишем cd этот_путь_сюда
* Выполняем команду
```bash
webinst -publish -apache24 -wsdir test -dir "C:\Apache24\htdocs" -connstr "Srvr=localhost;Ref=ТУТ_ВСТАВИТЬ_НАЗВАНИЕ_ИНФОРМАЦИОННОЙ_БАЗЫ(db в моём случае)"
C:\Apache24\bin\httpd.exe -k restart
```
* Переходим по нашему WAN айпи и проверяем работу (возможно придётся добавить /test в конец ссылки)

# Ставим Prometheus на Windows

[Гайд/Источник](https://ultravds.com/blog/kak-ustanovit-prometheus-na-windows-server/)

* Открываем прошлый гайд и скачиваем Prometheus, но уже под виндовс
* Папку внутри архива на диск C
* Скачиваем программу (NSSM)[https://nssm.cc/download]
* Папку внутри архива с NSSM на диск C
* Дальше в cmd или Powershell cd C:\nssm-2.24\win64
* Потом команду nssm install Prometheus
* В окне выбираем файл prometheus.exe
* В Arguments --config.file=prometheus.yml
* Нажимаем Install Service
* Переходим в службы и включаем службу Prometheus
* Нажимаем Win + R и пишем firewall.cpl
* Выбираем "Дополнительные Параметры"
* Слево "Правила Для Входящих подключений"
* Справо "Создать Правило"
* Выбираем "Для Порта" > Определённые локальные порты (9090) > Далее > Далее > Имя Prometheus
* В Grafana подключаем наш Prometheus
