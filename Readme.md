[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=22131780)

# Unit Testing in Python with `unittest`

This README introduces the basics of **unit testing** in Python using the built-in **`unittest`** framework and then explains step by step how to solve the 4 exercises from this lab.

---

## 1. What are unit tests?

A **unit test** checks that a *small piece of code* (usually a function or method) behaves as expected for specific inputs.

Why we use unit tests:

- ❗ Catch bugs early  
- 🔁 Prevent regressions when changing code  
- 📚 Serve as executable documentation  
- 🤖 Allow automated checking of behavior

In Python, the most common built-in framework for unit testing is the **`unittest`** module from the standard library.  

---

## 2. The `unittest` framework – core ideas

### 2.1. Test files and imports

A typical **test file** structure looks like this:

```python
import unittest          # import the framework
from my_module import f  # import the function you want to test


class TestMyFunction(unittest.TestCase):
    def test_something(self):
        # assertions go here
        self.assertEqual(f(2), 4)


if __name__ == "__main__":
    unittest.main()
````

Key points:

* Every test file should `import unittest`.
* You then import the functions you want to test from your project modules.
* Tests are grouped in classes that **inherit from `unittest.TestCase`**.

---

### 2.2. Test classes and test methods

A **test class** is a class that:

* Inherits from `unittest.TestCase`.
* Contains **methods whose names start with `test_`**.

Example:

```python
class TestMath(unittest.TestCase):
    def test_add_two_positive_numbers(self):
        self.assertEqual(add(2, 3), 5)
```

Anything that does **not** start with `test_` is not run as a test method by `unittest`’s automatic discovery. 


### 2.3. Assertions

**Assertions** are methods that check if something is true.

Common ones:

* `self.assertEqual(a, b)` – checks `a == b`
* `self.assertNotEqual(a, b)` – checks `a != b`
* `self.assertTrue(x)` – checks that `bool(x) is True`
* `self.assertFalse(x)` – checks that `bool(x) is False`
* `self.assertAlmostEqual(a, b)` – checks that `a` and `b` are almost equal (useful for floats)
* `self.assertRaises(SomeException, callable, *args, **kwargs)` – checks that a callable raises a given exception
* `with self.assertRaises(SomeException):` – context manager style for checking exceptions 

Example:

```python
self.assertEqual(add(2, 3), 5)
self.assertTrue(is_even(4))
self.assertFalse(is_even(5))
```

---

### 2.4. Testing exceptions with `assertRaises`

When you expect a function to raise an exception (e.g., for invalid input), use `assertRaises`.

Two common patterns:

**a) Functional form:**

```python
self.assertRaises(ValueError, safe_divide, 5, 0)
```

**b) Context manager form:**

```python
with self.assertRaises(ValueError):
    safe_divide(5, 0)
