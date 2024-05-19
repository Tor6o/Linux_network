## Part 1. Инструмент ipcalc

- #### **1.1. Сети и маски**

1) **адрес сети 192.167.38.54/13** узнаем с помощью команды ipcalc (для установки: `sudo apt install ipcalc`):

![address net](./linux net/ipcalc_1.png)

адрес сети:  **192.160.0.0**

**2.1) Перевод маски 255.255.255.0 в префиксную и двоичную запись**:

![prefix and binary](./linux net/ipcalc_2.1.png)

- Префиксная запись - **/24**
- Двоичная запись - **11111111.11111111.11111111.00000000**

**2.2) Перевод */15* в обычную и двоичную запись:**

![ordinary and binary 15](./linux net/ipcalc_2.2.png)

- Обычная запись - **255.254.0.0**
- Двоичная запись - **11111111.11111110.00000000.00000000**

**2.3) Перевод *11111111.11111111.11111111.11110000* в обычную и префиксную запись:** 

![from binary to ordinary and prefix](./linux net/ipcalc_2.3.png)

- Префиксная запись - **/28**

- Обычная запись - **255.255.255.240**

  

  **3.1) минимальный и максимальный хост в сети *12.167.38.4* при маске */8***:

![mask 8_max-min](./linux net/ipcalc_3.1.png)

- минимальный хост - **12.0.0.1**
- максимальный хост - **12.255.255.254**

**3.2) минимальный и максимальный хост в сети *12.167.38.4* при маске 11111111.11111111.00000000.00000000:** 

![mask 16_max-min](./linux net/ipcalc_3.2.png)

- минимальный хост - **12.167.0.1**

- максимальный хост - **12.167.255.254**

**3.3) минимальный и максимальный хост в сети *12.167.38.4* при маске 255.255.254.0**:

![mask 255.255.254.0_max-min](./linux net/ipcalc_3.3.png)

- минимальный хост - **12.167.38.1**
- максимальный хост - **12.167.39.254**

**3.4) минимальный и максимальный хост в сети *12.167.38.4* при маске  /4**:

![mask 4_max-min](./linux net/ipcalc_3.4.png)

- минимальный хост - **0.0.0.1**
- максимальный хост - **15.255.255.254**

#### **1.2. localhost**

**Определить можно ли обратиться к приложению, работающему на localhost, со следующими IP: *194.34.23.100*, *127.0.0.2*, *127.1.0.1*, *128.0.0.1***

- Localhost работает с адресами в диапазоне `127.0.0.1` — `127.255.255.254`, следовательно с IP **127.0.0.2** и **127.1.0.1** можно обратиться к приложению (о чём свидетельствует слово **loopback** в пункте **Hosts/Net**

![localhost_access granted](./linux net/localhost_1.png)

- Обратиться к приложению по **194.32.23.100 и 128.0.0.1** , работающему на localhost, нельзя:

![localhost_access denied](./linux net/localhost_2.png)

#### 1.3. Диапазоны и сегменты сетей

1) **Какие из перечисленных IP можно использовать в качестве публичного, а какие только в качестве частных: *10.0.0.45*, *134.43.0.2*, *192.168.4.2*, *172.20.250.4*, *172.0.2.1*, *192.172.0.1*, *172.68.0.2*, *172.16.255.255*, *10.10.10.10*, *192.169.168.1***

- **Публичным IP-адресом называется IP-адрес, под которым вас видят устройства в интернете, и он является уникальным во всей сети интернет.** Доступ к устройству с публичным IP-адресом можно получить из любой точки глобальной сети.

- **Частный или не анонсированный, внутренний IP-адрес** принадлежащий к специальному диапазону, не имеющему доступа к сети Интернет. Такие адреса предназначены для применения в локальных сетях, использование таких адресов контролируется только администраторами данных сетей.

- В строке Hosts/Net для частного IP будет выведено **Private Internet**:

![private and public ip_1](./linux net/private_public_ip_1.png)

![private and public_2](./linux net/private_public_ip_2.png)

![private and public_3](./linux net/private_public_ip_3.png)

![private and public_4](./linux net/private_public_ip_4.png)

