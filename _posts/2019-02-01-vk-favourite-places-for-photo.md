---
published: true
layout: single
title:  Анализ городской среды через гео-данные фотографий из VK
excerpt: >-
  Однажды захотелось поработать с VK API и картографическими сервисами Google.
  За основу взял задачу определения мест в городах Крыма пользующихся наибольшей популярностью и пользователей соцсети, т.е. требовалось собрать все фотографии сделанные в заданной области (VK хранит информацию о том, где была сделана фотография) и сформировать из этого набора тепловую карту.
categories: programming
tags: python vk google maps
toc: true
header:
  teaser: /assets/images/vk-favourite-places/title-min.png
  og_image: /assets/images/vk-favourite-places/title-min.png
---

Однажды захотелось поработать с [VK API][vk-api] и картографическими сервисами [Google][gmaps]. За основу взял задачу определения мест в городах Крыма пользующихся наибольшей популярностью и пользователей соцсети, т.е. требовалось собрать все фотографии сделанные в заданной области (VK хранит информацию о том, где была сделана фотография) и сформировать из этого набора тепловую карту.

В данной статье покажу полученные результаты, программу для повторения моих опытов и опишу ограничения с которыми пришлось столкнуться. Для работы с данными и интерфейсами VK и Google использовал язык Python и Jupyter Notebooks.

Для анализа взял несколько мест на карте - города пользующиеся популярностью у туристов - Евпатория, Севастополь, Ялта; и Симферополь, как представитель крупного города, где гости не задерживаются надолго и как таковые туристические объекты отсутствуют.

## Используемые инструменты

1. [Python 3.7][python]
2. [JupyterLab 0.35.4][jupyter]
3. Библиотеки python:
  * [jupyter-gmaps 0.8.2][jupyter-gmaps]
  * [requests 2.21.0][requests]
  * [pandas 0.24.0][pandas]

