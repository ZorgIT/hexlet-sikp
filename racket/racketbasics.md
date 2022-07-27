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

Нельзя использовать объявления другого модуля если модуль не экспортирует их явно. Для экспорта объявлений используется форма provide

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
Объявления переменных должно идти в самом начале функции, до любых других выражений.
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

L16 =====
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

L17 =====
`Cond` (несолько условий)
пример:
(cond
  [(positive? -5) "first return"]
  [(zero? -5) "second return"]
  [(positive? 5) "third return"]
  [else "boom!"])

`HOMEWORK CODE`
#lang racket

(provide (all-defined-out))

#| BEGIN (write your solution here) |#
(define (programmer-level inplvl)
  (cond
  [(and (> inplvl 0) (< inplvl 10)) "junior"]
  [(and (> inplvl 10) (< inplvl 20)) "middle"]
  [(> inplvl 20) "senior"] 
  )
)
#| END |#

L18 =====
Логические выражения
Вместо логических конструкций можно использовать if,case, cond,
Примеры:
(displayln (if #t "Ok" "Oops")) ; => Ok
(displayln (when #f "Ok"))      ; => #<void>

можно писать довольно таки сложные выражения, не выделяя промежуточные вычисления в переменные:
(define (classify x)
  (cond
    [(< x 0) "Negative"]
    [(case x
       [(13 42 100500) #t]
       [else #f]) "Special"]
    [else "Boring"]))

Пример с Let
(define (classify x)
  (let ([is-special
         (case x
           [(13 42 100500) #t]
           [else #f])])
    (cond
      [(< x 0) "Negative"]
      [is-special "Special"]
      [else "Boring"])))

#lang racket

`HOMEWORK CODE`
(provide (all-defined-out))

#| BEGIN |#
(define (do-today day-of-week)
  (cond
    [(and (integer? day-of-week)
          (<= 1 day-of-week 7))
     (case day-of-week
       [(6 7) "rest"]
       [else "work"])]
    [else "???"]))
#| END |#

L19 =====
`Объявление списков`

(list 1 2 3); `(1 2 3)`
(list "apple" "orange"); `("apple" "orange")`
(list 1 (list 2 3) 4); `(1 (2 3) 4)`

функция предикат list? вернет #t #f

`HOMEWORK CODE` Реализуйте функцию triple, которая должна принимать один аргумент любого типа и возвращать список, в котором содержится три копии аргумента:
#lang racket
(provide (all-defined-out))

#| BEGIN (write your solution here) |#
(define (triple inpt)
  (list inpt inpt inpt)
)
#| END |#

L20 =====
`Встроенные средства обхода списков, map`
встроенная функция add1 - добавляет к числу единицу

применение Map:
(map add1 (list 1 2 3 )); '(2 3 4) - map превращает старый список в новый применяя функцию к каждому элементу.
map можно применить к нескольким спискам, но размерность должна быть одинаковой
(map +
  (list 1 2 3)
  (list 10 20 30)
  (list 100 200 300)); '(111 222 333)

`HOMEWORK CODE` 
Реализуйте функцию maps, которая должна принимать два списка — список функций и список списков аргументов — и возвращать список результатов применения функций к наборам аргументов.

Решение учителя:
#lang racket
(provide (all-defined-out))
#| BEGIN |#
(define (maps fs as) (map map fs as))
#| END |#

L21 =====
`Фильтрация списков, filter`
функция filter принимает два аргумента - функцию предикат (должна вернуть #t) для элементов которые нужно оставить 
и список элементов для последующего отбора. Результат - новый список. 
Примеры:
(filter integer? (list 1 2.5 "foo" 7))     ; '(1 7)
(filter positive? (list -1 5 42 0 -100 3)) ; '(5 42 3)

`HOMEWORK CODE` 
Реализуйте функцию increment-numbers, которая берёт из списка-аргумента значения, являющиеся числами (number?) и возвращает список этих чисел, увеличив предварительно каждое число на единицу (add1). 

#lang racket
(provide (all-defined-out))
#| BEGIN (write your solution here) |#
(define (increment-numbers inptlst)
  (map add1 (filter number? inptlst))
)
#| END |#

L22 =====
`Сворачивание списков, функции свертки foldl и foldr`
Суть сворачивания - применение некоторой бинанрной операции к очередному элменту списка и текущему значеиню аккумулятора с целью получить новое значение аккумулятора.
foldl лева свертка
foldr правая свертка

(foldr + 0 (list 1 2 3)) ; 6
(foldl + 0 (list 1 2 3)) ; 6

пример левой свертки с двумя списками
 (define (f greeting name acc)
   (string-append acc greeting ", " name "!\n"))

 (display
   (foldl f ""
            (list "Hello" "Hi" "Good day")
            (list "Bob" "Alice" "Tom")))
 ; => Hello, Bob!
 ; => Hi, Alice!
 ; => Good day, Tom!
 ; =>
`HOMEWORK CODE` 
Реализуйте функцию max-delta, которая должна принимать два списка чисел и вычислять максимальную разницу (абсолютное значение разницы) между соответствующими парами элементов.
Решение учителя:
#lang racket

(provide (all-defined-out))

#| BEGIN |#
(define (max-delta xs ys)
  (foldl 
    (lambda [x y m] - объявление лямбда функции
    (max m (abs (- x y)))) - состав функции ()
         0 xs ys))
#| END |#

L23 =====
`Внутреннее устройство списков, пары, cons, null`

Пара созданется с помощью функции cons:
(cons 1 2)  ; '(1 . 2) элементы пары разделяет точка
(pair? (cons 1 2)) ; #true

для доступа к элементам пары используются функции car и cdr 
(define p (cons "Bob" 42))
(car p) ; "Bob"
(cdr p) ; 42

=cons+null
список - пара из первого сохраняемого значения (голова) и другого списка наываемого хвостом 
пример сборки списка вручную:
(cons 1 (cons 2 (cons 3 null))) ; '(1 2 3)
!НЕ любая пара является списком. второй элемент пары должен сам быть списком, пусть даже пустыми, только тогда эта пара будет считаться списком.
Приеры пар:
(pair? (cons 1 2))            ; #t
(list? (cons 1 2))            ; #f - справа не список

(pair? (cons 1 null))          ; #t
(list? (cons 1 null))          ; #t - тут всё понятно

(pair? (cons 1 (cons 2 3)))   ; #t
(list? (cons 1 (cons 2 3)))   ; #f - хвост не является списком!

(list? (cons 1 (cons 2 null))) ; #t - всё хвосты являются списками

first/rest
 - первый элемент, крайний элемент работает только со списком.
 
 (first (cons 1 2))
; first: contract violation
;   expected: (and/c list? (not/c empty?))
; ...
(rest (cons 1 2))
; rest: contract violation
;   expected: (and/c list? (not/c empty?))
; ...

`HOMEWORK CODE` 
Реализуйте функцию lookup, которая бы должна принимать аргумент-ключ и список пар "ключ-значение" и возвращать либо пару "ключ-значение", где ключ равен первому аргументу, либо возвращать #f, если подходящих пар в списке не нашлось. Если подходящих пар найдётся несколько, вернуть нужно первую.
#lang racket

(provide (all-defined-out))

#| BEGIN (write your solution here) |#
(define (lookup key pairs)
  (let* ([same-key? (lambda (kv) (equal? key (car kv)))]
         [found-pairs (filter same-key? pairs)])
    (if (empty? found-pairs) #f
        (first found-pairs))))
#| END |#

L24 =====
`Обход списков и рекурсия`
!Список это пара из головы и списка-хвоста.!

Пример рекрусивного вычисления длинны списка:
(define (length lst)
  (cond
    [(empty? lst)  0]
    [(cons? lst)   (+ 1 (length (rest lst)))]))

`HOMEWORK CODE` 
Реализуйте функцию skip, которая должна принимать два аргумента — целое число n и список — и возвращать новый список, содержащий все элементы из первого списка за исключением n первых элементов. Если n окажется больше, чем количество элементов во входном списке, результатом должен быть пустой список.

#lang racket

(provide (all-defined-out))

#| BEGIN (write your solution here) |#
(define (skip n l)
  (if (or (<= n 0) (empty? l)) l
      (skip (sub1 n) (rest l))))
#| END |#

L25 =====
`Символы`
Любой текст в Racket представлен типом string, который представляет собой список фиксированноой длины, состоящий из символов - значений типа char.

Любой печтный символ можно представить в виде символа О_О:
(displayln #\a) ; => a
(displayln #\1) ; => 1
(displayln #\,) ; => ,

Управляющие символы:
#\space   ; это пробел
#\newline ; a это — перевод строки

Символы юникод:
(displayln #\λ)                  ; => λ
(displayln (integer->char 955))  ; => λ
(displayln #\u3BB)               ; => λ
(displayln #\n)                  ; => n
(displayln #\110)                ; => n

integer->char преобразует десятичное число в строковый символ с кодом 955

`HOMEWORK CODE` 
Реализуйте две функции, next-char и prev-char, которые вычисляют для символа-аргумента следующий и предыдущий символы (с точки зрения десятичного кода).
#lang racket

(provide
 next-char
 prev-char)

#| BEGIN (write your solution here) |#
(define (next-char inptchar)
  (integer->char (+ (char->integer inptchar) 1))
)


(define (prev-char inptchar)
  (integer->char (- (char->integer inptchar) 1))
)
#| END |#

L26 =====
`Создание строк`

Строковые литералы могут содержать специальные символы вроде \n, символы Unicode вроде \u03BB и экранированные двойные кавычки \"
(displayln "Bond, James\nCode name: \"007\"")
; => Bond, James
; => Code name: "007"
Строковые символы объединяются с помощью string

(define l #\l)
(string #\H #\e l l #\o #\!) ; "Hello"
(string)  

заполненная строка заданной длины - make-string
(make-string 10 #\.) ; ".........."
Длина строки string-length
(string-length (string))             ; 0
(string-length (make-string 10 #\!)) ; 10

Строки нужно обрабатывать как списки
для этого есть 2 функции string->list и list->string:

(string->list "ab") ; '(#\a #\b)
(list->string null) ; ""
(list->string (rest (string->list "Hello"))) ; "ello"

`HOMEWORK CODE` 

Реализуйте функцию next-chars, которая создаёт новую строку на основе строки-аргумента таким образом, что каждый символ новой строки является "следующим" (с точки зрения кода) по отношению к соответствующему символу исходной строки.

#lang racket

(provide next-chars)

#| BEGIN |#
(define (next-char c) (integer->char (add1 (char->integer c))))

(define (next-chars s)
  (list->string (map next-char (string->list s))))
#| END |#


L27 =====
`Сравнение строк и символов, предикаты`
для сравнения строк и симвлово используются функции - string=? char<?
тип+оператор+?

(char<?   #\a #\b)     ; #t
(char>?   #\c #\b #\a) ; #t
(char=?   #\a #\b)     ; #f
(string<? "a" "b")     ; #t
(string>? "c" "b" "a") ; #t
(string=? "a" "b")     ; #f

регистронезавсимое сравнение с суфиксом -ci (caseinsensitive)

(string=?    "Apple" "apple") ; #f
(string-ci=? "Apple" "apple") ; #t
(char>?      #\C     #\b)     ; #f
(char-ci>?   #\C     #\b)     ; #t

проверка принадлежности к основным группам символов можно спомощью предикатов  -alphabetic? char-lower-case?

(char-alphabetic? #\a)    ; #t
(char-alphabetic? #\u3BB) ; #t — "λ" это буква, пусть и греческая
(char-lower-case? #\a)    ; #t
(char-lower-case? #\A)    ; #f

проверить все символы строки на принадлежность группе можно с применением функции andmap работающей со списками и объединить со string->list
(andmap char-alphabetic? (string->list "asd"))  ; #t
(andmap char-alphabetic? (string->list "r2d2")) ; #f

Если заменить andmap на ormap, то вместо проверки "все символы…" получится проверка на "хотя бы один символ…"!


`HOMEWORK CODE` 
Реализуйте два предиката, password-valid? и password-good?. Оба должны оценивать строку.

password-valid? должна возвращать #t, если строка состоит только из букв и цифр (char-alphabetic? и char-numeric?) и при этом имеет ненулевую длину.

password-good? должна возвращать #t, если строка содержит и буквы, и цифры, а длина строки не меньше восьми символов. И, разумеется, хороший пароль должен быть valid!
#lang racket

(provide
 password-valid?
 password-good?)

#| BEGIN (write your solution here) |#
#| Проверка символа - число-буква|#
(define (char-alphanumeric? c)
  (or (char-alphabetic? c)
      (char-numeric? c)))

(define (password-valid? password)
  (and
   (positive? (string-length password))
   (andmap char-alphanumeric? (string->list password))))

(define (password-good? password)
  (let ([chars (string->list password)])
    (and
     (password-valid? password)
     (<= 8 (length chars))
     (ormap char-alphabetic? chars)
     (ormap char-numeric? chars))))
#| END |#

L28 =====
`Оперирование строками. Получение отдельных символов и подстрок`
string-ref - получить элемент строки по его индексу

 (string-ref "apple" 3) ; #\l
 (string-ref "apple" 5)
 ; string-ref: index is out of range
 ;   index: 5
 ;   valid range: [0, 4]
 ;   string: "apple"
 ;   context...

изьятие подстроки  - через указание диапазона символов
; индексы:  01234
(substring "Apple" 2)   ; "ple"
(substring "Apple" 1 3) ; "pp"
(substring "Apple" 10 20)
; substring: starting index is out of range
;   starting index: 10
;   valid range: [0, 5]
;   string: "Apple"
;   context...

конкатенация строк - string-append
(string-append "Hello, " "World!") ; "Hello, World!"
(string-append "foo")              ; "foo"
(string-append "b" "a" "r")        ; "bar"

Модуль racket/string
string-join 

(string-join (list "a" "b" "c")) ; "a b c" - разделитель, это пробел

(define (greet names)
 (string-join
  names ", "
  #:before-first "Hello, "
  #:before-last " and "
  #:after-last "!"))

(greet (list "Bob"))               ; "Hello, Bob!"
(greet (list "Bob" "Tom"))         ; "Hello, Bob and Tom!"
(greet (list "Bob" "Tom" "Alice")) ; "Hello, Bob, Tom and Alice!"


`HOMEWORK CODE`
Реализуйте функцию scroll-left, которая "прокручивает" строку-аргумент влево так, что в строка-результате все символы, начиная со второго, оказываются сдвинуты на одну позицию влево, а первый символ оказывается в конце строки. В пустой строке прокручивать нечего, поэтому пустая строка должна оставаться неизменной.
#lang racket

(provide scroll-left)

#| BEGIN (write your solution here) |#
(define (scroll-left s)
  (if (zero? (string-length s)) s
      (string-append (substring s 1)
                     (substring s 0 1))))
#| END |#
