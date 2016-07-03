---
layout: page
title: Structure and Interpretation of Computer Programs Solutions
permalink: /structure-and-interpretation-of-computer-programs-solutions/
---

As I work my way through [*Structure and Interpretation of Computer Programs*](https://mitpress.mit.edu/sicp/full-text/book/book.html), I will post solutions on this page to the exercises found in that text.

--------------------------------------------

## Chapter 1

**Excercise 1.1.** Below is a sequence of expressions. What is the result printed by the interpreter in response to each expression? Assume that the sequence is to be evaluated in the order in which it is presented.

```
10
(+ 5 3 4)
(- 9 1)
(/ 6 2)
(+ (* 2 4) (- 4 6))
(define a 3)
(define b (+ a 1))
(+ a b (* a b))
(= a b)
(if (and (> b a) (< b (* a b)))
    b
    a)
(cond ((= a 4) 6)
      ((= b 4) (+ 6 7 a))
      (else 25))
(+ 2 (if (> b a) b a))
(* (cond ((> a b) a)
         ((< a b) b)
         (else -1))
   (+ a 1))
```

My solution:

```
10
12
8
3
6
;; evaluated, but no response
;; evaluated, but no response
19
#f
4
16
6
16
```

--------------------------------------

**Exercise 1.2.**  Translate the following expression into prefix form

![Taken kindly from https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-10.html](https://mitpress.mit.edu/sicp/full-text/book/ch1-Z-G-3.gif)

My solution:

```
(/ (+ 5 4
      (- 2 (- 3 (+ 6 0.8))))
   (* 3 (- 6 2) (- 2 7)))
```

----------------------------

**Exercise 1.3.**  Define a procedure that takes three numbers as arguments and returns the sum of the squares of the two larger numbers.

My solution:

```
(define (take n xs)
  (if (or (= (length xs) 0)
          (<= n 0))
      '()
      (cons (car xs) (take (- n 1) (cdr xs)))))

(define (two-largest-of-three a b c)
  (take 2 (sort > (list a b c))))

(define (sum-of-squares a b) (+ (* a a) (* b b)))

(define (exercise a b c)
  (apply sum-of-squares (two-largest-of-three a b c)))
```

----------------------------

**Exercise 1.4.**  Observe that our model of evaluation allows for combinations whose operators are compound expressions. Use this observation to describe the behavior of the following procedure:

```
(define (a-plus-abs-b a b)
  ((if (> b 0) + -) a b))
```

My solution:

The procedure `a-plus-abs-b a b` returns the sum of `a` and `b` if `b` is greater than 0. Otherwise, it returns `b` subtracted from `a`.

-------------------------------

**Exercise 1.5.**  Ben Bitdiddle has invented a test to determine whether the interpreter he is faced with is using applicative-order evaluation or normal-order evaluation. He defines the following two procedures:

```
(define (p) (p))

(define (test x y)
  (if (= x 0)
      0
      y))
```

Then he evaluates the expression

```
(test 0 (p))
```

What behavior will Ben observe with an interpreter that uses applicative-order evaluation? What behavior will he observe with an interpreter that uses normal-order evaluation? Explain your answer. (Assume that the evaluation rule for the special form if is the same whether the interpreter is using normal or applicative order: The predicate expression is evaluated first, and the result determines whether to evaluate the consequent or the alternative expression.)

My solution:

*What behavior will Ben observe with an interpreter that uses applicative-order evaluation?* Ben will observe infinite recursion (and probably stack overflow) since applicative-order requires that all arguments be evalutated before the procedure is applied to them. This means that `(p)` will be evaulated, which will infinitely recurse.

*What behavior will he observe with an interpreter that uses normal-order evaluation?* Ben will observe that the interpreter evaluates his expression `(test 0 (p))` to be 0.