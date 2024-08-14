## Part 1. Готовый докер

- Возьми официальный докер-образ с nginx и выкачай его при помощи `docker pull`

![](pict/part1/dockerpull.png)

- Проверь наличие докер-образа через `docker images`

![](pict/part1/images.png)

- Запусти докер-образ через `docker run -d [image_id|repository]`

![](pict/part1/run.png)

- Проверь, что образ запустился через `docker ps`

![](pict/part1/ps.png)

- Посмотри информацию о контейнере через `docker inspect [container_id|container_name]`, container name: `keen_margulis`

![](pict/part1/inspect.png)

- По выводу команды определи и помести в отчёт:

  1) размер контейнера:

  ![](pict/part1/size.png)

  2) список замапленных портов:

  ![](pict/part1/ports.png)

  3) `ip` контейнера

  ![](pict/part1/ip.png)

- Останови докер образ через `docker stop [container_id|container_name]`
- Проверь, что образ остановился через `docker ps`:

![](pict/part1/stop.png)

- Запусти докер с портами 80 и 443 в контейнере, замапленными на такие же порты на локальной машине, через команду `run`, container name: `stupefied_swanson`

![](pict/part1/80-443.png)

- Проверь, что в браузере по адресу localhost:80 доступна стартовая страница nginx

![](pict/part1/localhost.png)

- Перезапусти докер контейнер через `docker restart [container_id|container_name]`
- Проверь любым способом, что контейнер запустился:

![](pict/part1/restart.png)

---
---

## Part 2. Операции с контейнером

- Прочитай конфигурационный файл `nginx.conf` внутри докер контейнера через команду `exec`

![](pict/part2/exec.png)

- Создай на локальной машине файл `nginx.conf`

![](pict/part2/ngconf.png)

- Настрой в нем по пути /status отдачу страницы статуса сервера nginx.

![](pict/part2/status.png)

- Скопируй созданный файл `nginx.conf` внутрь докер-образа через команду `docker cp`

![](pict/part2/copy.png)

- Перезапусти nginx внутри докер-образа через команду `exec`

![](pict/part2/reload.png)

- Проверь, что по адресу `localhost:80/status` отдается страничка со статусом сервера nginx.

![](pict/part2/localhost.png)

- Экспортируй контейнер в файл `container.tar` через команду `export`

![](pict/part2/export.png)

- Останови контейнер.

![](pict/part2/stop.png)

- Удали образ через `docker rmi [image_id|repository]`, не удаляя перед этим контейнеры.

![](pict/part2/rmi.png)

- Удали остановленный контейнер

![](pict/part2/rm.png)

- Импортируй контейнер обратно через команду `import`

![](pict/part2/import.png)

- Запусти импортированный контейнер

![](pict/part2/run.png)

- Проверь, что по адресу `localhost:80/status` отдается страничка со статусом сервера nginx

![](pict/part2/localhost2.png)

---
---

## Part 3. Мини веб-сервер

- Напиши мини-сервер на C и FastCgi, который будет возвращать простейшую страничку с надписью Hello World!.

![](pict/part3/1.png)

- Напиши свой nginx.conf, который будет проксировать все запросы с 81 порта на 127.0.0.1:8080.

![](pict/part3/2.png)

- Скопировали созданный nginx.conf и мини сервер в контейнер

![](pict/part3/3.png)

- Заходим в контейнер, устанавливаем gcc, spawn-fcgi и libfcgi-dev

![](pict/part3/4.png)

- Скомпилировали и запустили написанный мини сервер через spawn-fcgi на порту 8080

![](pict/part3/5.png)

- Применили изменения в настройках сервера:

![](pict/part3/6.png)

- Проверь, что в браузере по localhost:81 отдается написанная тобой страничка.

![](pict/part3/7.png)

- Положи файл nginx.conf по пути ./nginx/nginx.conf (это понадобится позже)

![](pict/part3/8.png)

## Part 4. Свой докер

- Напиши свой докер-образ, который:

1) собирает исходники мини сервера на FastCgi из Части 3, копирует внутрь образа написанный ./nginx/nginx.conf;

![](pict/part4/part4.png)
![](pict/part4/dockerfile.png)

2) запускает его на 8080 порту;

![](pict/part4/nginx1.png)

3) запускает nginx

![](pict/part4/run.png)

- Собери написанный докер-образ через `docker build` при этом указав имя и тег (`docker build -t part4:v1 .`)

![](pict/part4/build.png)

- Проверь через `docker images`, что все собралось корректно

![](pict/part4/images.png)

- Запусти собранный докер-образ с маппингом 81 порта на 80 на локальной машине и маппингом папки ./nginx внутрь контейнера по адресу, где лежат конфигурационные файлы nginx'а (`docker run -d -p 80:81 -v $(pwd)/nginx/nginx.conf:/etc/nginx/nginx.conf part4:v1`)

![](pict/part4/dockrun.png)

- Проверь, что по localhost:80 доступна страничка написанного мини сервера.

![](pict/part4/hello.png)

- Допиши в ./nginx/nginx.conf проксирование странички /status, по которой надо отдавать статус сервера nginx

![](pict/part4/status.png)

- Перезапусти докер-образ. Проверь, что теперь по localhost:80/status отдается страничка со статусом nginx

![](pict/part4/checkstatus.png)

---
---

## Part 5. Dockle

- Просканируй образ из предыдущего задания через dockle [image_id|repository].

![](pict/part5/dockle1.png)

- Исправь образ так, чтобы при проверке через dockle не было ошибок и предупреждений

![](pict/part5/dock.png)

- Просканировала образ:

![](pict/part5/dockle2.png)

---
---

## Part 6. Базовый Docker Compose

- Напиши файл docker-compose.yml, с помощью которого:

1) Подними докер-контейнер из Части 5 (он должен работать в локальной сети, т.е. не нужно использовать инструкцию EXPOSE и мапить порты на локальную машину).
2) Подними докер-контейнер с nginx, который будет проксировать все запросы с 8080 порта на 81 порт первого контейнера.

![](pict/part6/yml.png)

- Замапь 8080 порт второго контейнера на 80 порт локальной машины.

![](pict/part6/nginx.png)

- Останови все запущенные контейнеры. Собери и запусти проект с помощью команд `docker-compose build`

![](pict/part6/build.png)

  и `docker-compose up`

![](pict/part6/up.png)

- Проверь, что в браузере по localhost:80 отдается написанная тобой страничка, как и ранее

![](pict/part6/loc.png) ![](pict/part6/status.png)