- **Публичные сети:** 134.43.0.2, 172.0.2.1, *192.172.0.1*, 172.68.0.2, 192.169.168.1
- **Частные сети:** 10.0.0.45, 192.168.4.2, 172.20.250.4, 172.16.255.255, 10.10.10.10
2) **Какие из перечисленных IP адресов шлюза возможны у сети *10.10.0.0/18*: *10.0.0.1*, *10.10.0.2*, *10.10.10.10*, *10.10.100.1*, *10.10.1.255***

У сети **10.10.0.0/18** возможны следующие адреса: 10.10.0.2, 10.10.10.10, 10.10.1.255

![gateway address](./linux net/gateway adresses.png)

## Part 2. Статическая маршрутизация между двумя машинами

- С помощью команды `ip a` посмотреть существующие сетевые интерфейсы

  ![ws1_ip a command](./linux net/ws1_ipa.png)

![ws2_ip a command](./linux net/ws2_ipa.png)

**Описать сетевой интерфейс, соответствующий внутренней сети, на обеих машинах и задать следующие адреса и маски: ws1 - *192.168.100.10*, маска */16*, ws2 - *172.24.116.8*, маска */12***

- На каждой машине выполняем команду `sudo nano /etc/netplan/00-installer-config.yaml` и вносим соответствующие изменения.

  ![ws1 yaml config](./linux net/ws1_yaml_config.png)

![ws2 yaml config](./linux net/ws2_yaml_config.png)

- Выполняем команду `netplan apply` для перезапуска сервиса сети

![ws1 netplan apply command](./linux net/ws1_netplan apply.png)

![ws2 netplan apply command](./linux net/ws2_netplan apply.png)

#### 2.1. Добавление статического маршрута вручную

##### Добавить статический маршрут от одной машины до другой и обратно при помощи команды вида `ip r add` и пропинговать соединение между машинами

![ws1 ip add command](./linux net/ws1_auto add.png)

![ws2 ip add command](./linux net/ws2_auto add.png)

#### 2.2. Добавление статического маршрута с сохранением

- Перезапустить машины

- Добавить статический маршрут от одной машины до другой с помощью файла *etc/netplan/00-installer-config.yaml*

  - редактриуем файл для ws1:

    ![ws1 yaml configuration](./linux net/part2.2_ws1 yaml.png)

    - редактриуем файл для ws2:

      ![ws2 yaml configuration](./linux net/part2.2_ws2 yaml.png)

- Пропинговать соединение ws1 -> ws2:

  ![ws1 ping output](./linux net/part2.2_ws1 ping.png)

- Пропинговать соединение ws2 -> ws1:

  ![ws2 ping output](./linux net/part2.2_ws2 ping.png)

## Part 3. Утилита **iperf3**

#### 3.1. Скорость соединения

- Перевести и записать в отчёт: 8 Mbps в MB/s, 100 MB/s в Kbps, 1 Gbps в Mbps

8 Mbps = 1 MB/s

100 MB/s = 800 000 Kbps

1Gbps = 1 000 Mbps

#### 3.2. Утилита **iperf3**

- Измерить скорость соединения между ws1 и ws2

*Утилита iPerf3 позволяет измерить максимальную пропускную способность между двумя узлами сети.*

*По умолчанию тест выполняется в направлении от клиента к серверу. Для обратного тестирования от сервера к клиенту необходимо использовать ключ -R.*

- На первой машине выполняем команду `iperf3 -s`
- На второй машине: `iperf3 -c 192.168.100.10`

![ws1_iperf3_server](./linux net/iperf_ws1.png)

![ws2_iperf3_client](./linux net/iperf_ws2.png)

## Part 4. Сетевой экран

#### 4.1. Утилита iptables

Создаем файл */etc/firewall.sh*, имитирующий фаерволл, на ws1 и ws2:

- В файл добавляем подряд следующие правила:

1) на ws1 применить стратегию когда в начале пишется запрещающее правило, а в конце пишется разрешающее правило (это касается пунктов 4 и 5)

2) на ws2 применить стратегию когда в начале пишется разрешающее правило, а в конце пишется запрещающее правило (это касается пунктов 4 и 5)