Не буду расписывать настройку среды и все подготовительные действия, они есть на официальных сайтах инструментов. Все довольно просто.
Но упомяну, что библиотека jupyter-gmaps требует пересборки jupyter (см. инструкции по [установке](https://jupyter-gmaps.readthedocs.io/en/stable/install.html)).

## Программа для сбора и обработки данных

Для начала подключим и импортируем библиотеки:

```python
import pandas as pd
import gmaps
import gmaps.datasets
import requests
from random import randint
from datetime import datetime
from time import sleep, strftime, localtime, mktime, strptime
from csv import DictWriter, DictReader
```

Потом необходимо [создать][create-app] свое приложение VK пользуясь этим [руководством][create-app-doc].
И приложение Google по этой [инструкции][create-google-token].
В итоге мы получим два токена (где '...' это различные символы) или ключа для работы с API сервисов.

```python
GOOGLE_API_KEY = 'AI......'
VK_ACCESS_TOKEN = 'd05......'
VK_VERSION = '5.92'
```

Затем требуется так называемая инициализация переменных
```python
geo = (44.498231, 34.169317) # точка поиска - Ялта администрация (взял из карт)
dist = 3000  # радиус поиска фотографий (в метрах)
timeperiod = (1514764800, 1546300800) # диапазон времени в котором сделано фото
                                      # ( 00:00 1.01.2018,  00:00 1.01.2019 )
```

Описание функции для получения данных от VK. На выходе получаем JSON структуру.
```python
def getVK(geo, timeperiod, offset):
    params = {
        'lat': geo[0],
        'long': geo[1],
        'count': '1000',
        'offset': offset,
        'radius': dist,
        'start_time': timeperiod[0],
        'end_time': timeperiod[1],
        'access_token': VK_ACCESS_TOKEN,
        'v': VK_VERSION,
        'sort': 0 # by date of creation
    }
    return requests.get("https://api.vk.com/method/photos.search",
                        params=params, verify=True).json()
```

Получаемый ответ будем сохранять в структуру map (словарь), где ключом будет пара - id фото и время фото.
Это позволит избежать повторов одинаковых фотографий.
Производить сохранение в словарь будем через функцию, принимающую на входе ответ сервера и сам словарь.
Там же сделаем обработку исключений, т.к. иногда ответ выдает пустой JSON.
В переменную `map_data` сохраняем непосредственно GPS координаты фотографии.

```python
def save_to_map(resp, map_data):
    try:
        items = resp['response']['items']
    except KeyError:
        return
    for f in items:
        try:
            map_data[(f['id'], f['date'])] = (f['lat'], f['long'])
        except KeyError:
            continue
```

И пожалуй самое главное - функция для запуска сбора данных из VK.

Что в ней важно?
Используется метод [photos.search][photos-search].
Поиск фото ведется циклически в течение заданного временного промежутка с шагов в 24 часа (сутки) - сделано для того, чтобы преодолеть ограничение VK на максимальное количество возвращаемых фото (3000 фото) при одинаковом запросе.
Сделан цикл для получения дополнительных фотографий, которые не были получены в первый раз (расчет сдвига - offset). И если обработка зацикливания в случае преодоления этого лимита. В общем, есть одна особенность - мы не можем получить больше 3000 фотографий за сутки, все остальные будут так или иначе отброшены. Но в данном примере ими можно принебречь. Т.к. данных для анализа все равно будет достаточно. И да, пришлось добавить вызов функции `sleep(0.5)` для того чтобы не слать очень часто запросы и VK не забанил бы приложение.

```python
map_data = {}
step = 24*60*60 # шаг - день
i = timeperiod[0]
while i < timeperiod[1]:
    resp = getVK(geo, i, i+step, 0)
    save_to_map(resp, map_data)
    count = resp['response']['count']
    returned = len(resp['response']['items'])
    if count > returned:
        offset = returned
        while offset < count and offset < 3000:
            resp = getVK(geo, i, i+step, offset)
            save_to_map(resp, map_data)
            count = resp['response']['count']
            returned = len(resp['response']['items'])
            offset = offset + returned
            if returned == 0:
                break
    i = i + step
    sleep(0.5)      
```

Дальше написал функции для сохранения полученного словаря `map_data` с данными о расположении фотографий и чтении этих данных обратно в словарь. Это чтобы не производить сбор данных еще раз и в будущем был бы уже готовый датасет.

```python
def save_map_to_csv(map_data, path):
    with open(path, 'w', newline='', encoding='utf-8') as csvfile:
        columns = ['id', 'date', 'lat', 'long']
        writer = DictWriter(csvfile, fieldnames=columns, delimiter=';')
        writer.writeheader()
        for key in map_data:
            data = {
                'id': key[0],
                'date': strftime("%d.%m.%Y", localtime(key[1])),
                'lat': map_data[key][0],
                'long': map_data[key][1],
            }
            writer.writerow(data)

def read_csv_to_map(map_data, path):
    with open(path, newline='', encoding='utf-8') as csvfile:
        reader = DictReader(csvfile, delimiter=';')
        for row in reader:
            map_data[(int(row['id']),
                      int(mktime(strptime(row['date'], '%d.%m.%Y'))))] =
                (float(row['lat']), float(row['long']))
```

Воспользуемся сохранением в CSV формат полученных данных

```python
save_map_to_csv(map_data, 'yalta2018.csv')
```

И последнее, что осталось сделать это нанести фотографии на карту и создать из них тепловую карту, для выявления очагов, где было сделано наибольшее количество фотографий.

```python
print('Count of photos {}'.format(len(map_data)))
gmaps.configure(api_key=GOOGLE_API_KEY)
fig = gmaps.figure(center=geo, zoom_level=13)
heatmap_layer = gmaps.heatmap_layer(list(map_data.values()))
fig.add_layer(heatmap_layer)
fig
```

У Google тоже есть ограничение на максимальное количество запросов в день и др.
Тогда карта окрашивается в темный цвет (см. пример с Симферополем).
В таких случаях я просто создал еще одно приложение и получил новый токен.

В общем, вот так выглядит программа и полученная тепловая карта в JupyterLab

![jupyter-lab-notebook]({{ site.baseurl }}/assets/images/vk-favourite-places/jupyter-lab-interface.png){: .align-center}

## Результаты

В итоге были получены следующие результаты. Я их скомпоновал и выложил в [открытый доступ][google-presentation].

### Евпатория - 2017 год

Собрано около 217 тысяч фотографий - [итоговый CSV-файл][evpatoria-2017-csv].
![evpatoria-2017]({{ site.baseurl }}/assets/images/vk-favourite-places/evpatoria2017.png){: .align-center}

### Евпатория - 2018 год

Собрано около 273 тысяч фотографий - [итоговый CSV-файл][evpatoria-2018-csv].
![evpatoria-2018]({{ site.baseurl }}/assets/images/vk-favourite-places/evpatoria2018.png){: .align-center}

### Симферополь - 2017 год

Собрано около 370 тысяч фотографий - [итоговый CSV-файл][simferopol-2017-csv].
![simferopol-2017]({{ site.baseurl }}/assets/images/vk-favourite-places/simferopol2017.png){: .align-center}

### Ялта - 2018 год

Собрано около 422 тысяч фотографий - [итоговый CSV-файл][yalta-2018-csv].
![yalta-2018]({{ site.baseurl }}/assets/images/vk-favourite-places/yalta2018.png){: .align-center}

### Севастополь - 2018 год

Собрано около 1 232 тысяч фотографий - [итоговый CSV-файл][sevastopol-2018-csv].
![sevastopol-2018]({{ site.baseurl }}/assets/images/vk-favourite-places/sevastopol2018.png){: .align-center}

## Выводы

Помимо полученного опыта работы с API google и VK, удалось с помощью соцсетей вычислить места более всего принятые для фотографирования.
Это может быть началом для более глубокой аналитики. Например, где сегментации по полу, возрасту, городу; определение временных предпочтений (что популярно днем/ночью, летом/зимой) и пр.
А для себя, благодаря исследованию, я нашел пару примечательных мест недалеко от Евпатории, которые собираюсь в ближайшее время посетить.

[vk-api]: https://vk.com/dev/first_guide
[gmaps]: https://www.google.com/maps
[python]: https://www.python.org/
[jupyter]: https://jupyter.org/
[jupyter-gmaps]: https://jupyter-gmaps.readthedocs.io/en/stable/index.html
[requests]: http://docs.python-requests.org/en/master/
[pandas]: http://pandas.pydata.org/
[photos-search]: https://vk.com/dev/photos.search
[create-app]: https://vk.com/editapp?act=create
[create-app-doc]: https://vk.com/dev/first_guide?f=2.%20%D0%A0%D0%B5%D0%B3%D0%B8%D1%81%D1%82%D1%80%D0%B0%D1%86%D0%B8%D1%8F%20%D0%BF%D1%80%D0%B8%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%B8%D1%8F
[create-google-token]: https://jupyter-gmaps.readthedocs.io/en/stable/authentication.html
[jupyter-gmaps]: https://jupyter-gmaps.readthedocs.io
[evpatoria-2017-csv]: {{ site.baseurl }}/assets/files/vk-favourite-places/evpatoria2017.zip
[evpatoria-2018-csv]: {{ site.baseurl }}/assets/files/vk-favourite-places/evpatoria2018.zip
[simferopol-2017-csv]: {{ site.baseurl }}/assets/files/vk-favourite-places/simferopol2017.zip
[yalta-2018-csv]: {{ site.baseurl }}/assets/files/vk-favourite-places/yalta2018.zip
[sevastopol-2018-csv]: {{ site.baseurl }}/assets/files/vk-favourite-places/sevastopol2018.zip
[google-presentation]: https://docs.google.com/presentation/d/10o3zywYCnKqnC6N70O8he3XyAHWikVdwPxepUXixMTQ
