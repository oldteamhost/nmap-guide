# NmapGuide
Nmap для нетсталкеров, и не только.  
`
by oldteam 0.1
`
# Что такое nmap
Прежде всего **NMAP** это сканер портов, в который засунули всё что можно.  
- Например он может сканировать порты методами: `TCP`,`UDP`,`SYN`,`FIN`,`XMAS`,`NULL`.
- Может определить версию софта на порту, например: `nginx 1.24.0`.
- И ещё очень очень много всего, [а точнее](https://nmap.org/book/man-briefoptions.html).



Умеет тварь всё, но это ещё надо уметь использовать.  

# Зачем нам порты?
Зная какие порты открыты на сервере, мы можем сказать что на нём находиться.

**Например:** если у сервера открыт порт `8000` то мы можем сказать что это `IP камера` - `HIKVISION`.  
**Например:** если у сервера открыт порт `80` - `http` то мы можем сказать что у него есть сайт. 

**PS:** Как вы понимате это правило работает только если порт открыт.  
**PS:** Полный список портов можете найти [тут](https://ru.wikipedia.org/wiki/%D0%A1%D0%BF%D0%B8%D1%81%D0%BE%D0%BA_%D0%BF%D0%BE%D1%80%D1%82%D0%BE%D0%B2_TCP_%D0%B8_UDP).

# Сканирования цели
Сканирования dns - **google.com**:
``` bash
nmap google.com
```
Сканирования IP - **127.0.0.1**:
``` bash
nmap 127.0.0.1
```
Сканирования IP диапазона - **62.131.130.0-62.131.130.255**:
``` bash
nmap 62.131.130.0-255
```
Сканирование подсети - **62.131.130.0/24**:
``` bash
nmap 62.131.130.0/24
```
![alt text](https://i.imgur.com/VZybtDk.png)

**PS:** Ссылки указывать нельзя!  
**PS:** Потому что URL изначально имеет протокол (http://, https://).  
**PS:** А это и есть порты 80 и 443.  

# Полезные аргументы
- `--open` : показывать только открытые порты.
```
nmap --open
```
![alt text](https://i.imgur.com/Io3yau0.png)
- `-sV` : включить определение систем.
```
nmap -sV
```
![alt text](https://i.imgur.com/C0XbEMg.png)
- `-p` : Позволяет указать порты.
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
# Скорость
Nmap не такой уж и быстрый, но его можно сильно ускорить этими аргументами.  

- `-T` : установить количество потоков, от 0 до 5, чем больше тем быстрее.
```
nmap -T5
nmap -T4
nmap -T3
и тд.
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
nmap --min-hostgroup 8
и тд.
```

- `--min-parallelism` : позволяет установить количество параллельных запросов.
```
nmap --min-parallelism 10
nmap --min-parallelism 9
nmap --min-parallelism 8
и тд.
```
![alt text](https://i.imgur.com/wpsohy2.png)
**PS:** Но помните что использывание `-Pn`, `-n` понижает точность сканирования.

# Скрипты
Ну не зря же они [движок](https://nmap.org/book/nse.html) для этого писали.  
Для использывания скриптов используется аргумент `--script=`.
```
nmap --script=ИМЯ_СКРИПТА
```

Скриптов [очень много](https://nmap.org/nsedoc/scripts), среди них есть даже для тестирования уязвимостей сайта, и тд.  
Но для нетсталкинга полезней всего будет скрипт `http-title` который показывает заголовок станицы.  

![alt text](https://i.imgur.com/a16IekA.png)

Благодаря заголовку, мы можем сразу понять, мы нашли интересное, или как обычно страницу роутера.
```
// Использывание скрипта http-title
nmap --script=http-title
```
