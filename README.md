 <p align="center"><b>МОНУ НТУУ КПІ ім. Ігоря Сікорського ФПМ СПіСКС</b></p>
 <p align="center">
 <b>Звіт з лабораторної роботи 2</b><br/>
 "Рекурсія"<br/>
 дисципліни "Вступ до функціонального програмування"
 </p>
 <p align="right"><b>Студент(-ка)</b>: Приймак Юліанна КВ-23</p>
 <p align="right"><b>Рік</b>: 2025</p>
 
 ## Загальне завдання
 Реалізуйте дві рекурсивні функції, що виконують деякі дії з вхідним(и) списком(-ами), за
 можливості/необхідності використовуючи різні види рекурсії. Функції, які необхідно
 реалізувати, задаються варіантом (п. 2.1.1). Вимоги до функцій:
 1. Зміна списку згідно із завданням має відбуватись за рахунок конструювання нового
 списку, а не зміни наявного (вхідного).
 2. Не допускається використання функцій вищого порядку чи стандартних функцій
 для роботи зі списками, що не наведені в четвертому розділі навчального
 посібника.
 3. Реалізована функція не має бути функцією вищого порядку, тобто приймати функції
 в якості аргументів.
 4. Не допускається використання псевдофункцій (деструктивного підходу).
 5. Не допускається використання циклів.
 Кожна реалізована функція має бути протестована для різних тестових наборів. Тести
 мають бути оформленні у вигляді модульних тестів (див. п. 2.3).
 ## Варіант 6
 1. Написати функцію  merge-lists-spinning-pairs , яка групує відповідні елементи
 двох списків, почергово змінюючи їх взаємне розташування в групі:
 2. Написати предикат list-set-intersect-p , який визначає чи перетинаються дві
 множини, задані списками атомів, чи ні
 ## Лістинг функції merge-lists-spinning-pairs>
 ```
(defun merge-lists-spinning-pairs (list1 list2)
  (merge-lists-spinning-pairs-helper list1 list2 0))
```
 ### Тестові набори та утиліти
 ```
(defun merge-lists-spinning-pairs-helper (list1 list2 counter)
  (cond
    ((and (null list1) (null list2)) nil)
    ((null list1)
     (cons (list (car list2))
           (merge-lists-spinning-pairs-helper nil (cdr list2) (+ counter 1))))
    ((null list2)
     (cons (list (car list1))
           (merge-lists-spinning-pairs-helper (cdr list1) nil (+ counter 1))))
    ((evenp counter)
     (cons (list (car list1) (car list2))
           (merge-lists-spinning-pairs-helper (cdr list1) (cdr list2) (+ counter 1))))
    (t
     (cons (list (car list2) (car list1))
           (merge-lists-spinning-pairs-helper (cdr list1) (cdr list2) (+ counter 1))))))
(defun check-first-function (name input1 input2 expected)
    (format t "~:[FAILED~;PASSED~] ~a~%"
        (equal (merge-lists-spinning-pairs input1 input2) expected)
        name))
(defun test-first-function ()
    (check-first-function "[Test 1]" '(1 2 3 4 5) '(a b c d) '((1 a) (b 2) (3 c) (d 4) (5)))  
    (check-first-function "[Test 2]" '(1 2 3) '(a b c) '((1 a) (b 2) (3 c))) 
    (check-first-function "[Test 3]" nil nil nil))
 ```
 ### Тестування
```
CL-USER> (test-first-function)
PASSED [Test 1]
PASSED [Test 2]
PASSED [Test 3]
NIL
 ```
 ## Лістинг функції <list-set-intersect-p>
 ```lisp
 (defun list-set-intersect-p (set1 set2)
  (cond
    ((null set1) nil)
    ((member-p (car set1) set2) t)
    (t (list-set-intersect-p (cdr set1) set2))))
 ```
 ### Тестові набори та утиліти
 ```lisp
 (defun member-p (element list)
  (cond
    ((null list) nil)
    ((equal element (car list)) t)
    (t (member-p element (cdr list)))))
(defun check-second-function (name set1 set2 expected)
    (format t "~:[FAILED~;PASSED~] ~a~%"
        (equal (list-set-intersect-p set1 set2) expected)
        name))
(defun test-second-function ()
    (check-second-function "[Test 1]" '(1 2 3) '(4 5 6) nil)  
    (check-second-function "[Test 2]" '(1 2 3) '(3 4 5) t) 
    (check-second-function "[Test 3]" nil nil nil))
 ```
 ### Тестування
 ```lisp
 CL-USER> (test-second-function)
PASSED [Test 1]
PASSED [Test 2]
PASSED [Test 3]
NIL
 ```
