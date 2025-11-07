# Python Behave BDD Practical for Beginners

## Introduction to BDD and Behave

**Behavior-Driven Development (BDD)** is a software development approach that allows technical and non-technical team members to collaborate using simple, human-readable language to describe how software should behave[1][4].

**Behave** is a BDD framework for Python that allows you to write tests in plain English using a language called Gherkin, which uses keywords like `Given`, `When`, `Then`, `And`[1][4].

---

## Prerequisites

Before starting this practical, ensure you have:
- Python 3.7 or higher installed
- Visual Studio Code installed
- Basic understanding of Python syntax
- pip (Python package manager) installed

---

## Part 1: Setting Up Your Environment

### Step 1: Install Behave

Open your terminal or command prompt and run:

```bash
pip install behave
```

To verify the installation:

```bash
behave --version
```

You should see the version number displayed (e.g., `behave 1.2.6`)[37].

### Step 2: Install VS Code Extension

1. Open Visual Studio Code
2. Go to Extensions (Ctrl+Shift+X or Cmd+Shift+X on Mac)
3. Search for "Behave VSC" by jimasp
4. Click Install[8]

This extension provides:
- Syntax highlighting for Gherkin feature files
- Test explorer integration
- Step navigation between feature files and step definitions
- Ability to run tests directly from feature files[8]

---

## Part 2: Project Structure

### Step 1: Create Project Folder

Create a new folder for your project called `calculator_bdd`:

```
calculator_bdd/

mkdir calculator_bdd
```

### Step 2: Open in VS Code

1. Open Visual Studio Code
2. Go to File > Open Folder
3. Select your `calculator_bdd` folder

### Step 3: Create Required Folders and Files

Your project structure should look like this[21][28]:

```
calculator_bdd/
├── features/
│   ├── calculator.feature
│   └── steps/
│       └── calculator_steps.py
```

