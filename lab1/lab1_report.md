# Лабораторная работа 1. Обзор Google Cloud и исследование основных сервисов

**University:** [ITMO University](https://itmo.ru/ru/)  
**Faculty:** [FICT](https://fict.itmo.ru)  
**Course:** [Cloud platforms as the basis of technology entrepreneurship](https://itmo-ict-faculty.github.io/cloud-platforms-as-the-basis-of-technology-entrepreneurship/)  
**Year:** 2025/2026  
**Group:** U4125  
**Author:** Vladimir Novikov  
**Lab:** Lab 1  
**Date of create:** 30.04.2026  
**Date of finished:** 05.04.2026

# Лабораторная работа 1. Обзор Google Cloud и исследование основных сервисов

**University:** [ITMO University](https://itmo.ru/ru/)  
**Faculty:** [FICT](https://fict.itmo.ru)  
**Course:** [Cloud platforms as the basis of technology entrepreneurship](https://itmo-ict-faculty.github.io/cloud-platforms-as-the-basis-of-technology-entrepreneurship/)  
**Year:** 2025/2026  
**Group:** U4125  
**Author:** Vladimir Novikov  
**Lab:** Lab 1  
**Date of create:** 05.05.2026  
**Date of finished:** 05.05.2026

## Цель работы
Ознакомиться с основными возможностями Google Cloud, научиться создавать Service Account, виртуальную машину и работать с Cloud Storage.

## Ход работы

1. Я заполнил Google-форму и получил доступ к проекту в Google Cloud.

2. Установил Google Cloud CLI (gcloud) на Windows 10.

3. Создал Service Account `vnovikov-sa-lab1` с ролью **Storage Admin**.

4. Создал виртуальную машину `vnovikov-vm-lab1` типа e2-micro в режиме Spot.

5. Подключился к VM по SSH и с помощью команды `gsutil cp` скопировал 3 файла из бакета `lab1-bucket-itmo` на машину. Командой `ls -lah` убедился, что файлы появились.

6. Изменил роль Service Account на **Compute Viewer**. При попытке повторного копирования файлов появилась ошибка доступа. Вывод: права доступа важны, Compute Viewer не даёт работать с хранилищем файлов.

7. Удалил все созданные ресурсы (VM и Service Account).

## Выводы
Google Cloud удобная платформа. Важно правильно управлять правами доступа (IAM) и не забывать удалять ресурсы после работы, чтобы не тратить деньги.

