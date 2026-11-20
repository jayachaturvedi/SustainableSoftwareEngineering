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
Or feel free to use the code you previously created.

The function here calculates the hypotenuse of a right-angled triangle from the lengths of its two shorter sides.

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

