**Vyos. Основы**

**1 Установка системы**

Для начала, создаем виртуальную машину с VyOS и подключаем туда ISO-образ с VyOS. Загружаемся с ISO-образа, во время появления окошка входа вводим учетные данные **vyos:vyos** и попадаем в систему, но она еще не установлена на диск. Для установки требуется перейти в режим root используя команду **sudo su -** и ввести команду **install image** для начала установки системы.

После этого, нажимаем просто enter для тестовой установки, протыкиваем до момента, где нам предложат удалить все данные с диска, вводим туда **Yes,** как показано на рисунке 1.


Рисунок 1 - удаление всех данных с диска

Далее, просто продолжаем нажимать enter. Не забываем ввести пароль от пользователя vyos, как представлено на рисунке 2:


Рисунок 2 - ввод пароля администратора

Продолжаем жать enter!) 

После завершения установки перезагружаемся и отключаем iso-образ для проверки того, что система запущена.

**2 Режим конфигурации и режим операций**

Для того, чтобы зайти в режим конфигурации, требуется ввести команду **configure.** На рисунке 3 представлено приглашение из режима конфигурации.


Рисунок 3 - режим конфигурации

Выйдя из режима конфигурации, можно попасть в режим операций. Отсюда выполняются различные действия, НЕ СВЯЗАННЫЕ с настройкой сервера.

**Сохранение настроек**

Для того, чтобы сохранить настройки на VyOS, требуется ввести сначала команду **commit,** а потом команду **save.**

**2.1 Настройка сетевого интерфейса**

Для настройки сетевого интерфейса сначала надо узнать существующие у нас интерфейсы. Сделать это прямо из режима конфигурации, используя линуксовую команду **ip a.** Вывод этой команды представлен на рисунке 4.

Рисунок 4 - вывод команды ip a

После этого можно приступать к полноценной настройке сетевого интерфейса. Сделать это можно только из режима конфигурации, обращайте на это внимание. Для настройки получения ip-адреса по dhcp на интерфейсе eth0 требуется ввести команду, которая представлена ниже.

**set interface ethernet eth0 address dhcp**

где **eth0 -** название интерфейса, на который мы хотим назначить адрес, а **dhcp** - вариант назначения адреса. Также, ip address может быть назначен статически. Для этого, нужно ввести команду, которая представлена ниже.

**set interface ethernet eth0 address ‘192.168.0.1/24’**

**2.2 Настройка ssh на VyOS**

Для настройки ssh на VyOS сначала требуется активировать порт, который будет отвечать за работу ssh. Делается это с помощью команды, представленной ниже.

**set service ssh port <number\_of\_port>**	

Настройка адреса, на который будут приниматься ssh-соединение производится с помощью команды, которая представлена ниже.

**set service ssh listen-address <address>**

где **<address> -** ip-адрес, который мы берём с локального интерфейса

Настройка шифрования, которое разрешено пользователю при работе по ssh производится с помощью команды, представленной ниже.

**set service ssh cipher <cipher>**

Где **<cipher> -** вариант шифрования из возможных: 3des-cbc, aes128-cbc, aes192-cbc, aes256-cbc, aes128-ctr, aes192-ctr, aes256-ctr, arcfour128, arcfour256, arcfour, blowfish-cbc, cast128-cbc.

Если нужно отключить парольную аутентификацию и использовать только аутентификацию по открытым SSH-ключам, то необходимо ввести команду, представленную ниже.

**set service ssh disable-password-authentification**

Для отключения верификации хоста через DNS-lookup, которое может увеличить скорость подключения к хосту и уменьшить время задержки, требуется ввести команду, указанную ниже.

**set service ssh disable-host-validation**

Для указания отличного от стандартного MAC-алгоритма, который используется только во второй версии протокола ssh, требуется ввести команду, указанную ниже. Несколько видов протоколов, указанный одновременно, тоже будут поддерживаться.

**set service ssh macs <mac>**

Список доступных mac-алгоритмов указан далее: hmac-md5, hmac-md5-96, hmac-ripemd160, hmac-sha1, hmac-sha1-96, hmac-sha2-256, hmac-sha2-512, umac-64@openssh.com, umac-128@openssh.com, hmac-md5-etm@openssh.com, hmac-md5-96-etm@openssh.com, hmac-ripemd160-etm@openssh.com, hmac-sha1-etm@openssh.com, hmac-sha1-96-etm@openssh.com, hmac-sha2-256-etm@openssh.com, hmac-sha2-512-etm@openssh.com, umac-64-etm@openssh.com, umac-128-etm@openssh.com