3) открыть на машинах доступ для порта 22 (ssh) и порта 80 (http)

4) запретить echo reply (машина не должна "пинговаться”, т.е. должна быть блокировка на OUTPUT)

5) разрешить echo reply (машина должна "пинговаться")

- **ws1:**

![ws1_firewall.sh](./linux net/iptables1_ws1.png)

- **ws2:** 

![ws2_firewall.sh](./linux net/iptables1_ws2.png)

- Разница в стратегиях следующая: проверка правил идёт последовательно, сверху вниз. Соответственно, у ws1 первой идёт команда REJECT (Отклонить), и следующая команда уже не выполняется. У ws2 первая команда ACCEPT (Принять), т.е. следующая команда выполняется.

- Вывод команд на машинах ws1 и ws2:

![ws1_chmod](./linux net/iptables_ws1_chmod.png)

![ws2_chmod](./linux net/iptables_ws2_chmod.png)

#### 4.2. Утилита **nmap**

- Командой `ping` найти машину, которая не "пингуется", после чего утилитой nmap показать, что хост машины запущен.

  Проверка: в выводе nmap должно быть сказано: *Host is up*

- Ping в направлении w1->w2 проходит, но обратно w2->w1 - нет, так как для машины ws1 мы ранее написали REJECT.

![nmap_ping_ws1](./linux net/nmap_ws1_ping.png)

![nmap_ws2_ping](./linux net/nmap_ws2.png)

## Part 5. Статическая маршрутизация сети

#### 5.1 Настройка адресов машин

Настраиваем конфигурации машин в *etc/netplan/00-installer-config.yaml* согласно схеме. 

Перезапускаем сервис сети (`sudo netplan apply`). Если ошибок нет, то командой `ip -4 a` проверяем, что адрес машины задан верно.

- ws11:

![ws11_config.yaml](./linux net/part5_ws11_config.png)

- r1:

![r1_config.yaml](./linux net/part5_r1_config.png)

- r2: 

  ![r2_config.yaml](./linux net/part5_r2_config.png)

- ws22:

![ws22_config.yaml](./linux net/part5_ws22_config.png)

- ws21:

  ![ws21_config.yaml](./linux net/part5_ws21_config.png)

-  Также пропинговать ws22 с ws21. 

  ![ping_ws21](./linux net/part5_ws21_ping.png)

  ![ping_ws22](./linux net/part5_ws22_ping.png)

- Аналогично пропинговать r1 с ws11:

![ping ws11](./linux net/part5_ws11_r1_ping.png)

![ping_r1](./linux net/part5_r1_ping.png)

#### 5.2. Включение переадресации IP-адресов

- Для включения переадресации IP выполняем команду на роутерах:

`sysctl -w net.ipv4.ip_forward=1` *При таком подходе переадресация не будет работать после перезагрузки системы.*

![r1_sysctl_forwarding](./linux net/part5.2_r1_sysctl.png)

![r2_sysctl_forwarding](./linux net/part5.2_r2_sysctl.png)

- Открываем файл /etc/sysctl.conf и добавляем в него следующую строку:

`net.ipv4.ip_forward = 1` *При использовании этого подхода, IP-переадресация включена на постоянной основе.*

![r1_sysctl.conf_forwarding](./linux net/part5.2_r1_conf.png)

![r2_sysctl_forwarding](./linux net/part5.2_r2_conf.png)

#### 5.3. Установка маршрута по-умолчанию

Настраиваем маршрут по-умолчанию (шлюз) для рабочих станций. Для этого добавляем  `default` перед IP роутера в файле конфигураций и вызываем `ip r` чтобы показать, что добавился маршрут в таблицу маршрутизации.

- ws11:

![ws11_default_config](./linux net/part5.3_ws11_config.png)

- ws21:

  ![ws21_default_config](./linux net/part5.3_ws21_config.png)

- ws22: 

![ws22_default_config](./linux net/part5.3_ws22_config.png)

- Пропинговать с ws11 роутер r2 и показать на r2, что пинг доходит. 

  Для этого использовать команду: `tcpdump -tn -i`

![ws11_ping_output](./linux net/part5.3_ws11_ping.png)

