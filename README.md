# Домашнее задание к занятию «Prometheus. Часть 2» Шелухин Юрий  

### Задание 1
Создайте файл с правилом оповещения, как в лекции, и добавьте его в конфиг Prometheus.

### Требования к результату
- [ ] Погасите node exporter, стоящий на мониторинге, и прикрепите скриншот раздела оповещений Prometheus, где оповещение будет в статусе Pending

---

## Решение 1

1.  Скачаем ПО с github
wget https://github.com/prometheus/alertmanager/releases/download/v0.28.1/alertmanager-0.28.1.linux-386.tar.gz
2.  Извлекем архив  
tar vxf alertmanager-0.28.1.linux-386.tar.gz  
3.  Скопируем утилиты  
sudo cp ./alertmanager /usr/local/bin  
sudo cp ./amtool /usr/local/bin  
sudo cp ./alertmanager.yml /etc/prometheus  
4.  Передадим права на эти файлы пользователю prometheus  
sudo chown -R prometheus:prometheus /etc/prometheus/alertmanager.yml  
5. Создадим сервис  
sudo vim /etc/systemd/system/prometheus-alertmanager.service
6. Запустим сервис  
sudo systemctl enable prometheus-alertmanager.service  
sudo systemctl start prometheus-alertmanager.service   
sudo systemctl status pprometheus-alertmanager.service     
<img src = "img/1-1.png" width = 60%>  

7.  Проверим по адресу: http://192.168.65.135:9093/#/alerts
<img src = "img/1-2.png" width = 60%>  

8. Настроим prometheus для работы с alertmanager  
sudo vim /etc/prometheus/prometheus.yml  
<img src = "img/1-3.png" width = 60%>      

9. Перезапустим prometheus  
<img src = "img/1-4.png" width = 60%>  

10. Создадим правило оповещения. Создадим файл.  
sudo vim /etc/prometheus/netology-test.yml

11. Подключим правило в prometheus, перезапустим.
<img src = "img/1-5.png" width = 60%>   

12. Отредактируем сonfig-файл Alertmanager.Перезапустим.  
sudo nvim /etc/prometheus/alertmanager.yml
<img src = "img/1-6.png" width = 60%>  

13. Проверим интерфейс alertmanager и prometheus
<img src = "img/1-7.png" width = 60%>   
<img src = "img/1-8.png" width = 60%>  
<img src = "img/1-9.png" width = 60%>  

14. Остановим node_exporter (9100)  
sudo systemctl stop node-exporter
systemctl status node-exporter
<img src = "img/1-10.png" width = 60%>  
<img src = "img/1-11.png" width = 60%>  
<img src = "img/1-12.png" width = 60%>  

---

### Задание 2
Установите Alertmanager и интегрируйте его с Prometheus.

### Требования к результату
- [ ] Прикрепите скриншот Alerts из Prometheus, где правило оповещения будет в статусе Fireing, и скриншот из Alertmanager, где будет видно действующее правило оповещения

---

## Решение 2
<img src = "img/2-1.png" width = 60%>  
<img src = "img/2-2.png" width = 60%> 

---

### Задание 3

Активируйте экспортёр метрик в Docker и подключите его к Prometheus.

### Требования к результату
- [ ] приложите скриншот браузера с открытым эндпоинтом, а также скриншот списка таргетов из интерфейса Prometheus.*

---

## Решение 3
1. Установим docker
sudo apt update  
sudo apt install curl software-properties-common ca-certificates apt-transport-https -y  
sudo install -m 0755 -d /etc/apt/keyrings   
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc  
sudo chmod a+r /etc/apt/keyrings/docker.asc  
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null  
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin  
sudo docker run hello-world  
sudo systemctl enable docker    
2. Создадим файл для оверрайда    
sudo vim /etc/docker/daemon.json    
Перезапустим docker.    
<img src = "img/3-1.png" width = 60%>   

3. Проверим интерфейс  
<img src = "img/3-2.png" width = 60%>    

4. Добавим таргет в prometheus, перезапустим.  
<img src = "img/3-3.png" width = 60%>    

5. Проверим в браузере.  
<img src = "img/3-4.png" width = 60%>     

---


### Задание 4* (со звездочкой)

Создайте свой дашборд Grafana с различными метриками Docker и сервера, на котором он стоит.

---