Для добавления ACL на пользаков и группы пользователей требуется ввести команду, указанную ниже. Директивы будут обрабатываться в следующем порядке: deny-users, allow-users, deny-group, allow-group.

**set service ssh access-control <allow | deny> <group | user> <name>**

Настройка timeout на автооключение пользователя из ssh-сессии производится с помощью установки интервала в секундах на это отключение. Команда для этой настройки приведена ниже.

**set service ssh client-keepalive-interval <interval-in-second>**

Для указания другого (отличного от дефолтного) алгоритма обмена ключами требуется использовать команду с параметрами, представленную ниже.

**set sevice ssh key-exchange <kex>**

**Доступные алгоритмы:** diffie-hellman-group1-sha1, diffie-hellman-group14-sha1, diffie-hellman-group14-sha256, diffie-hellman-group16-sha512, diffie-hellman-group18-sha512, diffie-hellman-group-exchange-sha1, diffie-hellman-group-exchange-sha256, ecdh-sha2-nistp256, ecdh-sha2-nistp384, ecdh-sha2-nistp521, curve25519-sha256 and curve25519-sha256@libssh.org

Настройка уровня логирования **ssh** производится с помощью команды, указанной ниже.

**set service ssh loglevel <quiet | fatal | error | info | verbose>**

Установка другого имено для инстанса VRF производится с помощью команды, указанной ниже.

**set service ssh vrf <name>**

Теперь переходим к операциям с ssh, осуществляемым из режима операций, который обозначается значком $, указанным в приглашении.

**restart ssh -** перезапуск ssh

**generate ssh server-key -** перегенерация публичного/приватного ключей для ssh, используемого для защищенного подключения.

**generate ssh client-key /path/to/private\_key -** перегенерация известного публичного/првиатного ключей, используемых для подключения другими сервисами. После создания два новых файла, а именно .ssh/config/auth/id\_rsa\_rpki и .ssh/config/auth/id\_rsa\_rpki.pub

**generate public-key-command name <username> path <location> -** Генерирует командsы режима конфигурации для добавления публичного ключа, используемого для режима аутентификации по ключу. <location> указывает на локальный путь или URL к удалённому файлу.

Поддерживаемые протоколы: FTP, HTTP, HTTPS, SCP/SFTP, TFTP.

**2.3 Быстрый старт в DHCP/DNS на VyOS (более подробно см. ниже)**

Следующие настройка позволяют конфигурировать DHCP и DNS-сервисы в ващей внутренней/локальной сети, где VyOS будет выступать как маршрут по-умолчанию и DNS-сервер.

Допустим, маршрут по-умолчанию и перенаправляющий dns-сервер будет иметь адрес 192.168.0.1/24.

Диапазон адресов 192.168.0.2/24 - 192.168.0.8/24 будет зарезервирован для статического назначения.

DHCP-клиенты будут получать адреса из диапазона 192.168.0.9 - 192.168.0.254 и будет иметь доменное имя *internal-network.*

Аренда DHCP-адресов будет длиться 1 день (86400 секунд).

VyOS будет выполнять роль полноценного перенаправляющего DNS-сервера, заменяя необходимость использовать сервисы Google, Cloudflare, или каких-либо другим пебличных DNS-серверов (что является достаточным для приватности).

Только хосты из вашей внутренней/локальной сети могут использовать перенаправляющий DNS-сервер.

Пример конфигурации:

**set service dhcp-server shared-network-name LAN subnet 192.168.0.0/24 default-router ‘192.168.0.1’ -** для подсети с названием LAN и подсетью 192.168.0.0/24 задаём маршрут по-умолчанию 192.168.0.1

**set service dhcp-server shared-network-name LAN subnet 192.168.0.0/24 name-server ‘192.168.0.1’ -** dns-сервер для данной подсети будет доступен по адресу 192.168.0.1

**set service dhcp-server shared-network-name LAN subnet 192.168.0.0/24 domain-name ‘vyos.net’ -** доменное имя в данной подсети - vyos.net

**set service dhcp-server shared-network-name LAN subnet 192.168.0.0/24 lease ‘86400’ -** стандартное время аренды адреса - 86400 секунд

**set service dhcp-server shared-network-name LAN subnet 192.168.0.0/24 range 0 start ‘192.168.0.9’ -** диапазон, из которого будут выдаваться адреса, начинается с 192.168.0.9

