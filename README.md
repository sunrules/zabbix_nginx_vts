# zabbix_nginx_vts
Скрипты мониторинга zabbix для nginx-module-vts.

В данном форке проекта Vovanys/zabbix_nginx_vts внесены некоторые изменения:
1. Экспорт template из версии Zabbix 4.4;
2. В tamplate:
   Добавлен триггер для проверки статуса сервиса nginx;
   Внесены изменения в макросы, item, item prototype, добавлены графики.
3. Скрипты переделаны на Python3;
4. Настройка Nginx описана для ОС FreeBSD 11.4.

**Требования**

[nginx](https://nginx.org/ru/) и установленный модуль [nginx-module-vts](https://github.com/vozlt/nginx-module-vts)
[nginx-plus-zabbix](https://github.com/sunrules/zabbix_nginx_vts)


**Установка**

1. Сборка Nginx
./configure --prefix=/usr/local/nginx \
--sbin-path=/usr/local/nginx/sbin/nginx \
--conf-path=/usr/local/nginx/conf/nginx.conf \
--error-log-path=/usr/local/nginx/log/error.log \
--http-log-path=/usr/local/nginx/log/access.log \
--user=www \
--group=www \
--with-http_stub_status_module \
--add-module=../nginx-module-vts-0.1.18 
make -j4
sudo make  install

2. Добавить в секцию http 
nginx.conf

http {
vhost_traffic_status_zone;

3. Добавить в конфигурационный файл, можно default.conf

location /nginx_vts_status {
    # Включаем отображение статуса
    vhost_traffic_status_display;
    vhost_traffic_status_display_format html;
    # Не писать в лог эти запросы
    access_log off;
    # Разрешить доступ с указанных адресов
    allow XX.XX.XX.XX; указать IP адрес данного сервера, нужно для python скрипта 
    allow XX.XX.XX.XX; указать IP адрес компьютера администратора для просмотра dashboard /nginx_vts_status
    deny all;
 }
 
4. Проверяем и запускаем nginx
/usr/local/etc/rc.d/nginx configtest
/usr/local/etc/rc.d/nginx start
4. Добавить в /usr/local/etc/zabbix44/zabbix_agentd.conf.d/userparameter_nginx_vts.conf
 
   UserParameter=nginx.stat[*],/usr/local/etc/zabbix5/scripts/nginx-stats.py $1 $2 $3 $4 $5 $6 $7
   UserParameter=nginx.discovery[*],/usr/local/etc/zabbix5/scripts/nginx-discovery.py $1

 5. Перезапустить zabbix-agent
 6. Импортировать шаблон в Zabbix
 7. В Макросе template можно изменить порт web сервера, в данном случае установлен 80.
 8. Присоединить шаблон Nginx VTS к узлу сети
 9. Проверить наличие свежих данных
 
 Discovery должен найти все существующие vhost и upstream

![vts_status_boar](https://github.com/sunrules/zabbix_nginx_vts/blob/master/img/nginx_vts_status_board.jpg?raw=true)

![lastdata](https://github.com/sunrules/zabbix_nginx_vts/blob/master/img/lastdata.jpg?raw=true)

![discovery](https://github.com/sunrules/zabbix_nginx_vts/blob/master/img/discovery.jpg?raw=true)
