Язык в курсе - LISP
`L1 введение, структура курса`
Базовая информация

`L2 (Listp, SCIM)` ================================================================================
На сайте нет информации чем пользоваться, как запускать (Диалект Racket)

Язык который взялся за решение проблем описания процессов и комплексных систем должен иметь как миминмум 3 вещи:
1) Иметь возможность создавать примитивные выражения (сложение\определение чисел)
2) Должны быть методы комбинирования этих простых выражений чтобы получить выражения более сложные
3) Должны быть способы создания абстракций (черных ящиков)

`Базовые операторы лиспа`
Lisp использует префиксную нотацию - в начале оператор, затем операнды
Скобки означают что это выражение

(+ 1 2) - выполнить сложение 1 и 2. (количество операндов любое в разумных пределах)
(- 2 3) - вычитание
(* 2 3) - умножение
(/ 2 4) - деление

выражения можно встраивать в другие выражения

(+ (* 10 2) (* 2 4)) тоже самое что ((10*2)+(2*4))
================================================================================
присвоить значение некоторой переменной\постоянной pi2
(define pi2 1.14159)
Переорпделить значение - просто повторить 

`определение функций`
(define curcumferense (* 2 pi raidus))

`compaund procedures` (составные процедуры) - процедура это процесс, поэтому нжуно вызывать через скобки
(define (square x) (* x x))
если обращаться без скобок - получим описание .

`Пример условия (cond) `
(define (abs x)
    (cond ((> x 0) x)
            ((= x 0) 0)
            (< x 0) (- x))))
Синтаксический сахар - если у условия только 2 варианта можно использовать  (if (УСЛОВИЕ) (TRUE) (FALSE))
 (define (abs x))
    (if (< x 0) (- x) x)

`Логические операции`
(define (>= x y)
    (not (< x y)))

`Рекурcия`

(define (sqrt-iter guess x)
    (if (good-enough? guess x) guess 
        (sqrt-iter (improve guess x) x)))

(define (improve quess x)
    (average guess (/ x guess)))

(define (average a b)
    (/(+ a b) 2))

(define (good-enough? guess x)
    (< (abs (- (square guess) x)) 0.01))

(define (sqrt x)
    (sqrt-iter 1.0 x))

`L2 Процедуры как асбстракции` ================================================================================
Инкапсуляция?

(define (sqrt x)
    (define (good-enough? guess x)
        (< (abs (- (square guess) x)) 0.01))
    (define (improve guess x)
        (average guess (/ x guess)))
    (define (average a b)
        (/ (+ a b) 2))
    (define (sqrt-iter guess x)
        (if (good-enough? guess x) guess (sqrt-iter (improve guess x) x)))
    (sqrt-iter 1.0 x)
    )

Упрощение (Исключаем лишнее объявление x т.к. он передается первым параметром):

(define (sqrt x)
    (define (good-enough? guess)
        (< (abs (- (square guess) x)) 0.01))
    (define (improve guess)
        (average guess (/ x guess)))
    (define (average a b)
        (/ (+ a b) 2))
    (define (sqrt-iter guess)
        (if (good-enough? guess) guess (sqrt-iter (improve guess))))
    (sqrt-iter 1.0)
    )

Дз - написать программу которая принимает три числа и возвращает сумму квадратов двух наибольших чисел

сумма квадратов
(define sum-of-squares(+ (square x1) (square y1)))
(define square (a)
    (* a a))

(define find-less(x y z)
   


If (a < b) swap (a, b);
If (a < c) swap (a, c);
if (b < c) swap (b, c);

(define (swap a1 b1 (define buf a1 (b1) b1 (buf)
    
    ))

   