**set service dhcp-server shared-network-name LAN subnet 192.168.0.0/24 range 0 stop ‘192.168.0.254’ -** заканчивается на 192.168.0.254

**set service dns forwarding cache-size ‘0’ -** размер кэша не ограничен

**set service dns forwarding listen-address ‘192.168.0.1’ -** адрес, на который принимаются подключения

**set service dns forwarding allow-from ‘192.168.0.0/24’ -** подсеть, с которой будут приниматься и обрабатываться запросы

**2.4 NAT (Network Address Translation (более подробно см. ниже))** 

С помощью следующих настроек можно сконфигурировать правила SNAT для всего внутренней/LAN(локальной) сети, разрешить хостам устанавливать связь во внешней/WAN(глобальной) сети через подмену IP (masquerade).

Пример конфигурации:

**set nat source rule 100 outbound-interface ‘eth0’ -** для правила под номером 100 добавляем выходной интерфейс eth0

**set nat source rule 100 source address ‘192.168.0.0/24’ -** для правила под номером 100 добавляем подсеть, с которой будем фильтровать трафик



**set nat source rule 100 translation address masquerade -** для правила 100 добавляем подмену IP (masquerade)

**2.5 Firewall**

Добавляет набор политик firewall для всего исходяшего/WAN трафика.

Следующая конфигурация создает хороший firewall который блокирует весь трафик, который не был отправлен из внутренней/LAN сети.

**set firewall name OUTSIDE-IN default-action ‘drop’**

**set firewall  name OUTSIDE-IN rule 10 action ‘accept’**

**set firewall  name OUTSIDE-IN rule 10 state established ‘enable’**

**set firewall  name OUTSIDE-IN rule 10 state related ‘enable’**

**set firewall  name OUTSIDE-LOCAL default-action ‘drop’**

**set firewall  name OUTSIDE-LOCAL rule 10 action ‘accept’**

**set firewall  name OUTSIDE-LOCAL rule 10 state established ‘enable’**

**set firewall  name OUTSIDE-LOCAL rule 10 state related ‘enable’**

**set firewall  name OUTSIDE-LOCAL rule 20 action ‘accept’**

**set firewall  name OUTSIDE-LOCAL rule 20 icmp type-name ‘echo-request’**

**set firewall  name OUTSIDE-LOCAL rule 20 protocol ‘icmp’**

**set firewall  name OUTSIDE-LOCAL rule 20 state new ‘enable’**

Если вы хотите разрешить SSH доступ на вашем firewall с внешнего/WAN интерфейса, вы должны создать дополнительное правило, что разрешит определённый трафик, т.е. - ssh.

Следующие правила разрешают ssh-трафик и ограничивают количество подключений до 4 запросов в минуту. Это блокирует попытки подбора пароля (brute-force атака).

**set firewall name OUTSIDE-LOCAL rule 30 action ‘drop’**

**set firewall name OUTSIDE-LOCAL rule 30 destination port ‘22’**

**set firewall name OUTSIDE-LOCAL rule 30 protocol ‘tcp’**

**set firewall name OUTSIDE-LOCAL rule 30 recent count ‘4’**

**set firewall name OUTSIDE-LOCAL rule 30 recent time ‘minute’**

**set firewall name OUTSIDE-LOCAL rule 30 state new ‘enable’**

**set firewall name OUTSIDE-LOCAL rule 31 action ‘accept’**

**set firewall name OUTSIDE-LOCAL rule 31 destination port ‘22’**

**set firewall name OUTSIDE-LOCAL rule 31 protocol ‘tcp’**

**set firewall name OUTSIDE-LOCAL rule 31 state new ‘enable’**

Чтобы применить политики firewall, требуется ввести следующие команды:

**set interface ethernet eth0 firewall in name ‘OUTSIDE-IN’**

**set interface ethernet eth0 firewall local name ‘OUTSIDE-LOCAL’**

Применение изменений, сохранение конфигурации и выход из режима конфигурации:

**commit**

**save**

**exit**

**2.6 Усиление безопасности (Hardening)**

Особенно если вы разрешили удалённый доступ по SSH через внешний/WAN интерфейс, необходимо выполнить еще несколько дополнительных шагов.

Заменить стандартного системного пользователя *vyos:*

**set system login user myvyosuser authentification plaintext-password mysecurepassword**

