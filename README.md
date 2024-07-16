# Анализ задач и поиск архитектурного решения для проекта интеграции системы документооборота заказчика с операторами ЭДО

## Бизнес-контекст

У заказчика есть внутренняя система документооборота. Система обслуживает и связывает географически разнесённые филиалы. Пользователями являются тысячи сотрудников. Так же заказчик осуществляет документооборот со своими контрагентами через несколько существующих операторов ЭДО.

## Контекстная схема системы

![Untitled](/images/Untitled.png)

Текущая архитектура взаимодействия с операторами ЭДО.

![Untitled](/images/Untitled%201.png)

## Бизнес-цели

Автоматизировать деятельность по документообороту с контрагентами в рамках существующей внутренней системы документооборота путём разработки новой высокопроизводительной системы, позволяющей сократить издержки существующей корреспонденции с контрагентами.

## Бизнес-драйверы

- на осуществление документооборота с контрагентами тратится много времени;
- данные по контрагентам обновляются слишком медленно во внутренней системе документооборота;
- данные по договорам не всегда актуальны во внутренней системе документооборота.

## Стейкхолдеры и их потребности

- Администратор – управляет работой интеграции, имеет доступ к логам и формирует запрос в службу техподдержки;
- Сотрудник-подписант – подписывает исходящие документы выданным ему сертификатом как представитель компании;
- Сотрудник-оператор – получает входящую корреспонденцию, изменяет состояние входящих и исходящих документов в зависимости от бизнес-процесса, формирует исходящий документ;
- Команда техподдержки – принимает обратную связь от администраторов в виде логов и сообщения о работе системы, от пользователей в виде сообщений об ошибках;
- Команда разработки.

## Пользовательские истории

1. **Управление и мониторинг:**  Администратор управляет работой модулей системы, ведёт мониторинг деятельности системы, собирает логи и сообщает о неисправностях в функционировании;
2. **Отправка исходящего документа:** Сотрудник-оператор формирует исходящий документ в соответствующем журнале регистрации, заполняет его атрибуты, указывает подписанта.
3. **Подписание исходящего документа:** Сотрудник-подписант выданным ему сертификатом подписывает исходящий документ как представитель организации;
4. **Управление состоянием входящих и исходящих документов** Сотрудник-оператор изменяет состояние входящих и исходящих документов в зависимости от бизнес-процесса. Если требуется подпись к документу для изменения состояния система использует технический сертификат;
5. **Получение полного документооборота по документу** Сотрудник-оператор формирует запрос на получение от оператора архива с полным документооборотом за всё время по заранее выбранному документу;
6. **Получение печатной формы по документу** Сотрудник-оператор формирует запрос на получение от оператора печатной формы (версию документа в формате PDF) от оператора по заранее выбранному документу;
7. **Работа с тикетами** Пользователь формирует тикет с сообщение об ошибке, обнаруженной в процессе работы с системой, и отслеживает состояние тикета.

## Атрибуты качества (и не функциональные требования)

- Сервисы и база данных должны быть высокодоступными, т.к. для бизнеса критична быстрая реакция на изменения в процессе документооборота с контрагентами;
- Время регистрации входящих документов должно составлять несколько секунд;
- Время отправки исходящих документов должно составлять несколько секунд;
- Время изменения состояния документа не больше 3 секунд;
- Сервисы должны быть отказоустойчивые, чтобы обеспечивать беспрерывный поток документооборота;
- Логи должны быть легкодоступны, физически разделены по сервисам и содержать удобочитаемую информацию;
- Управление сервисами должно осуществляться инструментами операционной системы;
- В системе не должно быть пропущенных тикетов.

## Критические сценарии

- Сотрудник-оператор изменяет состояние документа в система оператора ЭДО через систему внутреннего документооборота организации;
- Сотрудник-оператор отправляет документ в систему оператора ЭДО через систему внутреннего документооборота организации;
- Сотрудник-оператор получает документ из системы оператора ЭДО в системе внутреннего документооборота организации;
- Сотрудник-подписант подписывает исходящие из внутренней системы документооборота организации документы в этой же системе;