![r2_ping_output](./linux net/part5.3_r2_ping.png)

#### 5.4. Добавление статических маршрутов

Добавляем роутеры r1 и r2 статические маршруты в файлы конфигураций.

- r1:

![r1_static route](./linux net/part5.4_r1_config.png)

- r2: 

![r2_static route](./linux net/part5.4_r2_config.png)

Вызываем `ip r` и показываем таблицы с маршрутами на обоих роутерах.

- r1:

![r1_ip r command output](./linux net/part5.4_r1_ipr.png)

- r2:

![r2_ip r command output](./linux net/part5.4_r2_ipr.png)

- Запускаем команды на ws11:

  `ip r list 10.10.0.0/18 и ip r list 0.0.0.0/0`

При наличии нескольких маршрутов выбирается тот, где длиннее маска, так как такой маршрут является более точным.

![ws11_ip r list output](./linux net/part5.4_ws11.png)

#### 5.5. Построение списка маршрутизаторов

- Запускаем на r1 команду дампа:

`tcpdump -tnv -i`

![r1_tcpdump output](./linux net/part5.5_traceroute_r1.png)

- При помощи `traceroute` строим список маршрутизаторов на пути от ws11 до ws21

  ![ws11_traceroute output](./linux net/part5.5_traceroute_ws11.png)

- Для определения промежуточных маршрутизаторов `traceroute` отправляет серию пакетов (по умолчанию 3) данных целевому узлу, при этом каждый раз увеличивая на 1 *значение поля TTL («время жизни»)*. Это поле обычно указывает максимальное количество маршрутизаторов, которое может быть пройдено пакетом. Первый пакет отправляется с TTL=1, и поэтому первый же маршрутизатор возвращает обратно *сообщение ICMP*, указывающее на невозможность доставки данных. Traceroute фиксирует адрес маршрутизатора, а также время между отправкой пакета и получением ответа (эти сведения выводятся на монитор компьютера). Затем traceroute повторяет отправку пакета, но уже с TTL=2, что позволяет первому маршрутизатору пропустить пакет дальше. Процесс повторяется до тех пор, пока при определённом значении TTL пакет не достигнет целевого узла. При получении ответа от этого узла процесс *трассировки (trace)* считается завершённым.

#### 5.6. Использование протокола ICMP при маршрутизации

- Запускаем на r1 перехват сетевого трафика:

![r1_tcpdump output](./linux net/part5.6_r1_tcpdmp.png)

- Пропинговать с ws11 несуществующий IP (например, 10.30.0.111) с помощью команды:

![ws11_wrong ip ping output](./linux net/part5.6_ws11_ping.png)

## Part 6. Динамическая настройка IP с помощью **DHCP**

- Для r2 настроить в файле */etc/dhcp/dhcpd.conf* конфигурацию службы DHCP:

![dhcp_configuration](./linux net/part6_dhcp conf.png)

- В файле resolv.conf прописать `nameserver 8.8.8.8` :

![resolv.conf edit](./linux net/part6_resolv.png)

- Перезагрузить службу DHCP командой `systemctl restart isc-dhcp-server` :

![dhcp restart](./linux net/part6_dhcp restart.png)

- Машину ws21 перезагрузить при помощи `reboot` и через `ip a` показать, что она получила адрес. Также пропинговать ws22 с ws21:

![ws21_ip a output and ws22 ping](./linux net/part6_ip a and ping.png)

![ws22_ws21 ping](./linux net/part6_ws22 ping.png)

- Указать MAC адрес у ws11, для этого в *etc/netplan/00-installer-config.yaml* надо добавить строки: `macaddress: 10:10:10:10:10:BA, dhcp4: true`

![ws11_config netplan](./linux net/part6_netplan.png)

- Для r1 настроить аналогично r2, но сделать выдачу адресов с жесткой привязкой к MAC-адресу (ws11). Провести аналогичные тесты:

  ![r1 dhcp configuration](./linux net/part6_r1 dhcp config.png)

- Меняем nameserver и перезагружаем службу dhcp: 

![r1 resolv configuration](./linux net/part6_r1 resolv conf.png)

![ws11 updated ip a](./linux net/part6_ws11-r1.png)

