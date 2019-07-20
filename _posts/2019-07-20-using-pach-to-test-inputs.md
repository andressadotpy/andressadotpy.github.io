---
layout: post
title: Testing inputs in Python3
subtitle: Mock inputs using the @patch decorator
tags: [python, unittest, unittest.mock, patch]
---
# Testing inputs in Python 3
### Mock inputs using the @patch decorator

>unittest.mock is a library for testing in Python. It allows you to replace parts of your system under test with mock objects and make assertions about how they have been used.

This definition was taken from the [unittest.mock documentation](https://docs.python.org/3/library/unittest.mock.html). This post is about the patch function and how to unit test inputs inside a function using the **@patch decorator**.

The **patch function** temporarily replaces the **target** object with a different object during the test. The **@patch decorator** accepts a big amount of arguments, but here I will focus on **side_effect** and **return_value**. As I will talk only about mocking inputs, our target is the builtin function input, and the target for the patch decorator is **'builtins.input'**.

The **side_effect** argument can accept a function to be called when the mock is called, an iterable or an Exception. Passing in an iterable is very useful to mock multiple inputs inside the testing function, because it will **yield** the **next value** everytime it's called:

```python
import unittest
from unittest.mock import patch

class Test(unittest.TestCase):

    @patch('builtins.input', side_effect=['First', 'Second', 'Third'])
    def test_using_side_effect(self, mock_input):
        calling_1 = mock_input()
        calling_2 = mock_input()
        calling_3 = mock_input()
        self.assertTrue(calling_1 == 'First' and calling_2 == 'Second' and
                        calling_3 == 'Third')
```
The **return_value** configure the value returned when the mock is called. It will always return the same value when the mock is called.

```python

    @patch('builtins.input', return_value=10)
    def test_using_return_value(self, mock_input):
        calling_1 = mock_input()
        calling_2 = mock_input()
        self.assertTrue(calling_1 == 10 and calling_2 == 10)
```

### Simple Example

As a simple example of how to use **return_value** and **side_effect**, I will create a function _sum()_ inside a ```sum.py``` module. The function asks for space separated integers and returns their sum.

sum.py:

```python
def sum():
    """Asks for 5 space separated integers.
    Returns the sum of these integers.
    """
    L = input("Type 5 integers separated by space: ")
    L = L.split(' ')
    result = 0
    for num in L:
        result += int(num)
    return result
```

To test this function, inside ```test_list_sum.py```:

```python
import unittest
from unittest.mock import patch

from sum import sum

class TestListSum(unittest.TestCase):

    string_of_ints = '1 2 3 4 5'

    @patch('builtins.input', return_value=string_of_ints)
    def test_sum_string_of_ints(self, mock_input):
        result = sum()
        self.assertEqual(result, 15)

```

At the terminal, using ```python3 -m unittest```, the test runs without any error, but if I change ```self.assertEqual(result, 15)``` to ```self.assertEqual(result, 16)```, for example, an ```AssertionError: 15 != 16``` fails the test.

Now, I will change the function sum to asks for the number of integers the user will type and another input with those integers separated by space:

sum.py:    

```python
def sum():
    """Asks for the number of integers the user will type and
    the space separated integers."""
    n = input("Type the number of integers: ")
    L = input("Type the integers separated by space: ")
    L = L.split(' ')
    result = 0
    for num in range(n):
        result += int(L[num])
    return result
```  

test_list_sum.py:    

```python
import unittest
from unittest.mock import patch

from sum import sum

class TestListSum(unittest.TestCase):

    string_of_ints = '1 2 3 4 5'

    string_of_ints_2 = '1 1 1 1 1 1 1 1 1 1'

    @patch('builtins.input', side_effect=[5, string_of_ints])
    def test_sum_string_of_ints(self, mock_inputs):
        result = sum()
        self.assertEqual(result, 15)

    @patch('builtins.input', side_effect=[10, string_of_ints_2])
    def test_sum_string_of_ints_2self, mock_inputs):
        result = sum()
        self.assertEqual(result, 10)
```  
The **first time the input function is called inside sum()** is to ask for the **number of integers the user will type**. So, at the first test, **5 is passed in** as this number and **the string_of_inputs is called at the next call for input inside sum()**, the call for space separated integers. The same happens in the second test, but with different values.  

Those are silly functions, but it's easy to understand what is happening and when to use **side_effect** or **return_value**. I use side_effect when the function I'm testing has more than one call to input(). The return_value is good to functions that call input() once.

# Resources

[1] - [stackoverflow - Using unittest.mock to patch input() in Python 3](https://stackoverflow.com/questions/18161330/using-unittest-mock-to-patch-input-in-python-3)  
[2] - [unittest.mock - return_value](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock.return_value)  
[3] - [unittest.mock - side_effect](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock.side_effect)  
[4] - [Testing with mock - Michael Foord](https://pyvideo.org/pycon-us-2011/pycon-2011--testing-with-mock.html)  
[5] - [Lisa Roach - Demystifying the Patch Function - PyCon 2018](https://www.youtube.com/watch?v=ww1UsGZV8fQ)  
