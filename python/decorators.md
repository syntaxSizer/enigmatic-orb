# Decorators in python

## In this session we will look at *decorators*, what they are and how to create and use them.
Decorators provide a simple syntax for [calling higher-order functions](https://en.wikipedia.org/wiki/Higher-order_function).

 *A decorator is a function that takes another function as an argument and extend its functionality without modifying it.*

### Functions
In order to understand *decorators* you must understand functions first.

> A function returns a value based on the given arguments.
```
def increment(number):
    return number + 1

increment(1)
2
increment(2)
3
```
Functions may also have side effect rather than just expect an input and output a value.
eg.
> the `print()` it returns `None` while having the side effect of displaying the param it was passed in the console


### First-class Objects
functions are first-class objects in python. This mean that a function can be passed around and used as an argument, just like any other data type.
Consider this:
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


### Inner functions
Functions defined whithin a function are called inner functions, they are just like local variables, defined only when the parent function get called and scoped to the parent function: meaning you can only call inner functions within its parent function scope

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
### Function returns function
In python you can define a function to return a function

for example:

```
def odd_or_even(number):
    def odd():
        return 'not divisable by 2'
    def even():
        return 'divisable by 2'
    if number % 2 == 0:
        return even
    else:
        return odd

a = odd_or_even(1)
b = odd_or_even(2)
a()

'not divisable by 2'

b()

'divisable by 2'
```
Note that `odd_or_even` returns a reference to a callable object (a function) which we store in varibles `a` & `b`.


---


### Basics of decorators
now that we have learnt about functions lets dive into decorators
starting with an example:

```
def a_decorator(func):
    def wrapper():
        print('doing something before the function call')
        func()
        print('doing something after the function call')
    return wrapper

def say_hi():
    print(f'Hi')

say_hi = a_decorator(say_hi)
```
in this example we just applied what we've learnt earlier

*the decoration happens at this point*
`say_hi = a_decorator(say_hi)`

The `say_hi` variable now points to `a_decorator()` inner function as explained abovd, the `a_decorator` returns a `function` which is the decorated version of our inital function that was passed as an argument

and of course you can do a lot more than just printing, remember a decorator is a wrapper that allow you to modify/extend a function

### Syntactic Sugar
There is a better syntax for decorating a function, its called the "pie" syntax.
its better because you don't have to pass around the function you want to
decorate eg. `say_hi` was typed 3 times in the above example.
it also provide a clearer way of identify decorated function

lets rewrite the above example using pie syntax

```
def a_decorator(func):
    def wrapper():
        print('doing something before the function call')
        func()
        print('doing something after the function call')
    return wrapper

@a_decorator
def say_hi():
    print(f'Hi')

```
its much cleaner and simpler
this syntax is exactly same as writing `say_hi = a_decorator(say_hi)`


### passing arguments to Decorators
lets say you have a function `print_name(name)` and you ant to decorate it

```
@a_decorator
print_name(name):
    print(name)

print_name('Doe John')
```
running this code will raises a 'TypeError' error

and thats because the inner function `wrapper` wasn't defined to expect any argument, but was given one 'Doe John'.
we could solve this be adding an argument to the inner function, but that mean
it won't work `say_hi` function that we implemented earlier

the perfect solution is to use `**arg` & `**kwargs` in the inner function. Then it will accept an arbitrary number of positional and keyword arguments
lets add `**args` & `**kwargs` to our decorator

```
def a_decorator(func):
    def wrapper(**args, **kwargs):
        print('doing something before the function call')
        func(**args, **kwargs)
        print('doing something after the function call')
    return wrapper

```
Now `a_decorator`'s inner function accepts anynumber of arguments and passes
them to the function it decorates

if you run `print_name` & `say_hi` with the new implementation of `a_decorator` it should
work perfectly

### Returning values from decorated function
if we dont explicitly return the decorated function value the decorator will
return `None` which is a default behavior for functions in python.

> example:
```
@a_decorator
def is_even(number):
    return  number % 2 == 0

print(is_even(2))
None

'''
    to fix this we need to make sure the wrapper function returns the decorated
    function value
'''

def a_decorator(func):
    def wrapper(**args, **kwargs):
        print('doing something before the function call')
        return func(**args, **kwargs)
    return wrapper

# now if we call `is_even` it should return the decorated function value which
is a boolean
print(is_even(2))
True
```
