# Лабораторная работа 1а

Написана на Python. 

## Запуск
Для запуска сервера в командную строку необходимо ввести `python server.py`.

Для запуска клиента в командную строку необходимо ввести `python client.py`.

## Описание команд 
Сервер поддерживает две команды:
1) `!send {filename}` для отправки файла
2) `!exit` для выхода из чата

## Описание протокола
Протокол подразумевает последовательность действий:

1) **Установка соединения.** Может быть только первым, в остальных ситуациях не является корректным.
2) **Передача сообщений.** Выполняется в любой момент между установкой соединения и его завершением.
3) **Завершение соединения.** Может быть выполнено только после установки соединения.

## Формат пакета

**От клиента:**
+ без файла:

message_time | message
--- | --- |

+ с файлом:

message_time | message
--- | --- |

file_sha | file_data
--- | --- |

**От сервера:**
+ без файла:

message_time | [{nickname}]: message |
--- | --- |  

+ с файлом:

message_time | message
--- | --- |

file_sha | file_data
--- | --- |

message_time | {nickname} send file {filename}
--- | --- |

Время, передаваемое в сообщении должно быть в формате: `%Y-%m-%d %H:%M:%S.%f, где Y - год, m - месяц, d - день, H - часы, M - минуты, S - секунды, f - миллисекунды`.

При передаче сообщений в качестве разделителя используется символ `|`. Однако это не мешает использовать данный символ в сообщении, так как деление частей производится по первому символу.

## Подключение к серверу
После подключение клиента к серверу ему будет предложено выбрать никнейм следующим сообщением:
`{time}|[SERVER]: What's your nickname?`

Если уже существует пользователь с таким никнеймом, то клиенту будет предложено выбрать новый никнейм:
`{time}|[SERVER]: Nickname is busy. Choose another one.`

Такая процедура будет продолжаться до тех пор, пока никнейм пользователя не станет уникальным.

После этого пользователь может писать соообщения и отправлять файлы.

При подключении клиента к серверу всем участникам будет отправленно сообщение:
`{time}|[SERVER]: {nickname} joined`

При выходе клиента из чата всем участникам будет отправленно сообщение: 
`{time}|[SERVER]: {nickname} left`

## Отключение от сервера

Для отключение от сервера пользователю просто надо написать `!exit` в чат. 

После этого все участники чата получат сообщение:
`{time}|[SERVER]: {nickname} left`
