---
layout: post
title: Docker compose with django rabbitmq nginx mysql and celery
---

Melakukan deployment dari django sebagai aplikasi utama, rabbitmq untuk message broker, nginx sebagai file static server,
mysql sebagai database dan celery untuk async proses menggunakan docker

Diagram untuk masing - masing stack kurang lebih seperti ini: 

![Create instance 1]({{ site.url }}/images/drcmnstack.png)

# Django

