hexlet https://ru.code-basics.com/languages/racket
Без указания #lang racket интерпритатор не сработает


===
#lang racket
(displayln "Hello, World!") 
===

Lisp = LISt Processor (Обработчик списков). Односвязный список - онсовная структура данных.
Любая программа - сама по себе список.

Гомоиконичность - Код на Lisp Одновременно является данными Lisp-языка. Позволяет писать макросы, работающие с исходным кодом как со списком.

Список как дерево.
Списки могут быть вложенными.
Пример. (+ 1 (- 3 2)) ; сложение или список из трёх элементов, в котором третий элемент — список из трёх элементов
Список - рекурсивная структура данных. Любой элемент может быть списком и содержать внутри себя элементы-списки.

L5 ===
`Порядок вычисления`
задача 5 + 7 + (8 - 3) - (8 * 5)
решение (- (+ 5 7 (- 8 3)) (* 5 8))

L6 ====
`Скобки`


L7 ====
`Объявление символов`
переменные создаются с помощью конструкции define и называются объявлениями.

(define id expr)
; id — идентификатор
; expr — выражение

Для изменения значения объявления используется функция set! (использование set! не рекомендовано
лучше заменять все на константы.)
(set! lang "scheme")
(displayln lang) ; => scheme

L8 === 
`Создание и вызов функций`
; определение функции, вычисляющей сумму двух чисел
(lambda (x y) (+ x y))

тело может состоять из нескольких форм (как минимум из одной):
(lambda ()
  (displayln "one")
  (displayln "two"))
  Возвращается ВСЕГДА последнее вычисленное выражение.

примеры
; печать на экран
(lambda () (displayln "hello!"))
; квадрат числа
(lambda (n) (* n n))

; среднее между двумя числами
(lambda (num1 num2) (/ (+ num1 num2) 2))

(define cube (lambda (x) (* x x x))) ; функция вычисляющая куб переданного числа.

L9 ====
`Вызов функции без define`
((lambda (n) (* n n)) 5)
в форме вызова сразу после функции перечисляются параметры.

Пример для двух параметров.
((lambda (x y) (+ x y))
 8 7) ; 15

пример объявления и вызова функции:
 (define result
  ((lambda (num1 num2) (/ (+ num1 num2) 2))
   2 4))

(displayln result)

L10 ====
`Сокращенный синтаксис создания функции`
Полная запись
(define square (lambda (n) (* n n)))

Сокращенная запись
(define (square n) (* n n))

ример объявления функции с двумя аргументами:

(define (sum x y) (+ x y))

Сумма квадратов двух чисел.
(define (sum-of-squares x y) (+ (* x x) (* y y)))

L11 ====
`Модули`
Обычно каждый файл содержит ровно один модуль.
По умолчанию все объявления, сделанные в модуле, остаются внутри модуля.
импорт объявлений из других модулей проихсодит с помощью формы requre

(require "math.rkt") // пример.

Нельзя исопльзовать объявления другого модуля если модуль не экспортирует их явно. Для экспорта объявлений используется форма provide

; math.rkt
#lang racket
(provide sum)
(define (sum a b) (+ a b))

В provide пеерчисляются именя объявлений, которые нужно экспортировать. Любой другой модуль автоматически получает доступ ко всем экспортируемым объявлениям при импорте.
экспортивровать все объявления модулям можно с применением формы (provide (all-defined-out))

require работает с путями (абсолютными и относительными)
require автоматически делает доступным все, что в импортируемом модуле указано в provide.
Существует альтернативный способ вызова require
(require (only-in "math.rkt" sum)) - only-in говорит что надо включить из модуля переменную sum и больше ничего.

L12 =====
`Локальные объявления`
define может использоваться и в нутри функций 
(define (f)
  (define text "lorem")
  (displayln text))
Объявления переменных должно идти в самом анчале функции, до любых других выражений.
(let ([text "lorem"]) (displayln text)) ; => lorem

Все объявления внутри let доступны только в выражениях, которые вызываются внутри самого let после списка объявлений(ОБЪЯВЛЕНИЯ НЕ ВИДЯТ ДРУГ ДРУГА)
(let ([x 2]
      [y (+ 4 3)])
  (+ x y)) ; 9