**Explanation:**
- **features/**: Main directory containing all feature files
- **calculator.feature**: Feature file written in Gherkin language
- **steps/**: Directory containing Python step definitions
- **calculator_steps.py**: Python code that implements the steps

---

## Part 3: Writing Your First Feature File

### Step 1: Create the Feature File

In the `features` folder, create a file called `calculator.feature` and add the following content[21][42]:

```gherkin
Feature: Simple Calculator
    As a user
    I want to use a simple calculator
    So that I can perform basic arithmetic operations

    Scenario: Add two numbers
        Given I have a calculator
        When I add 5 and 3
        Then the result should be 8

    Scenario: Subtract two numbers
        Given I have a calculator
        When I subtract 3 from 10
        Then the result should be 7

    Scenario: Multiply two numbers
        Given I have a calculator
        When I multiply 4 by 6
        Then the result should be 24
```

**Understanding Gherkin Keywords:**
- **Feature**: Describes the high-level functionality being tested[3][9]
- **Scenario**: Describes a specific test case[3][9]
- **Given**: Sets up the initial context or preconditions[3][15]
- **When**: Describes the action being performed[3][15]
- **Then**: States the expected outcome[3][15]
- **And/But**: Adds additional steps to make scenarios more readable[3][15]

---

## Part 4: Creating the Calculator Class

### Step 1: Create Calculator Implementation

In your project root (not in features folder), create a file called `calculator.py`:

```python
class Calculator:
    """A simple calculator class for basic arithmetic operations"""
    
    def __init__(self):
        self.result = 0
    
    def add(self, a, b):
        """Add two numbers and return the result"""
        self.result = a + b
        return self.result
    
    def subtract(self, a, b):
        """Subtract b from a and return the result"""
        self.result = a - b
        return self.result
    
    def multiply(self, a, b):
        """Multiply two numbers and return the result"""
        self.result = a * b
        return self.result
    
    def divide(self, a, b):
        """Divide a by b and return the result"""
        if b == 0:
            raise ValueError("Cannot divide by zero")
        self.result = a / b
        return self.result
```

---

## Part 5: Implementing Step Definitions

### Step 1: Create Step Definitions File

In the `features/steps` folder, create `calculator_steps.py` with the following content[21][22]:

```python
from behave import given, when, then
from calculator import Calculator

@given('I have a calculator')
def step_given_calculator(context):
    """Initialize the calculator and store it in context"""
    context.calculator = Calculator()

@when('I add {num1:d} and {num2:d}')
def step_when_add(context, num1, num2):
    """Add two numbers using the calculator"""
    context.result = context.calculator.add(num1, num2)

@when('I subtract {num2:d} from {num1:d}')
def step_when_subtract(context, num1, num2):
    """Subtract num2 from num1 using the calculator"""
    context.result = context.calculator.subtract(num1, num2)

@when('I multiply {num1:d} by {num2:d}')
def step_when_multiply(context, num1, num2):
    """Multiply two numbers using the calculator"""
    context.result = context.calculator.multiply(num1, num2)

@then('the result should be {expected:d}')
def step_then_result(context, expected):
    """Verify the result matches the expected value"""
    assert context.result == expected, f"Expected {expected}, but got {context.result}"
```

**Understanding the Code:**

1. **Imports**: We import the decorators (`given`, `when`, `then`) from behave and our Calculator class[21]

2. **Context Object**: The `context` variable is a special object that Behave uses to share data between steps. It's automatically managed by Behave[21][13]

3. **Step Decorators**: Each function is decorated with `@given`, `@when`, or `@then` matching the keywords in the feature file[21]

4. **Step Parameters**: The `{num1:d}` syntax extracts integers from the step text. The `:d` means "parse as integer"[21]

---

## Part 6: Running Your Tests

### Method 1: Using Terminal

Open the terminal in VS Code (Ctrl+` or View > Terminal) and run:

```bash
behave
```

You should see output like this[21]:

```
Feature: Simple Calculator

  Scenario: Add two numbers
    Given I have a calculator
    When I add 5 and 3
    Then the result should be 8

  Scenario: Subtract two numbers
    Given I have a calculator
    When I subtract 3 from 10
    Then the result should be 7

  Scenario: Multiply two numbers
    Given I have a calculator
    When I multiply 4 by 6
    Then the result should be 24

3 scenarios passed, 0 failed, 0 skipped
9 steps passed, 0 failed, 0 skipped, 0 undefined
```

### Method 2: Using VS Code Debugger

To debug your tests in VS Code, create a launch configuration[2][5]:

1. Click on the Debug icon in the left sidebar (or press Ctrl+Shift+D)
2. Click "create a launch.json file"
3. Add this configuration:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Behave Current File",
            "type": "python",
            "request": "launch",
            "module": "behave",
            "console": "integratedTerminal",
            "args": [
                "${file}"
            ]
        }
    ]
}
```

Now you can:
- Open any `.feature` file
- Press F5 to run/debug that specific feature
- Set breakpoints in your step definition files[2][5]

### Method 3: Using Behave VSC Extension

With the Behave VSC extension installed, you can:
- Click the play button next to scenarios in feature files
- View test results in the Test Explorer panel
- Navigate between steps and their definitions[8]

---

## Part 7: Adding More Scenarios with Data Tables

### Step 1: Enhance Feature File with Scenario Outline

Add this to your `calculator.feature` file[21][42]:

```gherkin
    Scenario Outline: Add multiple number combinations
        Given I have a calculator
        When I add <num1> and <num2>
        Then the result should be <expected>

        Examples:
            | num1 | num2 | expected |
            | 1    | 1    | 2        |
            | 5    | 5    | 10       |
            | 100  | 200  | 300      |
            | 0    | 0    | 0        |
```

**Scenario Outline** allows you to run the same scenario multiple times with different data. The `Examples` table provides the test data[42].

### Step 2: Run Tests Again

```bash
behave
```

The scenario outline will run once for each row in the Examples table, giving you 4 additional test cases automatically!

---

## Part 8: Understanding Test Results

When you run `behave`, the output shows:

- **Green**: Passed steps
- **Red**: Failed steps  
- **Yellow**: Skipped steps
- **Cyan**: Undefined steps (steps without implementation)[21]

If a test fails, you'll see:

```
Failing scenarios:
  features/calculator.feature:10  Add two numbers

0 features passed, 1 failed, 0 skipped
1 scenario passed, 1 failed, 0 skipped
```

---

## Part 9: Common Behave Commands

Run all tests:
```bash
behave
```

Run a specific feature file:
```bash
behave features/calculator.feature
```

Run with verbose output:
```bash
behave -v
```

Run and stop at first failure:
```bash
behave --stop
```

Show all step definitions:
```bash
behave --dry-run
```

---

## Part 10: Exercise - Try It Yourself!

Now it's your turn! Add a division feature:

1. Add this scenario to `calculator.feature`:

```gherkin
    Scenario: Divide two numbers
        Given I have a calculator
        When I divide 20 by 5
        Then the result should be 4
```

2. Add the step definition to `calculator_steps.py`:

```python
@when('I divide {num1:d} by {num2:d}')
def step_when_divide(context, num1, num2):
    context.result = context.calculator.divide(num1, num2)
```

3. Run `behave` and verify it passes!

---

## Troubleshooting

**Problem: "ImportError: No module named behave"**
- Solution: Make sure you installed behave with `pip install behave`

**Problem: "No steps directory"**
- Solution: Ensure your folder structure matches the required format with `features/steps/` directory[21]

**Problem: Steps showing as "undefined"**
- Solution: Check that your step definitions exactly match the text in your feature file[21]

**Problem: "ModuleNotFoundError: No module named 'calculator'"**
- Solution: Make sure `calculator.py` is in your project root directory, not inside the `features` folder

---

## Summary

In this practical, you learned:

1. ✅ What BDD and Behave are
2. ✅ How to install and configure Behave in VS Code
3. ✅ The required project structure for Behave
4. ✅ How to write feature files using Gherkin syntax
5. ✅ How to implement step definitions in Python
6. ✅ How to run tests using multiple methods
7. ✅ How to use Scenario Outlines for data-driven testing
8. ✅ How to debug Behave tests in VS Code

---

## Next Steps

To continue learning Behave:

1. Explore the `environment.py` file for setup and teardown hooks
2. Learn about tags to organize and selectively run tests
3. Try integrating Behave with Selenium for web testing
4. Explore different step matchers (parse, re, cfparse)
5. Read the official documentation at https://behave.readthedocs.io/

Happy Testing! 🎉
