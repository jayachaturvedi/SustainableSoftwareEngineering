**Estimated time:** 30–40 minutes  
**Level:** Beginner-friendly  
**Files:** `pythagoras.py` and `test_pythagoras.py`

## Learning objectives

By the end of this practical, you should be able to:

- Explain what a unit test is.
- Write tests using Python's built-in `unittest` framework.
- Test expected results and invalid inputs.
- Recognise passing and failing tests.
- Explain how testing helps make research software more sustainable.

## Before you start

You will need Python 3. No additional Python packages are required because `unittest` is included in the Python standard library.

Create a project folder and open it in VS Code or GitHub Codespaces.

## 1. Starting code

Create a file called `pythagoras.py` containing:

```python
import math

def calculate_hypotenuse(side_a, side_b):
    return math.sqrt(side_a**2 + side_b**2)
```

The function calculates the hypotenuse of a right-angled triangle from the lengths of its two shorter sides.

## 2. Task 1 – Write tests for the function

Create another file called `test_pythagoras.py`.

Start with this template:

```python
import unittest
from pythagoras import calculate_hypotenuse


class TestPythagoras(unittest.TestCase):

    def test_basic_calculation(self):
        # Test that 3 and 4 produce a hypotenuse of 5
        pass

    def test_larger_values(self):
        # Test another known calculation
        pass

    def test_decimal_values(self):
        # Test that the function works with decimal numbers
        pass


if __name__ == "__main__":
    unittest.main()
```

**Your tasks:**

1. Replace each `pass` with an appropriate assertion.
2. Run the tests.
3. Check that all tests pass.

Useful assertion:

```python
self.assertAlmostEqual(actual, expected)
```

`assertAlmostEqual` is useful for floating-point calculations, where tiny rounding differences can occur.

Run your tests from the project folder:

```bash
python -m unittest -v
```

If your system uses `python3` instead of `python`, run:

```bash
python3 -m unittest -v
```

## 3. Task 2 – Make a test fail

Now deliberately introduce an error into one of your tests. For example, change the expected answer for the 3–4–5 triangle so that it is incorrect.

Run the tests again.

**Think about these questions:**

- Why did the test fail?
- What does `FAIL` in the test output mean?
- What information does the error message provide?
- Does a failing test necessarily mean the program is broken, or could the test itself be wrong?

After discussing the failure, correct the test so that it passes again.

## 4. Task 3 – Test invalid inputs

At the moment, the function does not explicitly check that side lengths are positive.

Improve `calculate_hypotenuse` so that it raises a `ValueError` if either side is zero or negative.

Then write tests for:

- A side length of zero.
- A negative side length.
- Two valid positive side lengths.

You can use this pattern to test that a function raises an expected exception:

```python
with self.assertRaises(ValueError):
    calculate_hypotenuse(0, 4)
```

## 5. Task 4 – Run the complete test suite

Once you have added input validation and written your tests, run:

```bash
python -m unittest -v
```

Check that all your tests pass.

The `-v` option displays the name of each test, making it easier to see what has been tested.

## 6. Extension challenge

If you finish early, try one or more of these challenges:

- Test what happens when a string is supplied instead of a number.
- Test whether the function works with very large numbers.
- Add a function to calculate the area of a right-angled triangle.
- Write unit tests for the new function.
- Deliberately introduce a bug and see whether your tests detect it.

## 7. Reflection: testing and sustainable research software

Discuss the following questions:

1. Why is it useful to test software after making changes?
2. How could tests help another researcher understand how your code is supposed to behave?
3. How might unit tests help prevent errors in a research analysis or simulation?
4. What are the limitations of unit tests? Do passing tests prove that software is completely correct?

Consider how testing contributes to:

- **Reliability:** tests can reveal errors.
- **Maintainability:** tests help check that existing behaviour still works after changes.
- **Extensibility:** tests make it safer to add new features.
- **Understandability:** tests provide examples of expected behaviour.
- **Reproducibility:** tests help check that software behaves consistently.

**Key takeaway:** Unit tests help us check that software behaves as expected, both when it is first written and when it is changed in the future.
'''

solutions = r'''# Instructor Solutions: Unit Testing a Pythagoras Calculator

**For instructor use — reveal after students have attempted the practical.**

This document contains example solutions, expected behaviour, and teaching notes. Students may produce different but equally valid tests.

## 1. Starting implementation

The initial `pythagoras.py` file:

```python
import math

def calculate_hypotenuse(side_a, side_b):
    return math.sqrt(side_a**2 + side_b**2)
```

## 2. Task 1 – Example basic tests

A possible `test_pythagoras.py` solution is:

```python
import unittest
from pythagoras import calculate_hypotenuse


class TestPythagoras(unittest.TestCase):

    def test_basic_calculation(self):
        self.assertAlmostEqual(calculate_hypotenuse(3, 4), 5)

    def test_larger_values(self):
        self.assertAlmostEqual(calculate_hypotenuse(5, 12), 13)

    def test_decimal_values(self):
        self.assertAlmostEqual(calculate_hypotenuse(1.5, 2), 2.5)


if __name__ == "__main__":
    unittest.main()
```

Run from the project folder:

```bash
python -m unittest -v
```

Expected result: three tests pass. The exact run time and formatting may vary.

### Teaching notes

- `unittest.TestCase` provides assertion methods and test-running support.
- Test method names begin with `test_` so `unittest` can discover them.
- `assertAlmostEqual` is preferable to exact equality for many floating-point calculations.
- Importing `calculate_hypotenuse` lets the test file test the function without asking for interactive input.

