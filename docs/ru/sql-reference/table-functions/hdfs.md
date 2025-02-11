---
slug: /ru/sql-reference/table-functions/hdfs
sidebar_position: 45
sidebar_label: hdfs
---

# hdfs {#hdfs}

Создаёт таблицу из файла в HDFS. Данная табличная функция похожа на табличные функции [url](url.md) и [file](file.md).

``` sql
hdfs(URI, format, structure)
```

**Входные параметры**

-   `URI` — URI файла в HDFS. Путь к файлу поддерживает следующие шаблоны в режиме доступа только для чтения `*`, `?`, `{abc,def}` и `{N..M}`, где `N`, `M` — числа, \``'abc', 'def'` — строки.
-   `format` — [формат](../../interfaces/formats.md#formats) файла.
-   `structure` — структура таблицы. Формат `'column1_name column1_type, column2_name column2_type, ...'`.

**Возвращаемое значение**

Таблица с указанной структурой, предназначенная для чтения или записи данных в указанном файле.

**Пример**

Таблица из `hdfs://hdfs1:9000/test` и выборка первых двух строк из неё:

``` sql
SELECT *
FROM hdfs('hdfs://hdfs1:9000/test', 'TSV', 'column1 UInt32, column2 UInt32, column3 UInt32')
LIMIT 2
```

``` text
┌─column1─┬─column2─┬─column3─┐
│       1 │       2 │       3 │
│       3 │       2 │       1 │
└─────────┴─────────┴─────────┘
```

## Шаблоны поиска в компонентах пути {#globs-in-path}

-   `*` — Заменяет любое количество любых символов кроме `/`, включая отсутствие символов.
-   `?` — Заменяет ровно один любой символ.
-   `{some_string,another_string,yet_another_one}` — Заменяет любую из строк `'some_string', 'another_string', 'yet_another_one'`. В случае, если в какой-либо из строк содержится `/`, то ошибки доступа (permission denied) к существующим, но недоступным директориям/файлам могут быть проигнорированы при помощи настройки [ignore_access_denied_multidirectory_globs](/docs/ru/operations/settings/settings.md#ignore_access_denied_multidirectory_globs).
-   `{N..M}` — Заменяет любое число в интервале от `N` до `M` включительно (может содержать ведущие нули).

Конструкция с `{}` аналогична табличной функции [remote](remote.md).

:::danger Предупреждение
Если ваш список файлов содержит интервал с ведущими нулями, используйте конструкцию с фигурными скобками для каждой цифры по отдельности или используйте `?`.
:::

Шаблоны могут содержаться в разных частях пути. Обрабатываться будут ровно те файлы, которые и удовлетворяют всему шаблону пути, и существуют в файловой системе.

## Виртуальные столбцы {#virtualnye-stolbtsy}

-   `_path` — Путь к файлу.
-   `_file` — Имя файла.

**Смотрите также**

-   [Виртуальные столбцы](index.md#table_engines-virtual_columns)
-   Параметр [ignore_access_denied_multidirectory_globs](/docs/ru/operations/settings/settings.md#ignore_access_denied_multidirectory_globs)

