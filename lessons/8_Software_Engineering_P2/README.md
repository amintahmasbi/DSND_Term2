# Testing And Data Science

- **TEST DRIVEN DEVELOPMENT:** a development process where you write tests for tasks before you even write the code to implement those tasks.
- Tests can check for all the different scenarios and edge cases you can think of, before even starting to write your function. 
- When refactoring or adding to your code, tests help you rest assured that the rest of your code didn't break while you were making those changes. 

## Unit Test

**Unit Test:** a type of test that covers a “unit” of code, usually a single function, independently from the rest of the program.

**Unit Test Advantages and Disadvantages**

The advantage of unit tests is that they are isolated from the rest of your program, and thus, no dependencies are involved. They don't require access to databases, APIs, or other external sources of information. However, passing unit tests is not always enough to prove that our program is working successfully. To show that all the parts of our program work with each other properly, communicating and transferring data between them correctly, we use integration tests. 

```python
import unittest

from your_code_file import YourObj

class TestYourCode(unittest.TestCase):
    def test_your_func(self):
        obj = YourObj()
        obj.step(2,'S')
        self.assertEqual(obj.value, 4) # assert obj.value == 4 
    
    def test_bad_input(self):
        obj = YourObj()
        with self.assertRaises(TypeError):
            obj.step(2) # Test for Wrong number of arguments
```

**Other Testing Tools:**

- Test doubles and **Mock**
- addCleanup: nicer than tearDown (if you have startUp for you test object)
- **doctest**: only for testing docs
- nose, **pytest**: better test runners
- **Hypothesis**: a library which lets you write tests that are parametrized by a source of examples.
- tox: a tool for automating test environment management and testing against multiple interpreter configurations
- ddt: data-driven test
- **coverage**: check the lines being tested
  - __pytest-cov__: produces coverage reports. and works along with pytest
- Selenium: in-browser testing
- Jenkins, Travis: run tests all the time (Continuous Testing)

## pytest:

`pytest` will run all files of the form test_*.py or *_test.py in the current directory and its subdirectories.

```shell
pip install -U pytest
```

Enter `pytest` into your terminal in the directory of your test file and it will detect these tests for you!

recommended test layout:

```shell
setup.py
src/
    mypkg/
        __init__.py
        app.py
        view.py
tests/
    __init__.py
    foo/
        __init__.py
        test_view.py
    bar/
        __init__.py
        test_view.py
```

A simple example:

```python
# content of test_sample.py
def func(x):
    return x + 1

def test_answer():
    assert func(3) == 5
```

## Doctest:

The [`doctest`](https://docs.python.org/3/library/doctest.html#module-doctest) module searches for pieces of text that look like interactive Python sessions in docstrings, and then executes those sessions to verify that they work exactly as shown. They are usually less detailed and don’t catch special cases or obscure regression bugs. They are useful as an expressive documentation of the main use cases of a module and its components.

```python
def square(x):
    """Return the square of x.

    >>> square(2)
    4
    >>> square(-2)
    4
    """

    return x * x

if __name__ == '__main__':
    import doctest
    doctest.testmod()
```

## Tips:

- A testing unit should focus on one tiny bit of functionality and prove it correct.
- Each test unit must be fully independent. Each test must be able to run alone, and also within the test suite, regardless of the order that they are called. The implication of this rule is that each test must be loaded with a fresh dataset and may have to do some cleanup afterwards. This is usually handled by `setUp()` and `tearDown()` methods.
- Try hard to make tests that run fast.
- Learn your tools and learn how to run a single test or a test case. Then, when developing a function inside a module, run this function’s tests frequently, ideally automatically when you save the code.
- Always run the full test suite before a coding session, and run it again after. This will give you more confidence that you did not break anything in the rest of the code.
- It is a good idea to implement a hook that runs all tests before pushing code to a shared repository.
- The first step when you are debugging your code is to write a new test pinpointing the bug.
- Use long and descriptive names for testing functions. 

---

# Logging

Logging is the process of recording messages to describe events that have occurred while running your software.

- Be professional and clear
- Be concise and use normal capitalization
- Choose the appropriate level for logging
  - DEBUG - level you would use for anything that happens in the program.
  - ERROR - level to record any error that occurs.
  - INFO - level to record all actions that are user-driven or system specific, such as regularly scheduled operations
- Provide any useful information

```python
import logging

# logging.basicConfig(**kwargs)
logging.debug('This is a debug message')
logging.info('This is an info message')
logging.warning('This is a warning message')
logging.error('This is an error message')
logging.critical('This is a critical message')
```

---

# Code Review

- Is the code clean and modular?
- Is the code efficient?
- Is documentation effective?
-  Is the code well tested?
- Is the logging effective?

Tips:

- Use a code linter (pylint)
- Explain issues and make suggestions
- Keep your comments objective
- Provide code examples

More tips:

- [Code Review](https://github.com/lyst/MakingLyst/tree/master/code-reviews)
- [Code Review Best Practices](https://www.kevinlondon.com/2015/05/05/code-review-best-practices.html)