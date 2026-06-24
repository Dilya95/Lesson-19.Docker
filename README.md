# Домашнее задание 19: Docker

## Задания
Установите Docker на хост машину https://docs.docker.com/engine/install/ubuntu/<br>
Установите Docker Compose - как плагин, или как отдельное приложение<br>
Создайте свой кастомный образ nginx на базе alpine. После запуска nginx должен отдавать кастомную страницу (достаточно изменить дефолтную страницу nginx)<br>
Определите разницу между контейнером и образом<br>
Вывод опишите в домашнем задании.<br>
Ответьте на вопрос: Можно ли в контейнере собрать ядро?

## Структура

```
├── nginx/<br>
│   ├── Dockerfile<br>
│   └── index.html<br>
├── README.md            # описание + ссылка на Docker Hub
```

## Ссылка на мой готовый репозиторий в Docker Hub
```
https://hub.docker.com/r/dilya9595/my-custom-nginx
```

## Выполнение

### Docker у меня уже установлен, доустановила docker-compose
```
dilyam@MacBook-Pro-Dilya-2 ~ % docker -v
Docker version 29.2.0, build 0b9d198

dilyam@MacBook-Pro-Dilya-2 ~ % brew install docker-compose

dilyam@MacBook-Pro-Dilya-2 ~ % docker-compose -v
Docker Compose version 5.1.4

```

### Создала директорию для проекта и файлы index.html, Dockerfile
```
dilyam@MacBook-Pro-Dilya-2 ~ % mkdir my-nginx

dilyam@MacBook-Pro-Dilya-2 ~ % cd ./my-nginx

dilyam@MacBook-Pro-Dilya-2 my-nginx % nano index.html

dilyam@MacBook-Pro-Dilya-2 my-nginx % cat index.html 
<!DOCTYPE html>
<html>
<head>
  <title>Мой кастомный Nginx</title>
</head>
<body>
  <h1>Hello! This is my custom page from my Docker image based on  Alpine</h1>
  <p>Homework is done</p>
</body>
</html>


dilyam@MacBook-Pro-Dilya-2 my-nginx % nano Dockerfile

dilyam@MacBook-Pro-Dilya-2 my-nginx % cat Dockerfile 
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```



### Загрузила образ к себе, запустила контейнер, проверила работу nginx
```
dilyam@MacBook-Pro-Dilya-2 my-nginx % docker build -t my-custom-nginx:alpine .
[+] Building 0.1s (7/7) FINISHED                                                                                                                                       docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                                   0.0s
 => => transferring dockerfile: 149B                                                                                                                                                   0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                                                                                                        0.0s
 => [internal] load .dockerignore                                                                                                                                                      0.0s
 => => transferring context: 2B                                                                                                                                                        0.0s
 => [internal] load build context                                                                                                                                                      0.0s
 => => transferring context: 251B                                                                                                                                                      0.0s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:6fa463e852c49e23e431726a54f8dd0b05dfcaef6cb435885f4b904fc4ef8ee5                                                                  0.0s
 => => resolve docker.io/library/nginx:alpine@sha256:6fa463e852c49e23e431726a54f8dd0b05dfcaef6cb435885f4b904fc4ef8ee5                                                                  0.0s
 => [2/2] COPY index.html /usr/share/nginx/html/index.html                                                                                                                             0.0s
 => exporting to image                                                                                                                                                                 0.1s
 => => exporting layers                                                                                                                                                                0.0s
 => => exporting manifest sha256:5d2995fd497be804fb72088a9677e18544a4f479453b90bfb0041677ec36e168                                                                                      0.0s
 => => exporting config sha256:dfdae0a6fdf8fe2c24623c146fae0f9f9ec722711bc8d929f31e8ffbfb901e9f                                                                                        0.0s
 => => exporting attestation manifest sha256:b10d06727fd4711224fb5b4e29922fa4219ecc0944572a7952d8c9adeadc0a7a                                                                          0.0s
 => => exporting manifest list sha256:690b931963a32b08c352abee36fc7f8ae40988fa50560d9cf93b0021e0fa2ba5                                                                                 0.0s
 => => naming to docker.io/library/my-custom-nginx:alpine                                                                                                                              0.0s
 => => unpacking to docker.io/library/my-custom-nginx:alpine                                                                                                                           0.0s


dilyam@MacBook-Pro-Dilya-2 my-nginx % docker run -d -p 8090:80 --name my-nginx my-custom-nginx:alpine
06036affeb083b78a7ad0928f2d20a5a96111d1888bfc1051438ddeb432a0474


dilyam@MacBook-Pro-Dilya-2 my-nginx % docker ps -a
CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS         PORTS                                     NAMES
06036affeb08   my-custom-nginx:alpine   "/docker-entrypoint.…"   7 seconds ago   Up 6 seconds   0.0.0.0:8090->80/tcp, [::]:8090->80/tcp   my-nginx


dilyam@MacBook-Pro-Dilya-2 my-nginx % curl http://localhost:8090     
<!DOCTYPE html>
<html>
<head>
  <title>Мой кастомный Nginx</title>
</head>
<body>
  <h1>Hello! This is my custom page from my Docker image based on  Alpine</h1>
  <p>Homework is done</p>
</body>
</html>
```


### Создала тег для моего кастомного образа и запулила мой образ в Docker Hub
```
dilyam@MacBook-Pro-Dilya-2 my-nginx % docker tag my-custom-nginx:alpine dilya9595/my-custom-nginx:alpine


dilyam@MacBook-Pro-Dilya-2 my-nginx % docker push dilya9595/my-custom-nginx:alpine
The push refers to repository [docker.io/dilya9595/my-custom-nginx]
a28acd1377b2: Pushed 
b82182f5a992: Pushed 
0443ca9c1c20: Pushed 
4c83039edf06: Pushed 
14a4754c352f: Pushed 
4d898d123a22: Pushed 
00a8abe377ce: Pushed 
80d7950679c8: Pushed 
c9b4ed3ccb4c: Pushed 
b87ab97d91a1: Pushed 
cf44d1ee1f5a: Pushed 
alpine: digest: sha256:690b931963a32b08c352abee36fc7f8ae40988fa50560d9cf93b0021e0fa2ba5 size: 856

```


### Ответы на вопросы:
#### Определите разницу между контейнером и образом.
```
Образ — шаблон по которому будет запущен контейнер, Контейнер — запущенный экземпляр образа.
```

#### Можно ли в контейнере собрать ядро?
```
Можно, но с нюансами. По умолчанию контейнеры не имеют полного доступа к hardware/kernel, поэтому без специальных флагов сборка модулей ядра не будет работать.
```

