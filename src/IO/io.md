# Ввод-вывод (IO)

Все функции и константы для работы с вводом-выводом имеют префикс `io.`. В языке определены два базовых типа потоков: `input` (чтение) и `output` (запись). Потоком может быть как файл, так и стандартные потоки ввода-вывода.

## Стандартные потоки
Для удобства работы с терминалом предусмотрены встроенные алиасы:

- **`io.stdin`**: Стандартный поток ввода (тип `input`).
- **`io.stdout`**: Стандартный поток вывода (тип `output`).

---

## io.open-read
Открывает файл для чтения. Возвращает объект типа `input`.

`(io.open-read filename)`

```
(let ((file (io.open-read "data.txt"))))
(io.readline file) // считает строку
```

## io.open-write
Открывает файл для записи. Возвращает объект типа `output`.

`(io.open-write filename)`

```
(let ((file (io.open-write "log.txt"))))
(io.write file "Hello, world!") // запишет Hello world
```

## io.read
Читает из потока `input` указанное количество символов. Возвращает список, состоящий из двух элементов: прочитанной строки и нового экземпляра потока.

`(io.read input char_count)`

```
(let ((res (io.read io.stdin 5))))
(head res) // Возвращает строку из 5 символов
```

## io.readline
Читает одну строку из потока `input`. Аналогично `read`, возвращает список из `(string, input)`.

`(io.readline input)`

```
(
    func read-and-print ()
    (
        (let ((input-data (io.readline io.stdin))))
        (io.write io.stdout (head input-data))
        
    )
)
```

## io.write
Записывает строку в поток `output`. Возвращает обновленный экземпляр потока `output`.

`(io.write output string)`

```
(io.write io.stdout "Ready.") // Выводит "Ready." в терминал
(io.write (io.open-write "test.txt") "Data") // Записывает в файл
```

## Проверка типа
Для проверки потоков в функции `is-type` используются соответствующие символы.

```
(is-type input-stream io.stdin)   // true
(is-type output-stream io.stdout) // true
```

## Пример: Чтение файла целиком
Поскольку итерация в языке реализуется через рекурсию, чтение данных до конца потока (через проверку возвращаемого значения на пустую строку) выглядит так:

```
(
    func print-file (file)
    (
        (let ((res (io.readline file))))
        (let ((line (head res))))
        (if (not (= line ""))
            (
                (let ((_ (io.write io.stdout line))))
                (print-file (head (tail res))) // Передаем обновленный поток
            )
            nil
        )
    )
)
```