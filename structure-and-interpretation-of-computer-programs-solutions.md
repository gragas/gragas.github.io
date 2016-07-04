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

---------------------------------

**Exercise 1.6.**  Alyssa P. Hacker doesn't see why if needs to be provided as a special form. ``Why can't I just define it as an ordinary procedure in terms of cond?'' she asks. Alyssa's friend Eva Lu Ator claims this can indeed be done, and she defines a new version of if:

```
(define (new-if predicate then-clause else-clause)
  (cond (predicate then-clause)
        (else else-clause)))
```

Eva demonstrates the program for Alyssa:

```
(new-if (= 2 3) 0 5)
5

(new-if (= 1 1) 0 5)
0
```

Delighted, Alyssa uses new-if to rewrite the square-root program:

```
(define (sqrt-iter guess x)
  (new-if (good-enough? guess x)
          guess
          (sqrt-iter (improve guess x)
                     x)))
```

What happens when Alyssa attempts to use this to compute square roots? Explain.

My solution:

The procedure will infinitely recurse (if applicative-order argument evaluation is used). Infinite recursion will happen under applicative-order because all arguments, including both `then-clause` and `else-clause` will be evaluated before the procedure is applied. In this case, `else-clause` is a recursive call to the function itself. Every single call to the function will result in another call to itself, regardless of the provided `predicate`.

----------------

**Exercise 1.7.**  The `good-enough?` test used in computing square roots will not be very effective for finding the square roots of very small numbers. Also, in real computers, arithmetic operations are almost always performed with limited precision. This makes our test inadequate for very large numbers. Explain these statements, with examples showing how the test fails for small and large numbers. An alternative strategy for implementing `good-enough?` is to watch how `guess` changes from one iteration to the next and to stop when the change is a very small fraction of the `guess`. Design a square-root procedure that uses this kind of end test. Does this work better for small and large numbers?

My solution:

*The* `good-enough?` *test used in computing square roots will not be very effective for finding the square roots of very small numbers.* Suppose that we try to compute the square root of a number that is less than our provided `good-enough?` threshold. There is very low probability that the result will be reasonably accurate. To demonstrate this, consider trying to determine the square root of 25, where our initial `guess` is 1 and our threshold is 250. Our initial guess already passes our `good-enough?` test, and so the procedure returns 1, which is quite inaccurate. More formally, if our threshold *t* is some multiple of the number *x* that we're trying to find the square root of, then our final guess can be up to `(* b 100)` percent greater or less than the actual square root.

Here's an example of this implementation failing miserably:

```
> (sqrt 1e-6)
0.031260655525445276
> (square 0.03126065552544276)
9.772285838803947e-4
```

Our final guess is more than two orders of magnitude away from the actual solution, 0.001!

*Also, in real computers, arithmetic operations are almost always performed with limited precision. This makes our test inadequate for very large numbers.* The specific problem with approximating the square roots of very large numbers using this implementation is our threshold, 0.001, is very small relative to the size of the square root that is being approximated. This means that many more steps than necessary are required to determine the square root of large numbers. For example, `(sqrt 3.3242e16)` takes a very long time to finish on my reltaively powerful computer.

*An alternative strategy for implementing good-enough? is to watch how guess changes from one iteration to the next and to stop when the change is a very small fraction of the guess. Design a square-root procedure that uses this kind of end test.* I've redefined the `good-enough?` procedure as follows:

```
(define (good-enough? guess x)
    (<= (/ (abs (- (square guess) x)) x) 0.01))
```

*Does this work better for small and large numbers?*

With this new procedure, both of the bad examples I listed above (`(sqrt 1e-6)` and `(sqrt 3.3242e16)`) are more accurate and terminate within the blink of an eye.

-------------------

**Exercise 1.8.**  Newton's method for cube roots is based on the fact that if y is an approximation to the cube root of x, then a better approximation is given by the value

![Taken kindly from https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-10.html](https://mitpress.mit.edu/sicp/full-text/book/ch1-Z-G-5.gif)

Use this formula to implement a cube-root procedure analogous to the square-root procedure.

My solution:

```
(define (cube-root x)
  (cube-root-iter 1.0 x))

(define (cube-root-iter guess x)
    (if (good-enough? guess x)
        guess
        (cube-root-iter (improve guess x) x)))

(define (improve guess x)
  (/ (+ (/ x (* guess guess))
        (* 2 guess))
     3))

(define (good-enough? guess x)
    (<= (/ (abs (- (cube guess) x)) x) 0.01))

(define (cube x) (* x x x))
```

How it stands:

```
> (cube-root 1.0)
1.0
> (cube-root 8.0)
2.003137499141287
> (cube-root 27.0)
3.001274406506175
> (cube-root 123123123123.0)
4989.154214060669
> (cube 4989.154214060669)
1.2418832924483836e11
```

-------------------------------------

**Exercise 1.9.**  Each of the following two procedures defines a method for adding two positive integers in terms of the procedures `inc`, which increments its argument by 1, and `dec`, which decrements its argument by 1.

```
(define (+ a b)
  (if (= a 0)
      b
      (inc (+ (dec a) b))))

(define (+ a b)
  (if (= a 0)
      b
      (+ (dec a) (inc b))))
```

Using the substitution model, illustrate the process generated by each procedure in evaluating `(+ 4 5)`. Are these processes iterative or recursive?

My solution:

```
(+ 2 3)
(inc (+ (dec 2) 3))
(inc (inc (+ (dec 1) 3)))
(inc (inc 3))
5
```

This process is recursive.

```
(+ 2 3)
(+ (dec 2) (inc 3))
(+ (dec 1) (inc 4))
5
```

This process is iterative.

-------------------------------

**Exercise 1.10.**  The following procedure computes a mathematical function called Ackermann's function.

```
(define (A x y)
  (cond ((= y 0) 0)
        ((= x 0) (* 2 y))
        ((= y 1) 2)
        (else (A (- x 1)
                 (A x (- y 1))))))
```

What are the values of the following expressions?

```
(A 1 10)

(A 2 4)

(A 3 3)
```

Consider the following procedures, where `A` is the procedure defined above:

```
(define (f n) (A 0 n))

(define (g n) (A 1 n))

(define (h n) (A 2 n))

(define (k n) (* 5 n n))
```

Give concise mathematical definitions for the functions computed by the procedures `f`, `g`, and `h` for positive integer values of *n*. For example, `(k n)` computes *5n^2*.

My solution:

*What are the values of the following expressions?*

`(A 1 10)` => `1024`

`(A 2 4)` => `65536`

`(A 3 3)` => `65536`

*Give concise mathematical definitions for the functions computed by the procedures* `f`, `g`, *and* `h` *for positive integer values of* n*. For example,* `(k n)` *computes* 5n^2*.*

*2n* is a concise mathematical definition for `(f n)`.

*2^n* is a concise mathematical definition for `(g n)`.

*2^2^2...* (n times) is a concise mathematical definition for `(g n)`.

-----------------------