## Критические характеристики

- Регистрация одного входящего документа – 95% квантиль не должен превышать 10 секунд;
- Отправка исходящего документа – 95% квантиль не должен превышать 10 секунд;
- Изменение состояния документа – 95% квантиль не должен превышать 3 секунд;
- Надёжность – не должно быть потерянных входящих и исходящих документов, изменений состояний документов, тикетов;
- Up-time в будние дни должен быть 99,9%, в выходные дни и праздники 95%;
- Время разработки: …
- Стоимость: …

## Два первых архитектурных решения запишите в виде ADR

### Решение 1

**Статус:**

proposed

**Бэк:** Сервер с API, БД.

**Функционал (MVP):**  отправка документов, получение документов, изменить состояние документа по запросу.

**Архитектура:**

SBA – Service-based Architecture (SOA).

- **Основной функциональный сценарий:** отправка документов, получение документов, изменить состояние документа по запросу;
- **Особенности*:*** все сервисы работают с одной БД, модульность;
- **Аппаратная часть:**
    - виртуальный сервер на существующих мощностях: $
    - покупка нового сервера: $$
    - выделение существующего физического сервера: 0
- **Стоимость разработки:** $$
- **Модификация:** $$$

**Стэк:**

- API:
    - C# $$ TT
    - Java $$ TT
- БД:
    - PostgreSQL $ T
    - MySQL $$ TT
    - MSSQL $$$ TT

**Разработка:**

- Переучивать команду
    - C# 0 0
    - Java $$$ TTT
- Аутсорс $$ TT
- Фриланс $ T

**Альтернативы:**

Использование для разработки системы таких технологий, как Go, php, Kotlin, Python не рассматривается, т.к. подразумевает длительное обучение, дороговизну разработки и трудность поддержки. На данный момент в рамках департамента разработки есть две команды, чей стэк включает Java и C#.

MSSQL: отклонено в виду ориентирования на импортозамещения и дороговизны лицензирования и разработки.

**Последствия**

- **Положительные:** рентабельность, нет дополнительного усложнения в виде слоя общения между сервисами, сопровождение, развёртывание;
- **Отрицательные:** требовательность к серверу, БД представляется уязвимым местом;

### Решение 2

**Статус:**

proposed

**Бэк:** Сервер с API, БД.

**Функционал (MVP):**  отправка документов, получение документов, изменить состояние документа по запросу.

**Архитектура:**

MSA – Microservice Architecture

- **Основной функциональный сценарий:** отправка документов, получение документов, изменить состояние документа по запросу;
- **Особенности*:*** контракт, каждый сервис работает со своей БД, модульность, межсервисные связи, большое количество сервисов.
- **Аппаратная часть:**
    - виртуальные машины на существующих мощностях: $
    - покупка новых серверов под каждый сервис: $$$
- **Стоимость разработки**: $$$
- **Модификация:** $$

**Стэк:**

- API:
    - C# $$ TT
    - Java $$ TT
- БД:
    - PostgreSQL $ T
    - MySQL $$ TT

**Разработка:**

- Переучивать команду
    - C# $ T
    - Java $$$ TTT
- Аутсорс $$ TT
- Фриланс $ T

**Альтернативы**

Использование для разработки системы таких технологий, как Go, php, Kotlin, Python не рассматривается, т.к. подразумевает длительное обучение, дороговизну разработки и трудность поддержки. На данный момент в рамках департамента разработки есть две команды, чей стэк включает Java и C#.

MSSQL: отклонено в виду ориентирования на импортозамещения и дороговизны лицензирования и разработки.

**Последствия**

- **Положительные:** отказоустойчивость, горизонтальная масштабируемость, TimeToMarket, возможность разбиения на команды.
- **Отрицательные**: сложность разработки, сложность сопровождения.