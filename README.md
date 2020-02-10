# flashick_platform
flashick Platform repository

## HW 10.12.2019:
 - Создан манифест для запуска простого вебсервера с использованием init-контейнера
 - Добавлены переменные окружения в манифест пода hipster-frontend для его корректного запуска 

 ## HW 17.12.2019:
 - Созданы манифесты ReplicaSet и Deployment для сервисов frontend и paymentservice
 - Создан манифест DaemonSet для node-exporter


 ## HW 24.12.2018
 - Созданы манифесты для сервисных аккаунтов

 ## HW 09.01.2020
 - Созданы манифесты для разворачивание сервисов с использованием ClusterIp, IPVS и Loadbalancer/Ingress
 - Создан сервис LoadBalancer, открывающий доступ к CoreDNS снаружи кластера
 - Через ingress предоставлен доступ для kubernetes-dashboard 
 - Реализовано канареечное развертывание через ingress

 ## HW 24.12.2019
 - Созданы манифесты для сервисных аккаунтов

 ## HW 21.01.2020
 - Изучена установка helm чартов вручную и с помощтю helmfile
 - Изучена шаблонизация с использованием kubecfg
 - Установлен chart chartmuseum :)
 - С помощью helmfile развернуты сервисы ingress, cert-manager и harbor 
 - Создан чарт hipster-shop
 - Из чарта hipster-shop frontend вынесен в отдельный чарт, добавленный в зависимости
 - Из чарта hipster-shop redis вынесен в зависимости
 - Из чарта hipster-shop shippingservice и paymentservice вынесены в отдельный чарт