(define (sum-of-squares x y)
  (let ([x-square (* x x)]
        [y-square (* y y)])
    (+ x-square y-square)))

(define (sum-of-squares x y)
  (let ([square (lambda (n) (* n n))])
    (+ (square x) (square y))))
(sum-of-squares 8 7) ; => 113

Объявления созданные в рамках одного использования let, не видны друг другу. Форма let так устроена что каждое объявление из списка она создает "с чистого листа"
Для того чтобы использовать в последующих объявлениях предыдущие, можно исользовать форму let*.
(let* ([x 2]
       [y (* x 20)]
       [z (+ x y)])
  z)                ; 42

  `HOMEWORK CODE`
#lang racket

(provide (all-defined-out))

#| BEGIN (write your solution here) |#
(define (square-of-sum x y)
  (let ([sum (lambda (x y) (+ x y))])
   (* (sum x y) (sum x y))))
#| END |#

L12 =====
`Логические операторы`
(equal? a b) проверка равенства сравнение по ссылке eq?
П1 х>y
(define (gt? x y) (> x y))

П2  четность
(define (even? n) (= (remainder n 2) 0)) где remainder - встроенная функция отстаток от деления.

пример логических операторов
(not "moon") ; #f
(and (odd? 3) (even? 4)) ; #t

`HOMEWORK CODE`
Реализовать функция same-parity? которая принимает на вход два числа и возвращает #t в том случае если их четность совпадает.
#lang racket

(provide (all-defined-out))

#| BEGIN (write your solution here) |#
(define (same-parity? x y) 
  (equal? 
    (= (remainder x 2) 0)
    (= (remainder y 2) 0)
    )
   )
#| END |#


L14 =====
`Услоная конструкция`
(if test-expr then-expr else-expr)
(if (> 3 2) (displayln "yes") (displayln "no")) ; => yes

`HOMEWORK CODE`
Реализовать функцию sentece-type, которая возвращает строку cry если впереданный текст написан загалвными буквами и возвращает строку common в остальных случаях.
#lang racket

(provide (all-defined-out))

#| BEGIN (write your solution here) |#
(define (sentence-type x)  
        (if (equal? (string-upcase x) x ) "cry" "common")
        )
#| END |#

L15 =====
`Услоная конструкция`
When и Unless

When
(when test-expr body ...+) Если результат test-expr истина, то вычисляется тело.

(when (positive? -5)
  (display "hi"))

(when (positive? 5)
  (display "hi")
  (display " there"))

Unless
(when (not test-expr) body ...+). unless работает наоборот. Тело вычисляется в том случае, если test-expr – ложь. unless хоть и бывает удобен, но резко становится нечитаемым, когда в test-expr появляются составные условия.

(unless (positive? 5)
  (display "hi"))
(unless (positive? -5)
  (display "hi")
  (display " there"))


`HOMEWORK CODE`
Реализуйте функцию say-boom, которая возвращает строку Boom!, если ее вызвали с параметром "go"

#lang racket

(provide (all-defined-out))

#| BEGIN (write your solution here) |#
(define (say-boom inptstr)  
        (when (equal? inptstr "go")
          "Boom!")
        )
#| END |#

L15 =====
`Case`
Пример:
(let ([v 0])
  (case v
    [(0) "zero"]
    [(1) "one"]
    [(2) "two"]
    [(3 4 5) "many"]))
; "zero"

Каждая ветка в case описывается квадратными скобками. В левой части список из одного или нескольких эелментов., в правой части возвращаемое значение.

(case 6
  [(0) "zero"]
  [(1) "one"]
  [(2) "two"]
  [else "many"])  - поведение по умолчанию.
; "many"


`HOMEWORK CODE`
Реализовать функцию humanize-permission, которая принимает на вход символьное обозначение прав доступа в Unix системах, и возвращает текстовое описание.

#lang racket

(provide (all-defined-out))

#| BEGIN (write your solution here) |#
(define (humanize-permission inptstr)
        (case inptstr
          [("x") "execute"]
          [("r") "read"]
          [("w") "write"])
)
#| END |#