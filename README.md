# OldNmap 0.3
Это официальная документация OldTeam по `nmap`, в этой документации мы делаем упор именно на `NETSTALKING`,
но и даже так документация будет очень большая, и информативная. Тут будет даже в целом про сканирование портов.
Зачем это надо? Что мы получаем? Как это работает? Тут мы постараемся ответить на все вопросы. Такая короткая
первая страница.. У меня никогда не получалось лить воду).

# Порты
Как вы знаете весь интернет построен на отправке и принятие пакетов, для отправки которых,
используеться один из протоколов транспортировки, среди них `TCP`,`UDP`,и другие.
Они отправляют пакет на указанный `IP адрес`. Но тут встаёт вопрос, а что делать с ним дальше,
кому он принадлежит, может `Телеграму`, может `FTP` серверу. Именно в этот момент в дело вступает порт.

Можно привести метафору, если `IP` адрес выступает как адрес до дома, то порт выступает в качестве
номера квартиры. Порт определяет, какому приложению, нужен отправленный пакет.

Как вы понимаете его главная фишка, и есть его главная ошибка. Благодаря тому что у приложений или протоколов,
уже чётко определены свои порты. К примеру:

```
у HTTP это порт  80
у HTTPS это порт 443
```

Мы можем узнать что скрываеться за этим хостом.  
Для начала стоит понять, что данные могут передаваться только через **ОТКРЫЙ** порт.
Если он закрыт, значит хост не имеет никакого отношения к приложению которое использует этот закрытый порт.

Например у `IP адреса`: `1.2.3.4`  
Открыт порт `80`, но закрыт порт `20`.

Порт `80` это порт протокола `HTTP` именно за счёт этого протокола,
вы можете открыть сайты. Ну если более точно принимать сайты.

Порт `20` это порт протокола `FTP` именно за счёт этого протокола,
вы можете обмениваться файлами в сети. Когда вы что-то скачиваете, скорее всего,
это что-то скачиваеться  с `FTP` сервера.

Итак, первый порт открыт, воторой закрыт.
Как мы помним данные могу передаваться только по открытому порту.
Поэтому мы можем сделать вывод, посмотрев на статус этих портов.

Для чего чаще всего используеться `HTTP`? Для сайтов! Собственно говоря, мы можем сказать что за этим `IP адресом`, скрываеться именно сайт. 

**PS:** Порт 20 мы даже не берём во внимание он закрыт.

