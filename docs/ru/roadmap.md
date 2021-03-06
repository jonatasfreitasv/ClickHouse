# Roadmap

## Q1 2018

### Новая функциональность
- Поддержка `UPDATE` и `DELETE`.
- Многомерные и вложенные массивы.
    
    Это может выглядеть например так:
    
```sql
CREATE TABLE t
(
    x Array(Array(String)), 
    z Nested(
        x Array(String), 
        y Nested(...))
)
ENGINE = MergeTree ORDER BY x
```

- Внешние MySQL и ODBC таблицы.

    Внешние таблицы можно интрегрировать в ClickHouse с помощью внешних словарей. Новая функциональность станет более удобной альтернативой для подключения внешних таблиц.

```sql
SELECT ... 
FROM mysql('host:port', 'db', 'table', 'user', 'password')`
```

### Улучшения

- Эффективное копирование данных между кластерами ClickHouse.

    Сейчас можно копировать данные функцией remote(), например: `
INSERT INTO t SELECT * FROM remote(...) `.

    Производительность этой операции улучшится.

- O_DIRECT for merges.

    Улучшит производительность кэша операционной системы, а также производительность 'горячих' запросов.
    
## Q2 2018

### Новая функциональность

- UPDATE/DELETE соответствующие Europe GDPR.
- Входные и выходные форматы Protobuf и Parquet.
- Создание словарей с помощью DDL-запросов.

    Сейчас, словари, которые являются частью схемы базы данных, определены во внешних XML-файлах. Это неудобно и неочевидно. Новый подход должен это исправить.

- Интеграция с LDAP.
- WITH ROLLUP и WITH CUBE для GROUP BY.
- Настраиваемые кодировки и сжатие для каждого столбца в отдельности.

    Сейчас, ClickHouse поддерживает сжатие столбцов с помощью LZ4 и ZSTD, и настройки сжатия глобальные (смотрите статью [Compression in ClickHouse](https://www.altinity.com/blog/2017/11/21/compression-in-clickhouse)). Поколоночное сжатие и кодирование обеспечит более эффективное хранение данных, что в свою очередь ускорит выполнение запросов.
    
- Хранение данных на нескольких дисках на одном сервере.

    Реализация это функциональности упростит расширение дискового пространства, поскольку можно будет использовать различные дисковые системы для разных баз данных или таблиц. Сейчас, пользователи вынуждены использовать символические ссылки, если базы данных и таблицы должны храниться на другом диске.
    
### Улучшения

Планируется множество улучшений и исправлений в системе выполнения запросов. Например:

- Использование индекса для `in (subquery)`.

    Сейчас, индекс не используется, что приводит с снижению производительности.
    
- Передача предикатов из `where` в подзапросы, а также передача предикатов в представления.

    Передача предикатов необходима, поскольку представление изменяется поздапросом. Сейчас производительность фильтров для представлений низкая, представления не могут использовать первичный ключ оригинальной таблицы, что делает представления для больших таблиц бесполезными.
    
- Оптимизация операций с ветвлением (тернарный оператор, if, multiIf).

    Сейчас, ClickHouse выполняет все ветви, даже если в этом нет необходимости.

- Использование первичного ключа для GROUP BY и ORDER BY.

    Может ускорить отдельные типы запросов в которых данные частично отсортированы.

## Q3-Q4 2018

На данный период планы ещё не сформированы, однако можно указать основные проекты:

- Пулы ресурсов для выполнения запросов.

    Позволят более эффективно управлять нагрузкой.
    
- Синтаксис ANSI SQL JOIN.

    Улучшит совместимость ClickHouse со множеством SQL-инструментов.
