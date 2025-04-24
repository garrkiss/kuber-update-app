# Домашнее задание к занятию "`Домашнее задание к занятию «Обновление приложений»`" - `Бакулев Евгений`

## Задание 1. Выбрать стратегию обновления приложения и описать ваш выбор

## Выбрана стратегия Blue/Green для обновления приложения:

Стратегия Blue/Green deployment выбрана для мажорного обновления приложения по следующим причинам:

1. **Совместимость с мажорным обновлением**  
   Blue/Green позволяет развернуть новую версию приложения (Green) отдельно от старой (Blue). Трафик переключается на Green только после успешного развертывания, что исключает проблемы несовместимости версий, так как старая и новая версии не работают одновременно.

2. **Минимальный downtime**  
   Переключение трафика между Blue и Green средами происходит мгновенно, минимизируя или полностью исключая время простоя.

3. **Управление ограниченными ресурсами (запас 20%)**  
   Blue/Green требует развертывания нового окружения, удваивая количество реплик на время обновления. Для преодоления ограничения по ресурсам:
   - Оптимизируем ресурсы, временно снижая количество реплик в Blue-окружении до минимально необходимого для поддержания доступности.
   - Используем постепенное масштабирование Green-окружения, запуская новые реплики по мере освобождения ресурсов.

4. **Простота отката**  
   Если Green-версия окажется нестабильной, трафик можно мгновенно переключить обратно на Blue-окружение, обеспечивая высокую надежность.

5. **Тестирование перед релизом**  
   Green-окружение можно протестировать перед переключением трафика, чтобы убедиться в его стабильности.

## Задание 2. Обновить приложение

- [Манифест Deployment](https://github.com/garrkiss/kuber-update-app/blob/main/manifest/task1/deployment.yaml)

### Демонстрация
Вывели history после всех обновлений, изменил образ nginx на 1.29 для создания проблемы, версию которую 1.28 по заданию, нужно указать, уже выпустили и без проблем приложение стартует.
![Ссылка](https://github.com/garrkiss/kuber-update-app/blob/main/img/task1/1.png)

Вывод подов, что есть ошибка с образом 1.29
![Ссылка](https://github.com/garrkiss/kuber-update-app/blob/main/img/task1/2.png)

Откат к 1.28
![Ссылка](https://github.com/garrkiss/kuber-update-app/blob/main/img/task1/3.png)

## Задание 3*. Создать Canary deployment

- [Манифест Nginx configmap v1](https://github.com/garrkiss/kuber-update-app/blob/main/manifest/taks2/nginx-v1-configmap.yaml)
- [Манифест Nginx configmap v2](https://github.com/garrkiss/kuber-update-app/blob/main/manifest/taks2/nginx-v2-configmap.yaml)
- [Манифест Nginx deployment v1](https://github.com/garrkiss/kuber-update-app/blob/main/manifest/taks2/nginx-v1-deployment.yaml)
- [Манифест Nginx deployment v2](https://github.com/garrkiss/kuber-update-app/blob/main/manifest/taks2/nginx-v2-deployment.yaml)
- [Манифест Nginx service v1](https://github.com/garrkiss/kuber-update-app/blob/main/manifest/taks2/nginx-v1-service.yaml)
- [Манифест Nginx service v2](https://github.com/garrkiss/kuber-update-app/blob/main/manifest/taks2/nginx-v2-service.yaml)
- [Манифест Ingress](https://github.com/garrkiss/kuber-update-app/blob/main/manifest/taks2/ingress.yaml)
- [Манифест Ingress Canary 20%](https://github.com/garrkiss/kuber-update-app/blob/main/manifest/taks2/ingress-canary.yaml)

### Демонстрация работы

Применяем и проверяем состояние
![Ссылка](https://github.com/garrkiss/kuber-update-app/blob/main/img/task2/1.png)

Видим, что обновляя страницу перекидывает между версиями, 20% трафика на версию v2
![Ссылка](https://github.com/garrkiss/kuber-update-app/blob/main/img/task2/2.png)
![Ссылка](https://github.com/garrkiss/kuber-update-app/blob/main/img/task2/3.png)