---
title: "Визуализация потребления облачных ресурсов"
author_profile: false
excerpt: "Разработка система для сбора данных с облачной инфраструктуры основанной на OpenNebula и отображение их с помощью системы Grafana."
header:
  image: assets/images/visualisation-jinr-cloud/visualisation-jinr-cloud-scheme.png
  teaser: assets/images/visualisation-jinr-cloud/visualisation-jinr-cloud-scheme-th-min.png
sidebar:
  - title: "Место"
    image: assets/images/visualisation-jinr-cloud/jinr-logo.png
    image_alt: "logo"
    text: "Объединенный институт ядерных исследований (ОИЯИ), г. Дубна, МО"
  - title: "Роль"
    text: "Аналитик, архитектор, разработчик"
  - title: "Мой вклад"
    text: "Разработал систему для получения статистики использования вычислительных ресурсов облачной инфраструктуры ОИЯИ"
gallery:
  - url: assets/images/visualisation-jinr-cloud/visualisation-jinr-cloud-1-min.png
    image_path: assets/images/visualisation-jinr-cloud/visualisation-jinr-cloud-1-min.png
    alt: "Распределение ресурсов между кластерами"
  - url: assets/images/visualisation-jinr-cloud/visualisation-jinr-cloud-2-min.png
    image_path: assets/images/visualisation-jinr-cloud/visualisation-jinr-cloud-2-min.png
    alt: "Распределение ресурсов между департаментами"
  - url: assets/images/visualisation-jinr-cloud/visualisation-jinr-cloud-3-min.png
    image_path: assets/images/visualisation-jinr-cloud/visualisation-jinr-cloud-3-min.png
    alt: "Общая статистика потребления ресурсов"
---

Для визуализации статистики потребления ресурсов облака Объединенного института ядерных исследований ([ОИЯИ](http://jinr.ru)) мною была выбрана система [Grafana](http://grafana.com).
Она предоставляет удобный пользовательский интерфейс через веб-браузер для отображения различных метрик в реальном времени в виде наборов диаграмм и графиков.

В качестве хранилища измерений для сбора информации с системы OpenNebula была выбрана система [InfluxDB](http://influxdata.com).
Эта система является свободно-распространяемой базой данных предназначенной для хранения данных измерений с требованиями к высокой скорости и доступности. InfluxDB имеет простой и высокопроизводительный интерфейс для записи данных и запросов через HTTP(s).

{% include gallery caption="Разработанные дашбоарды" %}