# Основные порты
Всего существует `65535` портов, начиная с `0`, тоесть (`0-65535`).  
Тут приведён лишь очень маленький список, если вам нужны все порты то вам [сюда](https://ru.wikipedia.org/wiki/%D0%A1%D0%BF%D0%B8%D1%81%D0%BE%D0%BA_%D0%BF%D0%BE%D1%80%D1%82%D0%BE%D0%B2_TCP_%D0%B8_UDP).

``` 
HTTP/tcp,udp             80,8080,8081
SMTP/tcp,udp             25,465,587
RVI/tcp                  37777
FTP/tcp                  20,21 
HIKVISION/tcp,udp        8000
HTTPS/tcp,udp            443 
SSH/tcp,udp              22
DNS/tcp,udp              53
```

Что можно сказать? Порт для нетсталкеров, служит в качестве сортировки, и обозначения того что скрываеться за `IP адресом`.
За ним может быть всё что угодно, `камера`, `сайт`, `FTP`. Но зная какие порты у него открыты мы можем сказать
что это такое.

Также например зная порты можем к ним подключиться, и даже попытаться что-то отправить. Ну вообще эта тема уже `блэкхэтов`.

# Что такое nmap
Прежде всего **nmap** это сканер портов, в который засунули всё что можно.  
- Например он может сканировать порты методами: `TCP`,`UDP`,`SYN`,`FIN`,`XMAS`,`NULL`.
- Может определить версию софта на порту, например: `nginx 1.24.0`.
- И ещё очень очень много всего, [а точнее](https://nmap.org/book/man-briefoptions.html).

Очень много функциональный инструмент, который стал уже классическим.

# Установка
### Debian 11
```
sudo apt install nmap
```
### Arch linux
```
sudo pacman -S nmap
```

### Windows
Скачать `nmap` на виндус можно тут - [download](https://nmap.org/download.html)

Для виндуса, всё не так однозначно. Тут у консольного `nmap`-(а) есть графическая оболочка
`zenmap`. После того как вы выбрали, нужен ли вам интерфейс.  

---

### Компиляция
Если вы рисковый чувак то можете его скомпилировать вручную.  
Для этого выполните эти команды:

```
git clone https://github.com/nmap/nmap
cd nmap
./configure
make
make install
```
## Вывод nmap
Вначале просто показываеться IP адреса и DNS сканируемого сервера.
```
Запуск: 
Starting Nmap 7.80 ( https://nmap.org ) at 2023-04-20 14:31 MSK
```
```
Сообщение о отправки запроса на указанный хост: 
Nmap scan report for google.com (64.233.162.101)
```

```
Состояние хоста, и время ответа: 
Host is up (0.019s latency).
```

```
Другие айпи этого сервер, которые не были просканированы: 
Other addresses for google.com (not scanned):64.233.162.100 64.233.162.139 64.233.162.138
64.233.162.102 64.233.162.113 2a00:1450:4010:c08::8b
2a00:1450:4010:c08::64 2a00:1450:4010:c08::8a 2a00:1450:4010:c08::71
```

```
Днс записи: 
rDNS record for 64.233.162.101: li-in-f101.1e100.net
```

```
Порты которые он тоже просканировал но не вывел на экран: 
Not shown: 998 filtered ports
```

а уже внизу идут строчки по типу:

```
PORT      STATE     SERVICE
21/tcp    closed    ftp
22/tcp    filtered  ssh
80/tcp    open      http
```

В начале идёт порт, и через `/`, его протокол.  
Вторым значение идёт состояние порта, их бывает несколько:  

```
(open)              открыт 
(closed)            закрыт 

(filtered)          фильтруется 
(unfiltered)        не фильтруется
(open|filtered)     открыт|фильтруется 
(closed|filtered)   закрыт|фильтруется 
```
Все `filtered` означают что nmap не смог определить состояние порта.
Он может быть как открыт, так и закрыт.

Третим идёт сервис, тоесть протокол или приложение этого порта.
```
ftp
ssh
http
```

## Словарик
```
IPaddress    это IP адрес
IPrange      это IP диапазон
DNS          это Домен
CIDR         это Подсеть
```

## Как просканировать:
#### DNS
``` bash
nmap google.com
```
#### CIDR
``` bash
nmap 62.131.130.0/24
```
#### IPaddress
``` bash
nmap 127.0.0.1
```
#### IPrange
``` bash
nmap 62.131.130.0-255
```

![alt text](https://i.imgur.com/VZybtDk.png)

**PS:** Ссылки, (URL), указывать нельзя!  
**PS:** Потому что URL изначально имеет протокол (http://, https://).  
**PS:** А это и есть порты 80 и 443.  

### Как просканировать цели из файла?
Для того что-бы просканировать все цели из файла, нужно использовать аргумент `-iL`.  
А потом указать путь к файлу с целями.  

```
nmap -iL ФАЙЛ
```

В файле, каждая новая цель должна быть на новой строчке.  
Пример файла:  

```
google.com
yandex.ru
youtube.com
github.com
```

**PS:** Формат файла не важен, но, я использую txt.

## Методы сканирования
В nmap как мы уже говорили выше, есть очень много методов для сканирования портов.
Но чем они отличаються, и какой же из них самый лучший.

**PS:** Некоторые методы работают только есть вы запускаете nmap с правами администратора! (sudo)

- `SYN` `-sS` - также известное как полуоткрытое сканирование, является самым популярным методом сканирования портов в `Nmap`. Этот метод использует `TCP` пакеты типа `SYN`. Nmap отправляет пакеты `SYN` на каждый порт, который нужно проверить. Если порт открыт, то удаленный хост отправляет ответное сообщение `SYN-ACK`. В этом случае, Nmap знает, что порт открыт и записывает его в список открытых портов. Если порт закрыт, то удаленный хост отправляет сообщение `RST`, а если порт фильтруется (например, брандмауэр блокирует пакеты `SYN`), то удаленный хост не отправляет никакого ответа. `SYN` сканирование очень эффективно и быстро определяет открытые порты.

- `TCP` `-sT` - это сканирование использует пакеты `TCP` с установленным флагом `ACK` для определения открытых портов. Как и в методе `SYN` сканирования, Nmap отправляет пакеты на каждый порт, который нужно проверить. Если порт открыт, то удаленный хост отправляет ответное сообщение `RST-ACK`. Если порт закрыт, то удаленный хост отправляет только сообщение `RST`. Если порт фильтруется, то удаленный хост не отправляет никакого ответа. `TCP` сканирование может быть полезным при обнаружении хостов, на которых нельзя использовать полуоткрытое сканирование.

- `UDP` `-sU` - это сканирование отправляет пакеты `UDP` на каждый порт, который нужно проверить. Если порт открыт, то удаленный хост не отправляет ответное сообщение. В этом случае, Nmap считает, что порт открыт. Однако, многие хосты настроены таким образом, что не отправляют ответы на закрытые порты, поэтому этот метод может быть медленным и не очень точным. Если порт закрыт или фильтруется, то удаленный хост отправляет `ICMP` сообщение типа `Port Unreachable`. `UDP` сканирование может быть полезным при обнаружении служб, которые используют `UDP` вместо `TCP`. Ведь порты могут быть не только `TCP`.

- `FIN` `-sF` - это сканирование отправляет пакеты `TCP` с установленным флагом `FIN` на каждый порт, который нужно проверить. Если порт открыт, то удаленный хост не отправляет ответное сообщение. Если порт закрыт, то удаленный хост отправляет сообщение `RST`. Если порт фильтруется, то удаленный хост отправляет `ICMP` сообщение типа `Destination Unreachable`. `FIN` сканирование может быть полезным при обнаружении служб, которые реагируют на такие пакеты, например, некоторые `Windows` системы.

- `NULL` `-sN` - это сканирование отправляет пакеты `TCP` без установленных флагов на каждый порт, который нужно проверить. Если порт открыт, то удаленный хост не отправляет ответное сообщение. Если порт закрыт, то удаленный хост отправляет сообщение `RST`. Если порт фильтруется, то удаленный хост отправляет `ICMP` сообщение типа `Destination Unreachable`. `NULL` сканирование может быть полезным при обнаружении служб, которые реагируют на такие пакеты, например, некоторые `UNIX` системы.

- `XMAS` `-sX` - это сканирование отправляет пакеты `TCP` с установленными флагами `FIN`, `PSH` и `URG` на каждый порт, который нужно проверить. Если порт открыт, то удаленный хост не отправляет ответное сообщение. Если порт закрыт, то удаленный хост отправляет сообщение `RST`. Если порт фильтруется, то удаленный хост отправляет `ICMP` сообщение типа `Destination Unreachable`. `XMAS` сканирование может быть полезным при обнаружении служб, которые реагируют на такие пакеты, например, некоторые `Linux` системы.

**PS:** Какой лучший та? Я считаю `SYN`, но стоит понимать что всё зависит от ваших целей!

### Аннонимность
Многие методы сканирования портов, которые поддерживает `Nmap`, могут быть обнаружены системами защиты, такими как брандмауэры, `IPS/IDS` и другие системы мониторинга трафика. Некоторые методы могут быть более "анонимными", чем другие, и могут обходить определенные виды защиты, но никакой метод сканирования не может гарантировать полную анонимность.

Для примера, сканирование `SYN` является одним из наиболее анонимных методов сканирования портов. Поскольку `Nmap` не завершает установление соединения, удаленный хост не отправляет ответное сообщение, если порт открыт. Это может помочь избежать обнаружения, основанного на контроле соединений, таком как `IDS`. Однако, если на удаленном хосте работает система мониторинга трафика, то она может обнаружить сканирование на основе количества `SYN` пакетов, направленных на различные порты.

Другие методы сканирования, такие как `FIN`, `XMAS` и `NULL` сканирования, также могут быть более анонимными, чем `SYN` сканирование, так как они не отправляют пакеты `SYN`, которые могут быть обнаружены системами защиты. Однако, эти методы сканирования также могут быть обнаружены на некоторых сетях. Тут всё зависит от хоста который вы сканируете.

Методы сканирования портов в `TCP` и `UDP` не отличаются существенно по своей анонимности,у обоих плохая. Как правило, методы сканирования портов в `TCP` и `UDP` могут быть обнаружены системами защиты на основе анализа трафика.

В случае `UDP` сканирования, его аннонимность можно увеличить использыванием опции "зеркального сканирования" `--scanflags IPID`, при сканировании портов. Она основана на отправке специальных пакетов `UDP` на закрытые порты, которые могут вызвать ответное сообщение от удаленного хоста с определенным значением поля `IP ID`. Это может помочь обойти некоторые системы защиты, которые мониторят только трафик на открытых портах.

Наконец, для обеспечения максимальной анонимности при сканировании портов в `TCP` и `UDP` рекомендуется использовать методы сканирования, которые могут обойти конкретные типы систем защиты и производить сканирование внутри `VPN`, или аннонимной сети, например такой как `Tor`. Я например в случаях необходимости используя `proxy`, для этого в nmap можно использывать аргумент `--proxies` и указать прокси типа `SOCK4`,`SOCK5`,`HTTP`, в таком формате `протокол://айпи:порт`.

## Обход IDS/Брандмауэров
Если вы по жизни рисковый, то например при сканирование айпи диапазона пинтагона, вас может забанить `IDS` или `брандмауэр`. Мы об этом уже говорили немного выше. Но в `nmap`есть способы обойти их. 

- `-f` - этот аргумент, позволяет фрагментировать `IP` пакеты, он разбивает `TCP` заголовок, на части, и посылает их в разных пакетах, что-бы защита, не смогла понять что вы мутите. Но поаккуратнее с этой опцией).

```
sudo nmap -f 
```

- `--data-length` - этот аргумент, позволяет добавлять к пакетам произвольные данные. Рандомная информация, которая нужно просто что-бы увеличить размер пакета. Например размер `TCP` пакета в `nmap` состовляет `40` байтов, а `ICMP` пакеты, `28` байтов. Для дудоса маловато, но для обхода неплохо.

```
nmap --data-length 100
```

- `--ttl` - этот аргумент, позволяет установить `IPv4` поле в `time-to-live` в отправленных пакетах.

```
nmap --ttl 255
```

- `--badsum` - этот аргумент, позволяет посылась пакеты с  неправильными `TCP` и `UDP` контрольными суммами пакетов.

```
nmap --badsum
```



## Основные аргументы
- `--open` : показывать только открытые (или возможно открытые) порты.
```
nmap --open
```
![alt text](https://i.imgur.com/Io3yau0.png)

- `-sV` : включить определение версий софта на портах.
```
nmap -sV
```
![alt text](https://i.imgur.com/C0XbEMg.png)

- `-p` : позволяет указать порты.
```
// Сканирует только на порт 80
nmap -p 80
```
```
// Сканирует только на порты от 0 до 100
nmap -p 0-100
```
```
// Сканирует только на порты 80,21,22
nmap -p 80,21,22
```
![alt text](https://i.imgur.com/ogfCcoF.png)

- `-O` : включить определение ОС
```
nmap -O
```
![alt text](https://i.imgur.com/O9aS5rr.png)

## Скорость
Nmap не очень и быстрый, но его можно сильно ускорить этими аргументами:

- `-T` : установить количество потоков, от 0 до 5, чем больше тем быстрее.
```
nmap -T5
nmap -T4
// и так далее.
```

- `-n` : не производить разрешение DNS имен.
```
nmap -n
```

- `-Pn` : не использовать пинг сканирование, полностью пропускает этап обнаружения хостов.
```
nmap -Pn
```

- `-sS` : использывать SYN сканирование.
```
nmap -sS
```

- `--min-hostgroup` : позволяет установить размер групп для параллельного сканирования.
```
nmap --min-hostgroup 10
nmap --min-hostgroup 9
// и так далее.
```

- `--min-parallelism` : позволяет установить количество параллельных запросов.
```
nmap --min-parallelism 10
nmap --min-parallelism 9
// и так далее.
```

![alt text](https://i.imgur.com/vscbnSF.png)

Также, для увелечения скорости можно ограничить количество проверяемых портов. 
Это очень сильно повлияет на скорость выполнение, ведь стандартно он проверяет `1000` самых популярных портов. 
Давайте изменим это число на `50`.

Например поставить топ 50 самых популярных: `--top-ports 50`

```
// Пресет для максимальной скорости.
sudo nmap -T5 -n -sS --min-hostgroup 10 --min-parallelism 10 --top-ports 50
```

**PS:** Но помните что использывание `-Pn`, `-n` понижает точность сканирования.

## Сохранение результата в файл
Когда результатов очень много, то они могут не помещаться в терминал, поэтому их можно
сохранить в файл, используя следующие аргументы.

- `-oN` : сохранение в txt файл.
```
nmap -oN ИМЯ_ФАЙЛА
```

- `-oX` : сохранение в xml файл.
```
nmap -oN ИМЯ_ФАЙЛА
```
## NETSTALKING
Теперь поговорим про то как его эффективно использывать для нетсталкинга.

- `-iR` : генерация рандомных IP для сканирования.

```
nmap -iR КОЛИЧЕСТВО_IP
```

Очень интересная функция которая позволяет не перебирать в ручную айпи диапазоны.  
Я её ещё совмещаю с, сохранением в файл. Ну и  накидываю настроек для скорости.
И у меня выходит вот такая вот команда:

```
nmap -T5 -n -iR 1000000 --min-hostgroup 10 --min-parallelism 10 
--top-ports 200 -oN output.txt --script=http-title --open 
```

Можете использывать)).

## Скрипты
Ну не зря же они [движок](https://nmap.org/book/nse.html) для этого писали.  
Для использывания скриптов используется аргумент `--script=`. 
```
nmap --script=ИМЯ_СКРИПТА
```
Скриптов [очень много](https://nmap.org/nsedoc/scripts), среди них есть даже для тестирования уязвимостей сайта, и тд.  

## http-title 
Для нетсталкинга полезней всего будет скрипт `http-title` который показывает заголовок станицы.  

![alt text](https://i.imgur.com/a16IekA.png)

Благодаря заголовку, мы можем сразу понять, мы нашли интересное, или как обычно страницу роутера.
```
// Использывание скрипта http-title
nmap --script=http-title
```
