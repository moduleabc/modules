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
<img src="./img/c/gu1.gif">
