# EVRAZ-AI-Challenge-Mountain-fighters
Хакатон EVRAZ [AI Challenge](https://hackathon.evraz.com/), 29-31 октября 2021 г. Решение команды **Mountain fighters**.

## Нейросеть

На основе YOLOv4 (https://github.com/tranleanh/darkeras-yolov4), с предобученными весами https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v3_optimal/yolov4.weights. Для работы в jupyter

1. Клонировать данный репозиторий
2. Разместить датасет в каталоге data_task2 рядом с репозиторием (не копировать в репозиторий)
3. Скачать в папку `\EVRAZ-AI-Challenge-Mountain-fighters\darkeras-yolov4\weights` файл `yolov4.weights` по ссылке https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v3_optimal/yolov4.weights.
4. Запустить jupyter notebook или jupyter lab в каталоге репозитория.
5. Открыть `\EVRAZ-AI-Challenge-Mountain-fighters\darkeras-yolov4\Mountain-fighters team ML RnD.ipynb`

![image](https://user-images.githubusercontent.com/26321368/139573830-469c4fa1-77cd-4cee-8b6f-cfaa38a7dce9.png)

Примеры размеченных изображений в каталоге `/visualized_recognition`

![am3_9_frame027](https://user-images.githubusercontent.com/26321368/139573973-d64599b2-2b63-49b1-a693-cafca3eac2af.jpg)


## Описание решения
Решение представляет собой cистемy для контроля опасных зон агломашины.

Нейронная сеть на основе YOLOv4 позволяет оценить положение рабочего на площадке со значением целевой функции 0.4.

Прототип веб-приложения позволяет воспроизводить в режиме реального времени кадры с камер, каталогизировать нарушения техники безопасности (при подключении модуля машинного обучения).

![image](https://user-images.githubusercontent.com/26321368/139573505-bfac40cb-8a59-4a22-a962-27cfc617f70e.png)


## Инструкция по запуску
Демо решение расположено по адресу [http://178.154.255.145/#/camera](http://178.154.255.145/#/camera (логин admin, пароль 1234, демо-приложение доступно до вечера 2 ноября 2021 г.)

Для запуска локально, см. Развертывание через docker-compose

## Руководство пользователя
В интерфейсе присутствует три основных вкладки: Дашборд, Нарушения и Камеры. Во вкладке камеры отображаются изображения с камер в режиме реального времени.

Вкладка Дашборд содержит статистику по камерам, сотрудникам и нарушениям.

Во вкладке Нарушения отображаются нарушения в соответствии с выбранными фильтрами, также здесь можно просматривать фотографии нарушений.

## Интеграция

Система предусматривает два режима использования:
1. Использование в качестве полноценной системы. В таком случае изображения с камер следует присылать следующим запросом:

Метод: POST

Путь: /rest_api/frame/

Параметры:
- camera_code - код камеры
- dttm - дата и время изображения в формате %Y-%m-%dT%H:%M:%S
- photo - файл с изображением

Система будет анализировать полученные изображения и определять нарушение. 

Кадры во вкладке Камеры будут автоматически обновляться

2. Использование только в качестве определения нарушения и/или выявления людей в кадре. Для этого предусмотрен следующий запрос:

Метод: POST

Путь: /rest_api/frame/detect/

Параметры:
- photo - файл с изображением

В ответ придет результат анализа.

## Развертывание через docker-compose
1. Установить [docker](https://docs.docker.com/engine/install/ubuntu/)
2. Установить [docker-compose](https://docs.docker.com/compose/install/)
3. В папке compose создать файлы .env и .uwsgi.env и заполнить их в соответствии с примерами
4. Запустить файл build.sh с правами суперпользователя
```bash
sudo ./build.sh
```
5. Настроить внешний nginx, который будет пересылать все запросы на порт приложения
## Команды docker-compose 
Все команды необходимо выполнять в папке compose
- Остановить все контейнеры
```bash
sudo docker-compose stop
```
- Перезапустить контейнер
```bash
sudo docker-compose restart {container_name}
```
- Запуск manage.py shell
```bash
sudo docker-compose exec web python manage.py shell
```
