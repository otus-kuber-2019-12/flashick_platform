- создал кастомный образ nginx со своим конфигом 
- создал деплоймент и сервис для своего образа вместе с nginx-exporter
- выкатил деплоймент и сервис в неймспейс monitoring
- с помощью helm 3 поставил kubernetes-operator
- выкатил сервси монитор для дискавери nginx-exporter
- добавил в графану дашборд для nginx-exporter
Результат:

![nginx.png](/kubernetes-monitoring/nginx.png)
