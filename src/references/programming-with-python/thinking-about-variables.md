# Thinking About Variables

This lesson assumes the reader has already seen simple Python programs, including assignment statements using numeric, string, and list types. They should also have seen simple function definitions, such as a function to sum two values, and the use of the `pass` keyword.

## Variables Let Us Store Values

Let's consider the following code.

```python
a = 1  # a gets 1
b = a  # b gets a copy of a
a = a + 1  # add 1 to a and store the result back in a

print(f'a: {a}')
print(f'b: {b}')
```

Running this code produces the following output.

```text
a: 2
b: 1
```

We can explain the behavior of this code as follows:

1. `a = 1` stores the value `1` in the variable `a`.
2. `b = a` copies the value stored in `a` into the variable `b`.
3. `a = a + 1` takes the value stored in `a` (1), adds `1` to it (2) and stores that result back into `a`, replacing the previous value.
4. We print out the the results.

We can think of each of the variables as a box that get updated with each of the steps that we described.

![Fig 1. a gets 1. b gets a. a gets a + 1. Or does it?](figures/python-variables-number-a-b-1.png)

*Fig 1. a gets 1. b gets a. a gets a + 1. Or does it?*

So is that it? Is that all there is to working with variables?

## Variables With Complex Data

Let's consider this similar code.

```python
a = [1]  # a gets a list containing the number 1
b = a  # b gets a copy of a???
a[0] = a[0] + 1  # add 1 to the value in a[0] and store the result back in a[0]

print(f'a: {a}')
print(f'b: {b}')
```

We might guess this code will produce the following:

```text
a: [2]
b: [1]
```

But instead, we see this!

```text
a: [2]
b: [2]
```

Has something gone wrong? Is there an error in our code?

We might try to explain this code as before.

1. `a = [1]` creates a new list containing the value `1` in position 0, and stores the list in the variable `a`.
2. `b = a` copies the list stored in `a` into the variable `b`???
3. `a[0] = a[0] + 1` takes the value stored in position 0 of the list stored in `a` (1), adds `1` to it (2) and stores that result back into position 0 of the list stored in `a`, replacing the previous value.
4. We print out the the results.

Using our box model from before, this would resemble the following:

![Fig 2. a gets [1]. b gets a. a[0] gets a[0] + 1. Maybe?](figures/python-variables-list-a-b-1.png)

*Fig 2. a gets [1]. b gets a. a[0] gets a[0] + 1. Maybe?*

By that explanation, our first guess about what result we would see should have been correct. But instead, we see that the addition in step 2 seemed to affect both `a` and `b` at the same time.

## Checking Whether Variables Can Refer to the Same Value

How might it be that modifying a value in `a` also modifies a value in `b`. What if `a` and `b` actually refer to the same value? In this case, the same list. Is there a way we can confirm this?

### The `id` Function

Every value in Python is tracked by a unique identifier. We can ask Python what any value's identifier is by using the built in `id` function. We can think of this identifier as the _address_ of that value in the program.

>In the standard Python interpreter, these identifiers are more than just _like_ the address of the value, they actually _are_ the address of the value, stored in the memory of the computer!

The `id` function can be called on any value, such as `None`, `1`, `2`, `""`, `"dog"`, `"cat"`, `[]`, `{}`, really anything we can think of. Recalling our knowledge of how Python evaluates expressions, any expression that produces a value will also work, since the resulting value is what the `id` function will receive.

