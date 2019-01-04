---
title: "Автоматизированная система для контроля перемещения робота в пространстве с препятствиями"
author_profile: false
excerpt: "Разработка моделей используемых в моей магистерской работе в университете."
header:
  image: assets/images/robot-motion/robot-motion-main-min.png
  teaser: assets/images/robot-motion/robot-motion-main-min.png
sidebar:
  - title: "Место"
    image: assets/images/robot-motion/pnrpu-logo-min.png
    image_alt: "logo"
    text: "Пермский научно-исследовательский политехнический университет (ПНИПУ), г. Пермь"
  - title: "Роль"
    text: "Студент"
  - title: "Мой вклад"
    text: "Написание научно-исследовательской работы"
gallery:
  - url: assets/images/robot-motion/robot-motion-1-min.png
    image_path: assets/images/robot-motion/robot-motion-1-min.png
    alt: "Робот манипулятор пробует обойти препятствия"
  - url: assets/images/robot-motion/robot-motion-2-min.jpg
    image_path: assets/images/robot-motion/robot-motion-2-min.jpg
    alt: "Прототип реального робота"
  - url: assets/images/robot-motion/robot-motion-3-min.png
    image_path: assets/images/robot-motion/robot-motion-3-min.png
    alt: "Модель робота с видимыми соединителями"
---

На последнем курсе обучения в университете я решил помочь своей кафедре создать модель которая бы имитировала реальную систему, применяемую для сканирования деталей с нескольких сторон. Основной целью работы был поиск подходящего алгоритма для перемещения частей робота по заданному маршруту без столкновений друг с другом и с предметами окружающего мира.

Я использовал систему [V-REP][v-rep] для разработки моделей и решения задач прямой и обратной кинематики совместного перемещения двух роботов.
На изображении в выше представлена эта 3D-модель состоящая из двух основных роботов - робота-манипулятора (обозначен "1") и робота-рамки ("2").
Также там представлены робот-подставка ("3"), который поворачивает сканируемую деталь ("4").

Возможно эта работа не для IT и разработчика, но благодаря ей я изучил много научной литературы, что позволило мне сделать хорошую научно-исследовательскую работу, которую я представил научному комитету в университете на защите.  
Также я изучил основы языка программирования Lua.

Я выгрузил исходники кода проекта на [Github](https://github.com/RuslanGainanov/robot-motion).
А в данном видео вы можете увидеть процесс сканирования детали:
<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/yxw9i_CwjF0" frameborder="0" allowfullscreen></iframe>

{% include gallery %}
