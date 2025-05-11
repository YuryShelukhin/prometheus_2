# Домашнее задание к занятию «Prometheus. Часть 2» Шелухин Юрий  

### Задание 1
Создайте файл с правилом оповещения, как в лекции, и добавьте его в конфиг Prometheus.

### Требования к результату
- [ ] Погасите node exporter, стоящий на мониторинге, и прикрепите скриншот раздела оповещений Prometheus, где оповещение будет в статусе Pending

---

## Решение 1

1-2.  Создадим  пользователя prometheus.   
sudo useradd --no-create-home --shell /bin/false prometheus (без домашней директории)
3. Скачаем prometheus  
wget https://github.com/prometheus/prometheus/releases/download/v2.53.4/prometheus-2.53.4.linux
-386.tar.gz  
извлекем архив  
~/Downloads$ tar vxf prometheus-2.53.4.linux-386.tar.gz  
создадим директории 
mkdir /etc/prometheus  
mkdir /var/lib/prometheus  
скопируем утилиты
cp ./prometheus-2.53.4.linux-386/prometheus /usr/local/bin  
cp ./prometheus-2.53.4.linux-386/promtool /usr/local/bin  
cp -R ./prometheus-2.53.4.linux-386/consoles/ /etc/prometheus/
cp -R ./prometheus-2.53.4.linux-386/console_libraries/ /etc/prometheus/
cp ./prometheus-2.53.4.linux-386/prometheus.yml /etc/prometheus/  
проверим
ls -l /etc/prometheus  
<img src = "img/1-1.png" width = 60%>  
передадим права на эти файлы пользователю prometheus
sudo chown -R prometheus:prometheus /etc/prometheus/ /var/lib/prometheus/  
sudo chown prometheus:prometheus /usr/local/bin/prometheus  
sudo chown prometheus:prometheus /usr/local/bin/promtool  

4. Запустим и проверим результат
sudo /usr/local/bin/prometheus --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path /var/lib/prometheus/ --web.console.templates=/etc/prometheus/consoles --web.console.libraries=//etc/prometheus/console_libraries  
<img src = "img/1-2.png" width = 60%>  
Запустим в браузере адрес (ip a) с портом 9090  
<img src = "img/1-3.png" width = 60%>  

5. Создадим сервис
vim /etc/systemd/system/prometheus.service  
sudo systemctl enable prometheus.service  
sudo systemctl start prometheus.service  
sudo systemctl status prometheus.service  
Не запустился, нет доступа. Проверим права.  
ls -l /usr/local/bin/prometheus  
ls -l /usr/local/bin/promtool  
ls -ld /var/lib/prometheus  
ls -ld /etc/prometheus/  
ls -l /etc/prometheus/  
директория и права принадлежат пользователю prometheus
При проверке ls -l /etc/prometheus/ выявим отсутствие таких прав.  
<img src = "img/1-4.png" width = 60%>  
Изменим права.   
sudo chown  -R prometheus:prometheus /var/lib/prometheus  
ok
запустим сервис. не запустился. порт занят. исправим
sudo lsof -i :9090  
sudo kill 46499
перезапустим сервис. проверим. ок
<img src = "img/1-4.png" width = 60%> 

---

### Задание 2
Установите Alertmanager и интегрируйте его с Prometheus.

### Требования к результату
- [ ] Прикрепите скриншот Alerts из Prometheus, где правило оповещения будет в статусе Fireing, и скриншот из Alertmanager, где будет видно действующее правило оповещения

---

### Задание 3

Активируйте экспортёр метрик в Docker и подключите его к Prometheus.

### Требования к результату
- [ ] приложите скриншот браузера с открытым эндпоинтом, а также скриншот списка таргетов из интерфейса Prometheus.*

---

### Задание 4* (со звездочкой)

Создайте свой дашборд Grafana с различными метриками Docker и сервера, на котором он стоит.

---