```

Both are valid; the context manager is often more readable, especially when code is longer. 

---

### 2.5. Running tests

To run tests from a test file like `test_math_utils.py`:

```bash
python test_math_utils.py
```

or using the `unittest` discovery mechanism from the project root:

```bash
python -m unittest
```

Discovery will automatically find files named like `test_*.py` and methods starting with `test_`. 

---

## 3. Learn more about `unittest`

For a broader tutorial and more examples, you can read:

**GeeksforGeeks – Python Unittest Tutorial**
[https://www.geeksforgeeks.org/python/unit-testing-python-unittest/](https://www.geeksforgeeks.org/python/unit-testing-python-unittest/) ([GeeksforGeeks][4])

You can also consult the **official Python documentation** for `unittest`: ([(https://docs.python.org/3/library/unittest.html)](https://docs.python.org/3/library/unittest.html))

---

## 4. Step-by-step: solving the 4 exercises

In this lab, all functions are already implemented in their respective modules:

* `math_utils.py` – `add(a, b)`
* `number_utils.py` – `is_even(n)`
* `calc_utils.py` – `safe_divide(a, b)`
* `grading.py` – `grade_student(score)`

Your job is to write **test files** that use `unittest` to verify that these functions behave correctly.

Below are the conceptual steps for each exercise.

---

### 4.1. Exercise 1 – Testing `add(a, b)`

**Files:**

* Function: `math_utils.py`
* Tests: `test_math_utils.py`

**Goal:**
Test that `add(a, b)` correctly adds two numbers in different scenarios.

**Steps:**

1. Create a file named `test_math_utils.py`.

2. Import `unittest` and `add`:

   ```python
   import unittest
   from math_utils import add
   ```

3. Create a test class:

   ```python
   class TestAddFunction(unittest.TestCase):
       ...
   ```

4. Inside the class, write three test methods:

   * `test_add_positive_numbers` – test `add` with two positive numbers (e.g. `2` and `3`).
   * `test_add_negative_numbers` – test `add` with two negative numbers (e.g. `-4` and `-6`).
   * `test_add_mixed_numbers` – test `add` with one positive and one negative number (e.g. `-2` and `5`).

5. In each test method, use `self.assertEqual(...)` to check the expected result.

6. At the bottom of the file, add:

   ```python
   if __name__ == "__main__":
       unittest.main()
   ```

---

### 4.2. Exercise 2 – Testing `is_even(n)`

**Files:**

* Function: `number_utils.py`
* Tests: `test_number_utils.py`

**Goal:**
Test that `is_even(n)` correctly returns `True` for even numbers and `False` for odd numbers.

**Steps:**

1. Create `test_number_utils.py`.

2. Import `unittest` and `is_even`:

   ```python
   import unittest
   from number_utils import is_even
   ```

3. Create a test class:

   ```python
   class TestIsEven(unittest.TestCase):
       ...
   ```

4. Add test methods for the following cases:

   * `is_even(2)` → `True`
   * `is_even(0)` → `True`
   * `is_even(7)` → `False`
   * `is_even(-4)` → `True`

5. Use:

   * `self.assertTrue(is_even(...))` for values you expect to be even.
   * `self.assertFalse(is_even(...))` for values you expect to be odd.

6. Add the `unittest.main()` block at the end.

---

### 4.3. Exercise 3 – Testing `safe_divide(a, b)` and exceptions

**Files:**

* Function: `calc_utils.py`
* Tests: `test_calc_utils.py`

**Function behavior:**

* Returns `a / b` as a **float**.
* Raises `ValueError("Division by zero is not allowed")` when `b == 0`.

**Goal:**
Test both **normal behavior** and **error behavior (exception)**.

**Steps:**

1. Create `test_calc_utils.py`.

2. Import `unittest` and `safe_divide`:

   ```python
   import unittest
   from calc_utils import safe_divide
   ```

3. Create a test class:

   ```python
   class TestSafeDivide(unittest.TestCase):
       ...
   ```

4. Add tests for **normal division**:

   * A test method (e.g. `test_division_integers`) for `safe_divide(10, 2)`:

     * Expect `5.0` – use `self.assertEqual(...)`.
   * A test method (e.g. `test_division_floats`) for `safe_divide(0.5, 0.1)`:

     * Expect something close to `5.0` – use `self.assertAlmostEqual(...)`.

5. Add a test for **division by zero**:

   * A method like `test_division_by_zero_raises_value_error`.
   * Inside it, use either:

     ```python
     self.assertRaises(ValueError, safe_divide, 5, 0)
     ```

     or

     ```python
     with self.assertRaises(ValueError):
         safe_divide(5, 0)
     ```

6. Optionally, you can also check the **error message** by capturing the exception and inspecting its `.args[0]` or `str(e)`.

7. Add the `unittest.main()` block at the end.

---

### 4.4. Exercise 4 – Testing `grade_student(score)`

**Files:**

* Function: `grading.py`
* Tests: `test_grading.py`

**Function behavior recap:**

* Returns a letter grade based on `score`:

  * `90–100` → `"A"`
  * `80–89`  → `"B"`
  * `70–79`  → `"C"`
  * `60–69`  → `"D"`
  * `0–59`   → `"F"`

* Raises `ValueError` if `score` is outside the range `0–100`.

**Goal:**
Test both **internal ranges** and **boundary values**, plus **invalid input**.

**Steps:**

1. Create `test_grading.py`.

2. Import `unittest` and `grade_student`:

   ```python
   import unittest
   from grading import grade_student
   ```

3. Create a test class:

   ```python
   class TestGradeStudent(unittest.TestCase):
       ...
   ```

4. Add tests for **typical values**:

   * `95` → `"A"`
   * `85` → `"B"`
   * `75` → `"C"`
   * `65` → `"D"`
   * `30` → `"F"`

   For each, use `self.assertEqual(grade_student(score), "ExpectedLetter")`.

5. Add tests for **boundary values**:

   * `90` → `"A"`, `89` → `"B"`
   * `80` → `"B"`, `79` → `"C"`
   * `70` → `"C"`, `69` → `"D"`
   * `60` → `"D"`, `59` → `"F"`

   This ensures that exact limits are handled correctly.

6. Add tests for **invalid values**:

   * For example, `-1` and `101` should both raise `ValueError`.

   Use `assertRaises`:

   ```python
   with self.assertRaises(ValueError):
       grade_student(-1)

   with self.assertRaises(ValueError):
       grade_student(101)
   ```

7. Optionally, you can group multiple `(score, expected_grade)` pairs in one test using a loop or `subTest`, but this is not required for the basic exercise.

8. Add the `unittest.main()` block at the end.

---