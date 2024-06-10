# Индивидуальное домашнее задание № 4
## __Безруков Давид Романович, БПИ224__
## Вариант № 27
### Задача о привлекательной студентке. У одной очень привлекательной студентки есть N поклонников. Традиционно в день св. Валентина очень привлекательная студентка проводит романтический вечер с одним из поклонников. Счастливый избранник заранее не известен. С утра очень привлекательная студентка получает N «валентинок» с различными вариантами романтического вечера. Выбрав наиболее заманчивое предложение, студентка извещает счастливчика о своем согласии, а остальных — об отказе.


### Задание выполнено на `7` баллов. 

### Работа на оценку [4-5]

Очаровательная студентка точно знает, сколько у неё есть поклонников, и с нетерпением ждёт валентинки от каждого из них. Когда все валентинки будут получены, она выберет *самую оригинальную* и уведомит автора о своём согласии на свидание. Остальным поклонникам она отправит сообщение с отказом.


#### Запуск программы
Для компиляции программы используйте Makefile:
```
make
```
Это создаст сервер и три клиента. Если нужно создать больше клиентов, можно сделать это командой:

```
gcc admirer.c common.h -o admirer
```
Сначала необходимо запустить сервер beauty, указав в аргументах порт и количество поклонников (клиентов). Если клиент будет запущен до сервера, он будет пытаться установить соединение в течение 10 секунд, после чего завершит работу.

Пример запуска:
```
./beauty 5000 3
./admirer 127.0.1.1 5000
```

#### Последовательность процессов в UDP-соединении
**Сторона сервера (Студентка):**
- Запуск сервера: Сервер запускается с указанием порта и количества поклонников (N). Создаётся сокет сервера, который будет слушать входящие соединения.
- Принятие соединений: Сервер ожидает входящие соединения от N поклонников. Для каждого поклонника создаётся отдельный сокет для связи.
- Получение валентинок: Сервер читает данные (валентинки), отправленные каждым поклонником через recv.
- Выбор лучшей валентинки: После получения всех валентинок, сервер выбирает самую впечатляющую.
- Отправка ответов: Сервер отправляет ответное сообщение автору лучшей валентинки, уведомляя о своём согласии. Остальным поклонникам отправляется сообщение с отказом.
- Завершение работы: Сервер закрывает соединения и завершает свою работу.


**Сторона клиента (Поклонник):**
- Запуск клиента: Клиент запускается с указанием IP-адреса и порта сервера.
- Подключение к серверу: Клиент пытается подключиться к сокету сервера. Если сервер не доступен, клиент будет повторять попытки в течение 10 секунд.  
- Отправка валентинки: После успешного подключения клиент отправляет свою валентинку серверу через send.
- Ожидание ответа: Клиент ожидает ответа от сервера, читая данные через recv.
- Принятие ответа: Клиент получает ответное сообщение: согласие или отказ.
- Завершение работы: Клиент закрывает соединение и завершает свою работу.

#### Результат работы прогграммы

```
$ ./admirer 127.0.0.1 8080
Usage: ./admirer <server ip> <port>

$ ./admirer 127.0.0.1 8080
Your pid: 2091
Can't connect to server. Retry: attempt 0 of 10
Can't connect to server. Retry: attempt 1 of 10
Connected to server.
Admirer is waiting for answer
Received YES
I'm very happy
```
```
$ ./admirer 127.0.0.1 8080
Your pid: 2092
Can't connect to server. Retry: attempt 0 of 10
Can't connect to server. Retry: attempt 1 of 10
Connected to server.
Admirer is waiting for answer
Received NO
There are more beauties around
```
```
$ ./be 8080 2
Usage: ./be <port> <num of admirers>

$ ./be 8080 2
Server IP: 0.0.0.0
Beauty received a message: "Unique valentine from 2091. Written with love after much thought." from admirer №1.
Beauty received a message: "Unique valentine from 2092. Written with love after much thought." from admirer №2.
Enter number of admirer you like: 1

<Beauty carefully considered everything and chose the number 1>


<Beauty carefully considered everything and chose the number 2151>
```
Server:
```
./be 2
Beauty received a message: Unique valentine from 2091. Written with love after much thought. from admirer №1
Beauty received a message: Unique valentine from 2092. Written with love after much thought. from admirer №2
Enter number of admirer you like: 1

<Beauty carefully considered everything and chose the number 1>
```
### Работа на оценку [6-7]

Привлекательная студентка точно знает, сколько у неё есть поклонников, и ждет валентинки от каждого из них. Когда все валентинки будут получены, она выберет самую впечатляющую и уведомит автора о своем согласии на свидание. Остальным поклонникам она отправит сообщение с отказом.

#### Запуск программы
Для компиляции программы используется `Makefile`:
```
make
```
Это создаёт сервер и три клиента. Если необходимо создать больше клиентов, это можно сделать следующей командой:
```
gcc admirer.c common.h -o admirer
```
Сначала нужно запустить сервер beauty, указав порт и количество поклонников (клиентов). Если клиент будет запущен раньше сервера, он будет пытаться установить соединение в течение 10 секунд, после чего завершит работу. То же самое касается и наблюдателя. Порядок запуска клиентов и наблюдателя не имеет значения, однако наблюдатель может быть только один.  
Пример запуска: 

```
./beauty 5000 3
./admirer 127.0.1.1 5000
./father 127.0.1.1 5000
```

#### Результат работы программы
```
$ ./be 8080 2
Server IP: 0.0.0.0
Beauty is ready to accept valentines
Received message from admirer №1.
Message: Unique valentine from 2091. Written with love after much thought.

Received message from admirer №2.
Message: Unique valentine from 2092. Written with love after much thought.

Beauty carefully considered everything and chose the number 1
Beauty sent YES to admirer №1
Beauty sent NO to admirer №2
```
```
$ ./admirer 127.0.0.1 8080
Your pid: 1099
Can't connect to server. Retry: attempt 0 of 10
Can't connect to server. Retry: attempt 1 of 10
Connected to server.
Admirer is waiting for answer
I'm very happy
```
```
$ ./admirer 127.0.0.1 8080
Your pid: 1093
Can't connect to server. Retry: attempt 0 of 10
Connected to server.
Admirer is waiting for answer
There are more beauties around
```