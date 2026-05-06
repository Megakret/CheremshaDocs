# Списки

Список — упорядоченная последовательность элементов произвольного типа. Реализован как односвязный список. `nil` — пустой список, эквивалентен `(list)`.

## list
Создаёт список из переданных аргументов. Все аргументы вычисляются до создания списка.

`(list expr1 expr2 ... exprN)`

```
(list 1 2 3)        // List [1, 2, 3]
(list "a" "b" "c")  // List ["a", "b", "c"]
(list)              // nil
```

## head
Возвращает первый элемент списка.

`(head list)`

```
(head (list 1 2 3))    // 1
(head (list "hello"))  // "hello"
(head nil)             // ошибка: head: empty list
```

## tail
Возвращает список без первого элемента. Если список содержит один элемент — возвращает `nil`.

`(tail list)`

```
(tail (list 1 2 3))  // List [2, 3]
(tail (list 1))      // nil
(tail nil)           // ошибка: tail: empty list
```

## cat
Конкатенирует два списка. `nil` трактуется как пустой список.

`(cat list1 list2)`

```
(cat (list 1 2) (list 3 4))  // List [1, 2, 3, 4]
(cat nil (list 1 2))         // List [1, 2]
(cat (list 1 2) nil)         // List [1, 2]
(cat nil nil)                // nil
(cat 1 (list 2 3))           // ошибка: cat: expected 2 lists
```

## Проверка типа
`is-type` с аргументом `list` возвращает `true` для любого списка, включая `nil`.

```
(is-type list (list 1 2))  // true
(is-type list nil)         // true
(is-type list 42)          // false
```

## Итерация
Списки не имеют встроенного цикла — обход выполняется через рекурсию. Паттерн: проверить список на пустоту через `if`, обработать `head`, рекурсивно обработать `tail`.

Сумма элементов:
```
(
    func sum (lst)
    (
        (if lst
            ((+ (head lst) (sum (tail lst))))
            0
        )
    )
)

(sum (list 1 2 3 4 5))  // 15
```

Фильтрация:
```
(
    func filter-positive (lst)
    (
        (if lst
        (
            (let
            (
                (h (head lst))
                (rest (filter-positive (tail lst)))
            )
            )
            (if (> h 0)
                (cat (list h) rest)
                rest
            )
        )
        nil
        )
    )
)

(filter-positive (list -1 2 -3 4 5))  // List [2, 4, 5]
```
