## Задание со * № 1
Добавил return ```{'Message': "mysql-instance created " + status +" restore-job"}``` в функцию mysql_on_create.
Значение status задается в зависимости от успешности создания job на восстанволение.

 Вывод команды kubectl describe mysqls.otus.homework mysql-instance | grep Status -A2
Status:
  mysql_on_create:
    Message:  mysql-instance created with restore-job

## Задание со * № 2
Написал функцию update_psswd, которая при применении манияеста с измененным паролем запускает job с изменением пароля в запущенном инстансе mysql,
а после обновляет deployment.
В результате после команды kubectl apply -f cr.yml сначала выполнится job restore-mysql-instance-job, о чем можно будет узнать из kubectl get jobs.
После этого k8s начнет запускать новый репликасет с обновленными перменными окружения. После успешного запуска старый репликасет удалится.


Вывод команды kubectl get jobs 

NAME                             COMPLETIONS   DURATION   AGE
backup-mysql-instance-job        1/1           10s        8m43s
pass-change-mysql-instance-job   1/1           15s        25m
restore-mysql-instance-job       1/1           3m20s      8m1s

Вывод команды kubectl exec -it $MYSQLPOD -- mysql -potuspassword_new -e "select * from test;" otus-database 

+----+-------------+
| id | name        |
+----+-------------+
|  1 | some data   |
|  2 | some data-2 |
+----+-------------+