The full details of the [`id` function](https://docs.python.org/3/library/functions.html#id) are available in the official Python documentation.

Let's consider a few examples.

```python
# try out a few different values
print(f'id(None): {id(None)}')
print(f'id(1): {id(1)}')
print(f'id(2): {id(2)}')
print(f'id(1 + 1): {id(1 + 1)}')  # the same result as id(2)

# a function that adds two values
def addem(a, b):
    return a + b

print(f'id(addem(1, 1)): {id(addem(1, 1))}')  # the same result as id(2)

def does_nothing():
    pass

print(f'id(does_nothing()): {id(does_nothing())}')  # the same result as id(None)
```

The result will resemble the following:

```text
id(None): 4494621808
id(1): 4494764480
id(2): 4494764512
id(1 + 1): 4494764512
id(addem(1, 1)): 4494764512
id(does_nothing()): 4494621808
```

The exact identifier values may differ on your own computer, but notice that the values for `id(None)` and `id(does_nothing())` are the same, since both result in a value of `None`. The values for `id(2)`, `id(1 + 1)`, and `id(addem(1, 1)` are also all the same, since they all result in a value of `2`. `id(1)` is different from the others, since it is the only call using a value of `1`.

![Fig 3. `id` returns the _address_ of a value.](figures/python-variables-id-1.png)

*Fig 3. `id` returns the _address_ of a value.*

Notice in this example that none of the values we passed to `id` were stored in variables. The _addresses_ returned by `id` are the addresses of the values themselves. Each can be thought of as a label for that value, similar to our original idea about how values might be like boxes with a label on them.

### Back to `a` and `b`

Let's return to our original example and use our new understanding of `id` to see if we can get a better understanding of how it works.

```python
a = 1
b = a

print(f'a: {a}')
print(f'b: {b}')
print(f'id(a): {id(a)}')
print(f'id(b): {id(b)}')
print(f'id(1): {id(1)}')
print(f'id(2): {id(2)}')
print()

a = a + 1

print(f'a: {a}')
print(f'b: {b}')
print(f'id(a): {id(a)}')
print(f'id(b): {id(b)}')
print(f'id(1): {id(1)}')
print(f'id(2): {id(2)}')
```

This time, there are a lot more `print` statements, but we're still doing the same steps we did in the beginning. We're just checking on the values and their identifiers as we update the variables.

When we run this code, we will see something like this:

```text
a: 1
b: 1
id(a): 4439181760
id(b): 4439181760
id(1): 4439181760
id(2): 4439181792

a: 2
b: 1
id(a): 4439181792
id(b): 4439181760
id(1): 4439181760
id(2): 4439181792
```

Again, the exact identifier values may differ on your own computer. The values of `id(1)` and `id(2)` may even differ from the previous sample. In fact, they will probably be different every time the program runs! But we can observe in the first group of output, where `a` and `b` both have the value `1`, that `id(a)`, `id(b)`, and `id(1)` all have the same identifier. `id(2)` is different.

In the second group of output, where `a` is `2` and `b` is still `1`, we can observe that now `id(a)` and `id(2)` have the same value, which is the same as `id(2)` in the first group, while `id(b)` and `id(1)` still have the same value they had at the beginning.

This tells us something very important about how values and variables work together. Let's review our original explanation for `b = a`. We said that the variable `b` got a copy of the value in variable `a`. But from the `id` output, we see this is not true. They are the _same_ value. `a` and `b` do not have different copies of the value `1`. They refer to the same `1`. The same `1` as every other `1` in the program, since `id(1)` returns the same identifier.

Our explanation for `a = a + 1` is also slightly off. We see that the resulting identifier in `a` is the same value as `id(2)`. So `a` did _not_ directly contain the value `1`, which was updated to the value `2`. Instead, Python looked up the value that `a` referred to (1), added 1 to it (2), and stored a reference to the value `2` in `a`. We know this because now `id(a)` and `id(2)` are the same.

So now we might conclude that Python variables don't actually store values. Instead they store references to values that we can identify using the `id` function. We can think of the identifier as a unique address of the value inside the program. When we need to see what value there is in a variable, we check the address stored in the variable, and then go look at what is stored at that address.

## Identifiers and Lists

## Identifiers and Numbers

_why do some numbers get the same id?_

## References

[1] [id(object)](https://docs.python.org/3/library/functions.html#id). Python Built-in Functions. https://docs.python.org/3/library/functions.html#id.
[2] [Variables in Python](https://realpython.com/python-variables/). Real Python. https://realpython.com/python-variables.