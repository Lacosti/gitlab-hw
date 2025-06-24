# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Филиппов Роман`

### Задание 1

`Установка, настройка и первый вход в Zabbix Server`

![Авторизация в админке](https://github.com/Lacosti/gitlab-hw/blob/main/img/login_Zabbix_Server.png) 

`Команды которые использовались для установки:`

```

wget https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.0+debian12_all.deb
dpkg -i zabbix-release_latest_7.0+debian12_all.deb
apt update 
apt install zabbix-server-pgsql zabbix-frontend-php php8.2-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix 
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix 
sudo nano /etc/zabbix/zabbix_server.conf
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2 

```
`При использовании команды sudo nano /etc/zabbix/zabbix_server.conf необходимо найти строки: DBPassword:, её необходимо разкомментировать и указать пароль, который устанавливали при установке, в моем случе был использован пароль: 123456. Также, необходимо изменить следующие строки в конфиге: Server:, ServerActive:, и Hostname:. Примеры ниже.`

```

DBPassword: 123456
Server=192.168.1.100
ServerActive=192.168.1.100
Hostname=Zabbix_Server

```


---

### Задание 2

`Установка Zabbix Agent на вторую ВМ.`

1. `Ранее мы установили Zabbix Agent на основную ВМ при устаноке Zabbix Server, поэтому, повторно устанавливать не нужно. Также, мы уже сделали настройку конфига на основной ВМ, более делать ничего не нужно.`
2. `Далее будут команды и скриншоты установки и настройки второй ВМ, так как ранее мы этого не делали.`


```

wget https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.0+debian12_all.deb
dpkg -i zabbix-release_latest_7.0+debian12_all.deb
apt update 
apt install zabbix-agent
systemctl restart zabbix-agent
systemctl enable zabbix-agent 
sudo nano /etc/zabbix/zabbix_agentd.conf
```

`Как и в первом задании необходимо изменить конфиг. Указываем все то же самое, что и на первой машине:`

```

DBPassword: 123456
Server=192.168.1.100
ServerActive=192.168.1.100
Hostname=Zabbix_Server

```


![Раздел конфигурации в веб-интерфесе с подключенными агентами](https://github.com/Lacosti/gitlab-hw/blob/main/img/Hosts_Agent.png)
![Раздел мониторинга, где видно поступающие данные от агента на второй ВМ](https://github.com/Lacosti/gitlab-hw/blob/main/img/Monitoring.png)
![Раздел мониторинга, где видно поступабщие данные от агента на сервере](https://github.com/Lacosti/gitlab-hw/blob/main/img/Server_Monitoring.png)
![Логи агента](https://github.com/Lacosti/gitlab-hw/blob/main/img/log.png)
