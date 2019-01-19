# Docorators in python
# Decorators in python

##### In this session we will look at *decorators*, what they are and how to
create and use them.
Decorators provide a simple syntax for [calling higher-order functions](https://en.wikipedia.org/wiki/Higher-order_function).

 *A decorator is a function that takes another function as an argument and extend its functionality without modifying it.*

### Functions
In order to understand *decorators* you must understand functions first.

> A function returns a value based on the given arguments.
eg.
```
def increment(number):
    return number + 1

increment(1)
2
increment(2)
3
```
Functions may also have side effect rather than just expect an input and output a value.
eg. the `print()` it returns `None` while having the side effect of displaying the param it was passed in the console


### First-class Objects
functions are first-class objects in python. This mean that a function can be passed around and used as an argument, just like any other data type
consider this:
```
def say_hi(name):
    return f'Hi {name}'

def say_something(name):
    return f'{name}, python is awesome'

def tell_em(func):
    return func('Qusai')
```
`say_hi` and `say_something` are reqular functions expecting a 'name', `tell_em` function however expect a `function` as its argument.
you can try it and pass `say_hi` or `say_something`.


### inner functions
Functions defined whithin a function are called inner functions, they are just like local variables, defined only when the parent function get called and scoped to the parent function: meaning you can only call inner functions within its parent function scope
eg.

```
def parent():
    print('i\'m the parent')
    def first():
        print('first child')
    def second():
        print('second child')
    first()
    second()
```
