# Lab 04 NoSQL

## Оглавление
1. [Описание](#описание)
2. [Стек технологий](#стек-технологий)
3. [Функциональные возможности](#функциональные-возможности)
4. [Структура данных](#структура-данных)
5. [Запросы и аргументы командной строки](#запросы-и-аргументы-командной-строки)
6. [Установка и запуск](#установка-и-запуск)
7. [Настройка логирования](#настройка-логирования)
8. [Полезные ссылки](#полезные-ссылки)
9. [Maintainers](#maintainers)

## Описание

Приложение анализирует данные пользователей социальной сети ВКонтакте (VK), включая их подписчиков, подписки, группы и общие связи. Используя VK API, программа собирает информацию вглубь до двух уровней (исходный пользователь, его подписчики и подписчики его подписчиков), и сохраняет её в базе данных Neo4j для дальнейшего анализа. 

Программа устойчива к ошибкам и логирует каждый шаг выполнения, что упрощает диагностику и отслеживание работы.

## Стек технологий
- Python
- VK API
- Neo4j
- Логирование (logging)

## Функциональные возможности
- Получение информации о пользователе VK, его подписчиках, подписках и группах на 2 уровня вглубь:
  - Исходный пользователь → его подписчики и подписки → их подписчики и подписки.
- Сохранение данных о пользователях, группах и их связях в базе данных Neo4j.
- Устойчивость к ошибкам: приложение обрабатывает исключения, такие как превышение лимитов API или отсутствие данных.
- Запросы на выборку данных с помощью аргументов командной строки:
  - Всего пользователей.
  - Всего групп.
  - Топ 5 пользователей по количеству подписчиков.
  - Топ 5 популярных групп.
  - Топ 3 пар пользователей, которые имеют больше всего подписок на одни и те же группы или страницы.
- Поддержка параметра для включения/отключения парсинга VK (по умолчанию включен).

## Структура данных
### Узлы
- **User**: данные о пользователях
  - `id`: уникальный идентификатор пользователя
  - `screen_name`: screen_name имя пользователя
  - `name`: полное имя (first_name + last_name)
  - `sex`: пол
  - `home_town`: город или местоположение

- **Group**: данные о группах
  - `id`: уникальный идентификатор группы (с отрицательным значением)
  - `name`: название группы
  - `screen_name`: screen_name имя группы

### Связи
- **Follow**: отношение подписчика к пользователю
- **Subscribe**: отношение пользователя к группе

## Запросы и аргументы командной строки
Программа поддерживает следующие аргументы для выборки данных и управления параметрами:

- `--total_users`: возвращает общее количество пользователей.
- `--total_groups`: возвращает общее количество групп.
- `--top_users`: выводит топ 5 пользователей по количеству подписчиков.
- `--top_groups`: выводит топ 5 самых популярных групп.
- `--common_subscription`: выводит топ 3 пар пользователей, которые имеют больше всего подписок на одни и те же группы или страницы.
- `--no_parse_vk`: отключает парсинг VK API (если не указать будет парсить).

Пример использования:
```bash
python main.py --total_users --total_groups --top_users --top_groups --common_subscription --no_parse_vk
```

## Полезные ссылки

- [Документация VK API](https://vk.com/dev/manuals)
- [Neo4j](https://neo4j.com)


## Запуск
1. Клонирование репозитория
`git clone https://github.com/Pavel-Demukhametov/nosql_lab04.git`

2. Переход в папку с репозиторием
`cd <название папки> в которую клонировали репозиторий`

3. Создание виртуального окружения
Windows
`python -m venv venv`
Linux 
`python3 -m venv venv`

4. Активация виртуального окружения
Windows
`.\venv\Scripts\activate`
Linux
`source venv/bin/activate`

5. Установка зависимостей.
`pip install -r requirements.txt`

6. Установка переменных окружения
   **Подключение к базе данных Neo4j:**
   - `NEO4J_BOLT_URL` — URL для подключения к Neo4j через протокол Bolt. По умолчанию: `bolt://localhost:7687`.
   - `NEO4J_USERNAME` — имя пользователя для подключения к базе данных Neo4j. По умолчанию: `neo4j`.
   - `NEO4J_PASSWORD` — пароль для подключения к базе данных Neo4j. По умолчанию: `12345678`.
   **Токен для VK API:**
   - `VK_TOKEN` — токен для доступа к API ВКонтакте
   
   Пример:
   ```
        VK_TOKEN = <Ваш токен>
        NEO4J_BOLT_URL = <Ваш URL>
        NEO4J_USERNAME = <Ваше имя пользователя>
        NEO4J_PASSWORD = <Ваш пароль>
    ```
 
7. Запуск. 
`python main.py --total_users --total_groups --top_users --top_groups --common_subscription`

8. Если не поставлено --no_parse_vk, после запуска надо ввести в консоль ID пользователя или его screen_name (Если ничего не ввести будет выолнено для страницы https://vk.com/dm). 

## Maintainers

- Developed by [Pavel Demukhametov](https://github.com/Pavel-Demukhametov)