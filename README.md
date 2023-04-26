# Composing and Currying Functions

A curried function to handle multiplication can be defined as:
```
(define mult
  (lambda (a)
    (lambda (b)
      (* a b))))
```

Exercise 1: Define a function `curry3` that takes a three-argument function and returns a curried version of that function. Thus `((((curry3 f) x) y) z)` returns the result of `(f x y z)`.

Sometimes when you define a curried function, the internal function needs to be recursive. This raises the question: how do you make a recursive function call in a function defined only by lambda? The following code snippet is a sample. This is a curried function that multiples the first parameter by the factorial of the second.
```
(define multfact
  (lambda (a)
    (letrec ((factorial
              (lambda (b)
                (if (= b 1) a
                    (* b (factorial (- b 1)))))))
      factorial)))
```
`letrec` defines the local variable factorial to be bound to a function. That function is then returned as the result of the `letrec` function call.

Exercise 2: Using the above code for inspiration if appropriate, define a function called `curried-map` that works like Scheme's built-in `map` function except for the fact that it is a curried version of `map`. For example, if you had a function called `add-one` that takes one parameter and adds 1 to it, a call to `curried-map` might look like:
```
((curried-map add-one) '(1 2 3)) => (2 3 4)
```

Exercise 3: Define a curried function called `curried-map-first-two` that works like `curried-map` except that the function argument is always a function of two arguments (like the operator `+`). Use the first and second elements of the list argument as the first pair of arguments to the function argument, then the second and third elements, then the third and the fourth, and so on. If there are fewer than two elements in the list, the empty list should be returned. Here is an example:
```
((curried-map-first-two +) '(2 3 4 5 7)) ---> (5 7 9 12)
```

Source: https://www.cs.carleton.edu/faculty/dmusican/cs217w07/scheme_currying.html

Follow this [tutorial](https://www.cs.oberlin.edu/~rms/classes/cs275/labs/lab5/lab52.html) and write teh following procedures:

Exercise 4: Use the unrestricted-lambda to write a procedure `mag` which computes the magnitude of a vector of any length. (For the purposes of this exercise, we are using the term "vector" to mean "list of numbers". So `mag` is expecting a bunch of numbers, as in `(mag 3 4)` or `(mag 2 4 6 8 5)`.) Use helper procedures as necessary.

Exercise 5: Define a procedure `multi-compose` which takes an arbitrary number of procedures each taking and returning an arbitrary number of arguments and composes them all. Test it by defining procedures of multiple arguments which return appropriate numbers of arguments as lists. For example if we have procedures f, g, h, and k we should be able to compose them all with `(multi-compose f g h k)`. Notice that the functions you pass to `multi-compose` should return lists NOT atoms since we need to be sufficiently general to deal with multi-value functions. Here is some sample output:
```
(define fac
  (lambda (n)
    (cond
     ((zero? n) '(1))
     (else (list (apply * (cons n (fac (- n 1)))))))))
(define plus
  (lambda args
    (list (apply + args))))
> ((multi-compose fac plus) 1 2 3)
(720)
> ((multi-compose) 1 2 3)
(1 2 3)
> ((multi-compose fac) 5)
(120)
> ((multi-compose plus) 1 2 3 4 5)
(15)
> ((multi-compose plus plus fac fac plus) 2 2)
(620448401733239439360000)
>
```
