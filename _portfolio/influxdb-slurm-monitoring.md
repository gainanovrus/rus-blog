---
title: "Система мониторинга вычислительного кластера на Docker и SLURM"
author_profile: false
excerpt: >-
  Разработка и развертывание системы для сбора, хранения и визуализации системных параметров вычислительного кластера.
header:
  image: assets/images/influxdb-slurm-monitoring/influxdb-slurm-monitoring-banner.png
  teaser: assets/images/influxdb-slurm-monitoring/influxdb-slurm-monitoring-teaser.png
sidebar:
  - title: "Место"
    image: assets/images/influxdb-slurm-monitoring/icmm_logo-min.png
    image_alt: "logo"
    text: "Институт механики сплошных сред УрО РАН (ИМСС УрО РАН), г. Пермь"
  - title: "Роль"
    text: "Инженер DevOps"
  - title: "Мой вклад"
    text: "Разработал систему для мониторинга очередей задач, загруженности вычислительных узлов кластера и отображения истории изменений системной статистики кластера SLURM"
gallery:
  - url: assets/images/influxdb-slurm-monitoring/influxdb-slurm-monitoring-scheme.png
    image_path: assets/images/influxdb-slurm-monitoring/influxdb-slurm-monitoring-scheme.png
    alt: "Схема SLURM кластера"
  - url: assets/images/influxdb-slurm-monitoring/grafana-all-panels.png
    image_path: assets/images/influxdb-slurm-monitoring/grafana-all-panels.png
    alt: "Пример дашбоарда для отображения статистики"
---
[Институт механики сплошных сред УрО РАН][ICMM UB RAS] имеет вычислительный кластер на базе суперкомпьютера состоящего из около 50 узлов с >400 CPU и >1 ТБ оперативной памяти.
Он используется в научной работе различными подразделениями института для построения математических моделей и расчета различных параметров.

В будущем Институт планирует обновить свою инфраструктуру и виртуализировать свои вычислительные ресурсы для снижения зависимостей физического уровня аппаратуры. В кластере используется менеджер задач [SLURM][SLURM] для запуска программ на вычислительных узлах.
Благодаря нему многие сотрудники Института могут запускать одновременно и независимо друг от друга свои задачи.

И я должен был разобраться с этой системой. Моей основной задачей было написание скриптов для развертывания виртуальной вычислительной инфраструктуры на базе Docker [Docker][Docker]. Затем мне требовалось разработать и сконфигурировать системы мониторинга системной статистики в реальном времени.

Для развертывания виртуального кластера был написан скрипт для docker-compose.
Его подробности выложены на [GitHub][docker-slurmbase].

Для мониторинга я использовал стек [InfluxDB](http://influxdata.com)+[Grafana](http://grafana.com),
Я его хорошо знаю по своей предыдущей [работе][visualisation-jinr-cloud].
Сбор данных о запущенных задачах происходил с использованием плагина-профилировщика HDF5.
Этот плагин был основан на работе пользователя [cfenoy][cfenoy].
Я его модифицировал для работы с последней версией SLURM и добавил сбор нескольких дополнительных полей.
Также был создан и настроен дашбоард в системе Grafanf, который помогает наблюдать всю статистику кластера в браузере в реальном-времени.

----------------------------------------------------------------------
### docker-slurmbase:

[https://github.com/GRomR1/docker-slurmbase](https://github.com/GRomR1/docker-slurmbase)


### influxdb-slurm-monitoring:

[https://github.com/GRomR1/influxdb-slurm-monitoring](https://github.com/GRomR1/influxdb-slurm-monitoring)

----------------------------------------------------------------------

{% include gallery %}

[ICMM UB RAS]: https://www.icmm.ru/en/about-institute
[SLURM]: https://slurm.schedmd.com/overview.html
[Docker]: https://www.docker.com/
[visualisation-jinr-cloud]: _portfolio/visualisation-jinr-cloud.md
[Swarm]: https://docs.docker.com/engine/swarm/
[stack]: https://docs.docker.com/engine/reference/commandline/stack/
[docker-slurmbase]: https://github.com/GRomR1/docker-slurmbase
[influxdb-slurm-monitoring]: https://github.com/GRomR1/influxdb-slurm-monitoring
[cfenoy]: https://github.com/cfenoy/influxdb-slurm-monitoring
