# Kafka Lab — Репетиторская платформа

Лабораторная работа по потоковой обработке данных в реальном времени с использованием Apache Kafka.

## Описание

Реализован полный цикл работы с сообщениями в Kafka на примере репетиторской платформы. Producer генерирует события о занятиях (репетитор, ученик, предмет, цена, рейтинг) и отправляет их в топик. Consumer читает сообщения из топика, валидирует их и выводит результат в консоль.

Около 30% сообщений намеренно генерируются с невалидными данными (отрицательная цена или рейтинг выше 5.0) для демонстрации работы валидации.

## Стек технологий

- **Apache Kafka 4.2.0** (KRaft mode, без ZooKeeper) — брокер сообщений
- **Python 3** — язык разработки скриптов
- **kafka-python** — клиентская библиотека для работы с Kafka из Python

## Структура проекта

```
kafka-lab/
├── generator.py      # Генерация случайных сообщений (вне Producer — принцип SOLID)
├── producer.py       # Отправка сообщений в Kafka-топик
├── consumer.py       # Чтение и валидация сообщений из Kafka-топика
└── requirements.txt  # Зависимости Python
```

## Формат сообщения

```json
{
  "tutor": "Ivan",
  "student": "Olga",
  "subject": "Math",
  "price": 35,
  "rating": 4.2
}
```

## Правила валидации (Consumer)

| Поле | Условие невалидности |
|------|----------------------|
| `price` | `<= 0` |
| `rating` | `> 5.0` |

## Запуск

### Требования
- Java 17+
- Python 3.8+
- Apache Kafka 4.2.0

### 1. Установить зависимости Python

```bash
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Инициализировать Kafka (только один раз)

```cmd
cd C:\path\to\kafka_2.13-4.2.0

bin\windows\kafka-storage.bat random-uuid
bin\windows\kafka-storage.bat format --standalone -t <UUID> -c config\server.properties
```

### 3. Запустить Kafka (окно 1)

```cmd
bin\windows\kafka-server-start.bat config\server.properties
```

### 4. Запустить Consumer (окно 2)

```cmd
venv\Scripts\activate
python consumer.py
```

### 5. Запустить Producer (окно 3)

```cmd
venv\Scripts\activate
python producer.py
```

## Пример вывода

**Producer:**
```
SEND: {"tutor": "Anna", "student": "Petr", "subject": "Physics", "price": 42, "rating": 4.7}
SEND: {"tutor": "Ivan", "student": "Maria", "subject": "Math", "price": -500, "rating": 6.3}
```

**Consumer:**
```
RECEIVED: {"tutor": "Anna", "student": "Petr", "subject": "Physics", "price": 42, "rating": 4.7}
NOT VALID: {"tutor": "Ivan", "student": "Maria", "subject": "Math", "price": -500, "rating": 6.3}
```

### 6. Результат работы
![Screenshoot](/Screenshot (7).png)