- Запросить с ws21 обновление ip адреса, `sudo dhclient -r`:

  ![dhclient killed output](./linux net/part6_dhclient killed.png)

  - `sudo dhclient`:

![dhclient updated output](./linux net/part6_dhclient updated.png)

- Команда `sudo dhclient -r` - удаление имеющегося адреса.


- Команда `sudo dhclient` - присвоение нового адреса.

## Part 7. **NAT**

- В файле */etc/apache2/ports.conf* на ws22 и r1 изменить строку `Listen 80` на `Listen 0.0.0.0:80`, то есть сделать сервер Apache2 общедоступным. Запустить веб-сервер Apache командой `service apache2 start` на ws22 и r1:

- ws22 изменение файла */etc/apache2/ports.conf* и запуск сервера Apache:

  ![ws22 apache2 ports configuration](./linux net/part7.1_apache2.png)

- r1 изменение файла */etc/apache2/ports.conf* и запуск сервера Apache:

![r1 apache2 ports configuration](./linux net/part7.1_apache2r1.png)

- Добавить в фаервол, созданный по аналогии с фаерволом из Части 4, на r2 следующие правила:


1) удаление правил в таблице filter `- iptables -F`


2) удаление правил в таблице "NAT" `- iptables -F -t nat`


3) отбрасывать все маршрутизируемые пакеты `- iptables --policy FORWARD DROP`

Запускать файл также, как в Части 4 :

![r2 firewall.sh edit](./linux net/part7.1 firewall sh.png)

- Проверить соединение между ws22 и r1 командой `ping` *При запуске файла с этими правилами, ws22 не должна "пинговаться" с r1*:

![ws22 no ping](./linux net/part7.1_no ping ws22.png)

- Добавить в файл ещё одно правило:

  4) разрешить маршрутизацию всех пакетов протокола ICMP

  Запускать файл также, как в Части 4:

  ![r2 firewall edited](./linux net/part7.1 firewall edited_1.png)

- Проверить соединение между ws22 и r1 командой `ping`.  *При запуске файла с этими правилами, ws22 должна "пинговаться" с r1*:

  ![ws22 ping success](./linux net/part7.1_ping ws22.png)

- Добавить в файл ещё два правила:

  5. включить SNAT, а именно маскирование всех локальных ip из локальной сети, находящейся за r2 (по обозначениям из Части 5 - сеть 10.20.0.0)

  6. включить DNAT на 8080 порт машины r2 и добавить к веб-серверу Apache, запущенному на ws22, доступ извне сети
  
     ![firewall edited with 2 new rules](./linux net/part7_firewall edited.png)
  
     - Проверить соединение по TCP для DNAT, для этого с r1 подключиться к серверу Apache на ws22 командой `telnet` (обращаться по адресу r2 и порту 8080): 

![telnet_r1 output](./linux net/part7_last one.png)

- Проверить соединение по TCP для SNAT, для этого с ws22 подключиться к серверу Apache на r1 командой: `telnet 10.100.0.11 80:` 

![ws22_telnet output](./linux net/part7_last one double.png)

## Part 8. Дополнительно. Знакомство с **SSH Tunnels**

- Меняем настройки портов в /etc/apache2/ports.conf и делаем рестарт службы apache2:

![](./linux net/part8_ports conf.png)

- Воспользоваться *Local TCP forwarding* с ws21 до ws22, чтобы получить доступ к веб-серверу на ws22 с ws21:

  ![ssh localhost apache2](./linux net/part8_ssh forwarding.png)

- Воспользоваться *Remote TCP forwarding* c ws11 до ws22, чтобы получить доступ к веб-серверу на ws22 с ws11

  ![ssh -R output](./linux net/Part8_lastone.png)

  ![ws11 ssh r output](./linux net/part8_ws11 ssh r.png)

- Для проверки, сработало ли подключение в обоих предыдущих пунктах, перейдите во второй терминал (например, клавишами Alt + F2) и выполните команду:

  `telnet 127.0.0.1 80`

  ![telnet final check](./linux net/part8_telnet.png)

- Делаем то же самое для ws11: 

  ![ws11_telnet output](./linux net/part8_telnet_ws11.png)
