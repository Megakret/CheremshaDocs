# Проверка типов

Cheremsha предоставляет встроенную функцию для проверки типа значения в runtime.

## is-type
Возвращает `true` если значение `expr` имеет тип `type`, иначе `false`.

`(is-type type expr)`

`type` — символ названия типа. `expr` — вычисляемое выражение.

### Поддерживаемые типы
| Символ    | Тип                          |
|-----------|------------------------------|
| `int`     | целое число                  |
| `float`   | число с плавающей точкой     |
| `boolean` | булево значение              |
| `str`     | строка                       |
| `list`    | список (включая `nil`)       |
| `func`    | функция (замыкание)          |

```
(is-type int 42)             // true
(is-type int 3.14)           // false
(is-type float 3.14)         // true
(is-type boolean true)       // true
(is-type str "hello")        // true
(is-type list nil)           // true
(is-type list (list 1 2 3))  // true
(is-type func (la (x) (x)))  // true
```

### Каррирование
`is-type` поддерживает каррирование — можно создать функцию-предикат для конкретного типа:

```
(let ((is-int (is-type int))))

(is-int 42)    // true
(is-int 3.14)  // false
```

### Замечания
- Проверка типа выполняется в runtime, а не на этапе компиляции
- `nil` считается списком: `(is-type list nil)` возвращает `true`
- Для функций, созданных через `func`, тип тоже `func`