Настройка аутентификации по ключам:

**set system login user myvyosuser authentification public-keys myusername@mydesktop type ssh-rsa**

**set system login user myvyosuser authentification public-keys myusername@mydesktop key contents\_of\_rsa.pub**

И наконец, попробуйте установить ssh-соединение в VyOS в качестве нового пользователя. После получения подтверждения что ваш пользователь может получать доступ на ваш роутер без пароля, удалите пользователя vyos и после этого отключите парольный вход по ssh.

**delete sytem login user vyos**

**set service ssh disable-password-authentification**

Как было сказано выше, подтвердите свои изменения, сохраните конфигурацию и выйдите из режима конфигурации:

**commit**

**save**

**exit**



**VyOS. DHCP**

**IPv4 сервер.**

Сетевая топология объявляется с помощью shared-network-name, и с помощью объявления подсети. Служба DHCP может обслуживать сразу несколько shared networks, при этом, каждая сеть может иметь одну или более подсетей. Каждая подсеть может быть привязана к интерфейсу. Диапазон может быть объявлен внутри подсети, как пул динамических адресов. Так же, можно объявить несколько диапазонов, которые будут содержать ... Статические сопоставления могут быть установлены для назначения статических адресов клиентам по их MAC-адресу.

**Конфигурация.**

**set service dhcp-server hostfile-update**

Создает DNS-записи для каждого аредованного клиентом адреса путём добавления клиента в /etc/hosts файл. Запись будет иметь формат: <shared-network-name>\_<hostname>.<domain-name>

**set service dhcp-server hpst-decl-name**

Будет удалять <shared-network-name>\_ для DNS-записей клиентов, используя только объявление имени и домена: <hostname>.<domain-name>

**set service dhcp-server shared-network-name <name> domain-name <domain-name>**

Параменто **domain-name** может содержать доменной имя, которое будет применяться к хостанеймам клиентов для формирования FQDN(полного доменного имени).

Этот параметр конфигурации для всего определения shared-network. Все подсети будут содержать этот параметр если он не будет задан локально в каждой подсети.

**set service dhcp-server shared-network-name <name> domain-search <domain-name>**

Параметр **<domain-name>** должен содержать доменное имя когда выполняется DNS-запрос где доменное имя не указано. Эта опция может быть объявлена несколько раз, если вам нужно использовать несколько доменов поиска.

Этот параметр конфигурации для всего определения shared-network. Все подсети будут содержать этот параметр если он не будет задан локально в каждой подсети.


**set service dhcp-server shared-network-name <name> name-server <address>**

Передаёт клиентам информацию о том, что DNS-сервер может быть доступен по адресу **<address>.**

Этот параметр конфигурации для всего определения shared-network. Все подсети будут содержать этот параметр если он не будет задан локально в каждой подсети.

Несколько DNS-серверов так же могут быть объявлены.

**set service dhcp-server shared-network-name <name> ping-check**

До того, как DHCP-сервер выдаст IP-адрес клиенту, первоначально, он отправит ICMP Echo-запрос (иначе говоря - пинг) на назначаемый адрес. Ожидание длится около секунды, и если нет Echo-ответа, то адрес будет назначен.

Если ответ  не был получен, то аренда будет прекращена, и сервер не ответит клиенту. Аренда будет прекращена на минимальное количество времени (параметр **abadoned-lease-time**), которые по-умолчанию равно 24 часам.

Если нет свободных адресов, но есть приостановленные аренды, то DHCP-сервер будет пробовать перезапросить приостановленные для аренды IP-адреса, не обращая внимание на параметр **abadoned-lease-time.**

**Индивидуальные подсети клиентов.**

**set service dhcp-server shared-network-name <name> authoritative**

Это говорит о том, что данное устройство будет единственным DHCP-сервером в сети. Если другое устройство будет пытаться предлагать аренду DHCP, это устройство будет отправлять ‘DHCPNAK’ на все устройства, которые будут пытаться запросить IP-адрес, который не действителен для этой сети.

**set service dhcp-server shared-network-name <name> subnet <subnet> default-router <address>**

Параметр конфигурации <subnet>, который сообщается при ответе, говорит клиенту, что его маршрут по-умолчанию должен содержаться в параметре <address>.

**set service dhcp-server shared-network-name <name> subnet <subnet> name-server <address>**

Этот параметр в подсети, который сообщается при ответе, говорит клиенту, что DNS-сервер располагается по адресу <address>.