## 3. Task 2 – Example of a deliberately failing test

For example:

```python
def test_basic_calculation(self):
    self.assertEqual(calculate_hypotenuse(3, 4), 6)
```

The function returns `5.0`, not `6`, so the test fails.

The test output will indicate a failure and show the difference between the actual and expected values. The exact formatting can vary by Python version.

**Teaching point:** a failing test does not automatically mean the program is wrong. The test may contain an incorrect expected value. In this example, the expected value should be `5`.

A useful distinction:

- `FAIL` means the test ran, but an assertion was not satisfied.
- `ERROR` means an unexpected exception interrupted the test.

## 4. Task 3 – Add input validation

One possible improved `pythagoras.py` implementation:

```python
import math

def calculate_hypotenuse(side_a, side_b):
    if side_a <= 0 or side_b <= 0:
        raise ValueError("Side lengths must be positive")

    return math.sqrt(side_a**2 + side_b**2)
```

This rejects zero and negative side lengths by raising `ValueError`.

Example tests:

```python
import unittest
from pythagoras import calculate_hypotenuse


class TestPythagoras(unittest.TestCase):

    def test_basic_calculation(self):
        self.assertAlmostEqual(calculate_hypotenuse(3, 4), 5)

    def test_larger_values(self):
        self.assertAlmostEqual(calculate_hypotenuse(5, 12), 13)

    def test_decimal_values(self):
        self.assertAlmostEqual(calculate_hypotenuse(1.5, 2), 2.5)

    def test_zero_side_length(self):
        with self.assertRaises(ValueError):
            calculate_hypotenuse(0, 4)

    def test_negative_side_length(self):
        with self.assertRaises(ValueError):
            calculate_hypotenuse(-3, 4)


if __name__ == "__main__":
    unittest.main()
```

Expected result: all five tests pass.

### Explain `assertRaises`

This code:

```python
with self.assertRaises(ValueError):
    calculate_hypotenuse(0, 4)
```

means: run the function and check that it raises a `ValueError`. If the expected exception is raised, the test passes. If no exception is raised, the test fails. If a different exception is raised, the test errors.

## 5. Task 4 – Run the test suite

Run:

```bash
python -m unittest -v
```

The `-v` flag means verbose. It prints the individual test names and their results.

Students can also run the test file directly:

```bash
python test_pythagoras.py
```

Both approaches should discover and run the tests in this example.

## 6. Extension challenge – Example solutions

### Test a string input

The current function does not explicitly validate types. Passing a string such as `"three"` will cause a `TypeError` during the calculation.

A test for that current behaviour could be:

```python
def test_string_input_raises_type_error(self):
    with self.assertRaises(TypeError):
        calculate_hypotenuse("three", 4)
```

This tests Python's current behaviour, rather than a custom validation message.

If you want the function to validate input types explicitly, you could extend it:

```python
import math

def calculate_hypotenuse(side_a, side_b):
    if (
        not isinstance(side_a, (int, float))
        or isinstance(side_a, bool)
        or not isinstance(side_b, (int, float))
        or isinstance(side_b, bool)
    ):
        raise TypeError("Side lengths must be numbers")

    if side_a <= 0 or side_b <= 0:
        raise ValueError("Side lengths must be positive")

    return math.sqrt(side_a**2 + side_b**2)
```

The explicit Boolean checks are included because Python's `bool` type is a subclass of `int`.

A corresponding test:

```python
def test_string_input_raises_type_error(self):
    with self.assertRaises(TypeError):
        calculate_hypotenuse("three", 4)

def test_boolean_input_raises_type_error(self):
    with self.assertRaises(TypeError):
        calculate_hypotenuse(True, 4)
```

### Add a triangle-area function

Add this to `pythagoras.py`:

```python
def calculate_area(side_a, side_b):
    if side_a <= 0 or side_b <= 0:
        raise ValueError("Side lengths must be positive")

    return (side_a * side_b) / 2
```

Example tests:

```python
def test_area_calculation(self):
    self.assertAlmostEqual(calculate_area(3, 4), 6)

def test_area_rejects_zero_side(self):
    with self.assertRaises(ValueError):
        calculate_area(0, 4)
```

Remember to import the new function in the test file:

```python
from pythagoras import calculate_hypotenuse, calculate_area
```

## 7. Discussion: testing and sustainable research software

Suggested discussion points:

| Testing benefit | Sustainability connection |
|---|---|
| Detects errors | Improves reliability |
| Checks existing functionality after changes | Supports maintainability |
| Documents expected behaviour | Helps other researchers understand the software |
| Allows changes to be checked automatically | Supports extensibility |
| Makes behaviour more dependable | Supports reproducibility |

### Important qualification

Passing unit tests does not prove that software is completely correct. Tests only check the cases that have been written. A program can pass every current test and still contain bugs in untested situations.

Good testing therefore involves choosing meaningful cases, including typical inputs, boundary cases, and invalid inputs. For research software, tests of individual functions may also need to be complemented by integration tests and checks against known scientific results.

## 8. Suggested teaching sequence

| Activity | Suggested time |
|---|---:|
| Introduce unit testing and starting code | 5 minutes |
| Students write basic tests | 10 minutes |
| Demonstrate a deliberately failing test | 5 minutes |
| Add input validation and exception tests | 10 minutes |
| Discuss sustainability and extension | 5–10 minutes |

**Teaching suggestion:** Let students attempt each task before revealing its solution. In particular, show the failing test output before explaining why it failed. This helps distinguish a test failure from a program error.
'''


