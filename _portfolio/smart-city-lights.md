---
title: "Интеллектуальное управление городским освещением"
author_profile: false
excerpt: "Дизайн-концепт интеллектуальной облачной системы с подключением датчиков и контроллеров IoT для мониторинга и управления городского освещения с целью энергосбережения и предотвращения аварийных ситуаций."
header:
  image: assets/images/smart-city-lights/smart-city-lights-scheme.png
  teaser: assets/images/smart-city-lights/smart-city-lights-scheme-th-min.png
sidebar:
  - title: "Место"
    image: assets/images/smart-city-lights/ivolga-logo.png
    image_alt: "logo"
    text: [Молодежный форум iВолга-2018, Самарская обл.](http://ivolgaforum.ru/)
  - title: "Роль"
    text: "Участник"
  - title: "Мой вклад"
    text: "Представил свой проект участникам и жюри форума"
gallery:
  - url: assets/images/smart-city-lights/ivolga-forum-info.png
    image_path: assets/images/smart-city-lights/ivolga-forum-info.png
    alt: "Информация о форуме"
  - url: assets/images/smart-city-lights/me-and-my-stand.png
    image_path: assets/images/smart-city-lights/me-and-my-stand.png
    alt: "Я и мой выставочный стенд - прототип системы"
  - url: assets/images/smart-city-lights/prototype-web-dashboards.png
    image_path: assets/images/smart-city-lights/prototype-web-dashboards.png
    alt: "Прототип системы мониторинга и управления из браузера"
  - url: assets/images/smart-city-lights/hardware-components.png
    image_path: assets/images/smart-city-lights/hardware-components.png
    alt: "Компоненты hardware-устройства"
---

В рамках молодежного форума iВолга-2018 и выставки "Наука и Образование" был спроектирован дизайн-концепт и разработан прототип интеллектуального городского освещения. Прототип позволяет осуществлять централизованный сбор данных с датчиков IoT, осуществлять базовую аналитику и отображать пользователю-оператору отчетную информацию и оповещения в реальном времени через браузер.

Аппаратная часть состоит из микроконтроллера ESP-8266 (NodeMCU) с подключенными аналоговыми и цифровыми датчиками расстояния, освещенности и др. Обмен данными с облачной системой хранения и обработки организован по защищенному протоколу MQTT. Обладает автономным питанием от литиевого аккумулятора. Заряда должно хватать на 3 месяца непрерывной работы.

Программная часть основана клиент-серверной архитектуре. Для визуализации данных используется система [Grafana](http://grafana.com). Обработка и хранение данных происходит в базе данных  [InfluxDB](http://influxdata.com). Дополнительный анализ осуществляется с помощью скриптов на языке Python.

Данный прототип был представлен на выставке, получил высокую оценку жюри и положительные отзывы других участников молодежного форума.

Презентация проекта - [скачать]({{ site.baseurl }}/assets/files/smart-city-lights/Smart Techologies. iVolga-18.pdf)

{% include gallery %}
