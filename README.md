# Анализ задач и поиск архитектурного решения для проекта интеграции системы документооборота заказчика с операторами ЭДО

## Бизнес-контекст

У заказчика есть внутренняя система документооборота. Система обслуживает и связывает географически разнесённые филиалы. Пользователями являются тысячи сотрудников. Так же заказчик осуществляет документооборот со своими контрагентами через несколько существующих операторов ЭДО.
## Контекстная схема системы

Текущая архитектура взаимодействия с операторами ЭДО.

![](Untitled1.png)

![](Учёба/Архитектура/Software%20Architect/Итоговый%20проект/images/Untitled.png)
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
    - покупка нового сервера: \$$
    - выделение существующего физического сервера: 0
- **Стоимость разработки:** $
- **Модификация:** \$$

**Стэк:**

- API:
    - C# \$$ TT
    - Java \$$ TT
- БД:
    - PostgreSQL $ T
    - MySQL \$$ TT
    - MSSQL \$\$$ TT

**Разработка:**

- Переучивать команду
    - C# 0 0
    - Java \$\$$ TTT
- Аутсорс \$$ TT
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
    - покупка новых серверов под каждый сервис: \$\$$
- **Стоимость разработки**: \$\$$
- **Модификация:** \$$

**Стэк:**

- API:
    - C# \$$ TT
    - Java \$$ TT
- БД:
    - PostgreSQL $ T
    - MySQL \$$ TT

**Разработка:**

- Переучивать команду
    - C# $ T
    - Java \$\$$ TTT
- Аутсорс \$$ TT
- Фриланс $ T

**Альтернативы**

Использование для разработки системы таких технологий, как Go, php, Kotlin, Python не рассматривается, т.к. подразумевает длительное обучение, дороговизну разработки и трудность поддержки. На данный момент в рамках департамента разработки есть две команды, чей стэк включает Java и C#.

MSSQL: отклонено в виду ориентирования на импортозамещения и дороговизны лицензирования и разработки.

**Последствия**

- **Положительные:** отказоустойчивость, горизонтальная масштабируемость, TimeToMarket, возможность разбиения на команды.
- **Отрицательные**: сложность разработки, сложность сопровождения.

## Декомпозиция 
### Предметная область as is

![](Pasted_image_20240915154904.png)
### Функциональная декомпозиция as is

![](63e988c00a5606a765384726f9c70a67.png)
### Предметная область to be

![](Pasted_image_20240915211759.png)

### Функциональная декомпозиция to be

![](Pasted_image_20240915201552.png)
### Сценарии изменений

- администратор может создать запрос на: 
	- повторное получение архива;
	- повторное получение печатной формы документа;
	- повторную отправку документа;
	- обработку события оператора.
- пользователь может зарегистрировать документ и отправить его;
- пользователь может создать запрос на:
	- подписание входящего документа;
	- отказ в подписании входящего документа;
	- запрос аннулирования;
	- отказ в запросе аннулирования.
### Решение в срезе сущностей

Выделено 5 сущностей: пользователь (П), запрос (З), внутренний документ (Д), очередь на обработку (О), администратор (А)

![](Pasted_image_20240915212107.png)
#### Оценка стоимости изменений

- администратор может создать запрос на:
	- повторное получение архива (А, З, О);
	- повторное получение печатной формы документа (А, З, О);
	- повторную отправку документа (А, З, О);
	- обработку события оператора (А, З, О).
- пользователь может зарегистрировать документ и отправить его (П, Д, З, О);
- пользователь может создать запрос на:
	- подписание входящего документа (П, Д, З, О);
	- отказ в подписании входящего документа (П, Д, З, О);
	- запрос аннулирования (П, З, О);
	- отказ в запросе аннулирования (П, З, О).
### Решение в срезе ограниченных контекстов

Выделено 2 контекста:
#### Контекст пользователя (КП)

![](Pasted_image_20240915211836.png)
#### Контекст администратора (КА)

![](Pasted_image_20240915211913.png)
#### Оценка стоимости изменений

- администратор может создать запрос на: 
	- повторное получение архива (КА, КП);
	- повторное получение печатной формы документа (КА, КП);
	- повторную отправку документа (КА, КП);
	- обработку события оператора (КА, КП).
- пользователь может зарегистрировать документ и отправить его (КА, КП);
- пользователь может создать запрос на:
	- подписание входящего документа (КА, КП);
	- отказ в подписании входящего документа (КА, КП);
	- запрос аннулирования (КА, КП);
	- отказ в запросе аннулирования (КА, КП).
### Сравнительный анализ

Оба способа практически идентичны по затратам. 
### Вывод

Если в документе сосредоточится большая часть логики, то предпочтительнее вариант 1.
## Диаграмма последовательности

![](Диграмма%20последовательности.png)
## Оценка атрибутов качества
### Надёжность
Сервис интеграции использует БД для сохранения событий и состояний обрабатываемых объектов. Это позволяет продолжить обработку начиная с момента отказа или, в случае перезапуска сервиса, с последнего сохранённого события. Предусмотрено ручное управление обработкой определённых событий и документов, т.е. в случае, когда происходит отказ на моменте обработке события и автоматическая повторная обработка не производится, можно через интерфейс взаимодействия с сервисом разместить нужный запрос в очереди на обработку.
### Масштабируемость
Для сервиса доступно горизонтальное масштабирование в виде репликации БД и развёртывания дополнительных экземпляров сервиса. 
### Отказоустойчивость
Структура БД позволяет запустить несколько экземпляров сервиса и за счёт очередей на обработку распределить между ними нагрузку. Это позволит обеспечить бесперебойную работу интеграции. 
### Сопровождаемость
Сервис реализован на стеке технологий, полностью поддерживаемом группой разработки. Для проведения тестирования изменений в сервисе используются юнит-тесты.
### Производительность
Сервис использует событийную модель, что позволяет сократить до нескольких секунд временной лаг между наступлением события у оператора ЭДО и его фиксацией на стороне системы. 
### Модифицируемость
Архитектура сервиса представляет собой сервис-ориентированную архитектуру, где сервисы общаются между собой через базу данных путём размещения запроса в очереди на обработку. Изменения одного сервиса не затрагивают другие. 