# zabbix_nginx_vts
Скрипты мониторинга zabbix для nginx-module-vts

**Требования**

[nginx](https://nginx.org/ru/) и установленный модуль [nginx-module-vts](https://github.com/vozlt/nginx-module-vts)



[nginx-plus-zabbix](https://github.com/sunrules/zabbix_nginx_vts)


**Установка**

 1. Добавить в /etc/zabbix/zabbix_agentd.d/userparameter_nginx_vts.conf

    UserParameter=nginx.stat.[*],/etc/zabbix/scripts/nginx-stats.py $1 $2 $3 $4 $5 $6 $7
    UserParameter=nginx.discovery[*],/etc/zabbix/scripts/nginx-discovery.py $1

 2. Перезапустить zabbix-agent
 3. Импортировать шаблон Zabbix
 4. Добавить в host макрос указывающий путь к url status  в формате json (!!!)
 {$URL_VTS_STATUS} например https://site.com/status/format/json
 
![macros](https://github.com/Vovanys/zabbix_nginx_vts/blob/master/img/macros.jpg?raw=true)

 5. Присоединить шаблон Nginx VTS к узлу сети
 6. Проверить наличие свежих данных

![lastdata](https://github.com/sunrules/zabbix_nginx_vts/blob/master/img/lastdata.jpg?raw=true)

![discovery](https://github.com/sunrules/zabbix_nginx_vts/blob/master/img/discovery.jpg?raw=true)
