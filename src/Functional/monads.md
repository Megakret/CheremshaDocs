# Монады

В языке Cheremsha вы можете писать монады.

---
## Пример монады Result
> [!IMPORTANT]
> Рекомендуется ознакомиться с главой [Утилиты](./utils.md) перед прочтением примера.

Пример с монадой Result из F#. Если в пайплайн попала ошибка, то она вернется в конце пайплайна.
```
(
    func result (x) (
        (map (("Result" x)))
    )
)
(
    func error (x)(
        (map (("Error" x)))
    )
)
(
    func result-bind (f x)
    (
        (
            if (map-get x "Error")
            (
                (error x)
            )
            (
                (f (map-get x "Result"))
            )
        ) 
    )
)
(func div (x y)
(
    (
        if (= y 0)
        (
            (error "Division by 0")
        )
        (
            (result (/ x y))
        )
    )
)
)
(
    func adder (x)(
        (la (y) (( pipe (list (+ x) result) y)) )
    )
)
(pipe-monadic result-bind (list (la (x) ((div 5 x))) (adder 5) ) (result 0))
(pipe-monadic result-bind  (list (la (x) ((div 5 x))) (adder 5) ) (result 2))
```