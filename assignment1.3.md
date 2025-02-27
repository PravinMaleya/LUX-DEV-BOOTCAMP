## In the data shared yesterday, sort the agent names in alphabetical order (A-Z)

Sorting Agent Names in Alphabetical Order (A-Z) is our end goal so these are the steps:

### 1. Import Required Libraries

We will begin by importing the necessary `pandas` library, which is used for data manipulation and analysis.

```python
import pandas as pd
```

### 2. Load the CSV File into a DataFrame

Next, we load the data from the CSV file into a pandas DataFrame. This allows us to work with the data in a tabular format.

```python
df = pd.read_csv(r"C:\Users\dell\Desktop\LUX DEV\PYTHON\CODE\train_data_cross-sell.csv", encoding='latin1')
```

- `pd.read_csv()` reads the CSV file and stores the data in the `df` DataFrame.
- The `encoding='latin1'` ensures the file is read correctly, especially if it contains non-ASCII characters. In this case it did have non-ASCII characters.

### 3. Sort the Agent Names Alphabetically

To sort the `Agent_name` column in ascending (A-Z) order, we use the `sort_values()` function:

```python
df_sorted = df.sort_values(by='Agent_name', ascending=True)
```

- `by='Agent_name'`: Specifies that we want to sort the DataFrame by the `Agent_name` column.
- `ascending=True`: Ensures the sorting is in ascending order (A-Z).

### 4. Print the Top 10 Rows of the Sorted Data

Finally, we print the top 20 rows of the sorted `Agent_name` column to check the result:

```python
print(df_sorted[['Agent_name']].head(10))
```

- `df_sorted[['Agent_name']]`: Selects only the `Agent_name` column from the sorted DataFrame.
- `.head()` displays the first 5 rows by default, but you can change the number inside the parentheses to view more or fewer rows, in this case the 10 rows.

---

Here is the full code with comments:

```python
# In the data shared yesterday, sort the agent names in alphabetical order (A-Z)

import pandas as pd

#Load the csv file into a dataframe
df = pd.read_csv(r"C:\Users\dell\Desktop\LUX DEV\PYTHON\CODE\train_data_cross-sell.csv", encoding='latin1')

#Sort the DataFrame by the 'column_name' column
df_sorted = df.sort_values(by='Agent_name', ascending=True)

#Print the top 20 rows of the sorted DataFrame 
print(df_sorted[['Agent_name']].head())
```

---

## Function to Categorize Agent Performance

### 1. Define the `categorize_performance` Function

The `categorize_performance` function takes each row of the DataFrame and assigns a performance category based on the agent's `performance_score`. The function compares the score against predefined quartiles to classify performance.

```python
def categorize_performance(row, quartiles):
    if row['performance_score'] >= quartiles[0.75]:
        return 'Excellent'
    elif row['performance_score'] >= quartiles[0.5]:
        return 'Good'
    elif row['performance_score'] >= quartiles[0.25]:
        return 'Average'
    else:
        return 'Poor'
```

- `row['performance_score']`: This refers to the performance score for each agent.
- `quartiles[0.75]`, `quartiles[0.5]`, `quartiles[0.25]`: These are the 75th, 50th, and 25th percentiles of the performance scores, used to categorize performance.
  - If the performance score is greater than or equal to the 75th percentile, the agent is classified as "Excellent".
  - If the score is greater than or equal to the 50th percentile but less than the 75th percentile, the agent is classified as "Good".
  - If the score is greater than or equal to the 25th percentile but less than the 50th percentile, the agent is classified as "Average".
  - If the score is less than the 25th percentile, the agent is classified as "Poor".

### 2. Define the `add_performance_category` Function

The `add_performance_category` function calculates the performance score for each agent, merges it back into the original DataFrame, and then applies the `categorize_performance` function to assign a category.

```python
def add_performance_category(df):
    # Step 1: Calculate performance scores (Premium and agent name)
    performance_scores = df.groupby('Agent_name')['Annual_Premium'].sum().reset_index(name='performance_score')
    
    # Step 2: Merge the performance scores back into the original DataFrame
    df = df.merge(performance_scores, on='Agent_name')
    
    # Step 3: Define performance categories based on quartiles
    performance_quartiles = df['performance_score'].quantile([0.25, 0.5, 0.75]).to_dict()
    
    # Step 4: Categorize performance
    df['performance_category'] = df.apply(categorize_performance, axis=1, quartiles=performance_quartiles)
    
    return df
```

#### Explanation of Each Step:

- **Step 1: Calculate performance scores**: 
  The performance score for each agent is calculated by summing the `Annual_Premium` values grouped by `Agent_name`. This is done using the `groupby()` function.

  ```python
  performance_scores = df.groupby('Agent_name')['Annual_Premium'].sum().reset_index(name='performance_score')
  ```

- **Step 2: Merge the performance scores**:
  The performance scores are merged back into the original DataFrame based on the `Agent_name` column, so each row now contains the total performance score for the corresponding agent.

  ```python
  df = df.merge(performance_scores, on='Agent_name')
  ```

- **Step 3: Define performance categories based on quartiles**:
  The quartiles (25th, 50th, and 75th percentiles) are calculated from the `performance_score` column to help categorize the agents.

  ```python
  performance_quartiles = df['performance_score'].quantile([0.25, 0.5, 0.75]).to_dict()
  ```

- **Step 4: Categorize performance**:
  The `categorize_performance` function is applied to each row to classify the performance based on the quartiles.

  ```python
  df['performance_category'] = df.apply(categorize_performance, axis=1, quartiles=performance_quartiles)
  ```

### 3. Return the Updated DataFrame

Finally, the updated DataFrame with the new `performance_category` column is returned.

```python
return df
```

---

---

## Python Operators and Their Uses

### 1. Arithmetic Operators
Arithmetic operators are used to perform common mathematical operations.

| Operator | Description   | Example      |
|----------|---------------|--------------|
| `+`      | Addition      | `a + b`      |
| `-`      | Subtraction   | `a - b`      |
| `*`      | Multiplication| `a * b`      |
| `/`      | Division (float) | `a / b`  |
| `//`     | Floor Division| `a // b`     |

### 2. Assignment Operators
Assignment operators are used to assign values to variables.

| Operator | Description       | Example       |
|----------|-------------------|---------------|
| `=`      | Assign            | `a = 10`      |
| `+=`     | Add and assign    | `a += 5`      |
| `-=`     | Subtract and assign | `a -= 3`    |
| `*=`     | Multiply and assign | `a *= 2`    |
| `/=`     | Divide and assign | `a /= 2`      |
| `//=`    | Floor divide and assign | `a //= 3`|

### 3. Comparison Operators
Comparison operators are used to compare two values.

| Operator | Description      | Example        |
|----------|------------------|----------------|
| `==`     | Equal to         | `a == b`       |
| `!=`     | Not equal to     | `a != b`       |
| `>`      | Greater than     | `a > b`        |
| `<`      | Less than        | `a < b`        |
| `>=`     | Greater than or equal to | `a >= b` |
| `<=`     | Less than or equal to | `a <= b`    |

### 4. Logical Operators
Logical operators are used to combine conditional statements.

| Operator | Description     | Example            |
|----------|-----------------|--------------------|
| `and`    | Returns True if both statements are true | `a > 5 and b < 10` |
| `or`     | Returns True if at least one statement is true | `a > 5 or b < 10`  |
| `not`    | Returns True if the statement is false | `not(a > 5)`         |

### 5. Identity Operators
Identity operators are used to compare objects, not if they are equal, but if they are actually the same object, with the same memory location.

| Operator | Description     | Example        |
|----------|-----------------|----------------|
| `is`     | Returns True if both variables are the same object | `a is b`    |
| `is not` | Returns True if both variables are not the same object | `a is not b` |

### 6. Membership Operators
Membership operators are used to test if a sequence is present in an object.

| Operator | Description     | Example        |
|----------|-----------------|----------------|
| `in`     | Returns True if a sequence is present in an object | `'a' in 'apple'`   |
| `not in` | Returns True if a sequence is not present in an object | `'b' not in 'apple'`|

### 7. Bitwise Operators
Bitwise operators are used to compare (binary) numbers.

| Operator | Description     | Example       |
|----------|-----------------|---------------|
| `&`      | AND             | `a & b`       |
| `|`      | OR              | `a | b`       |
| `^`      | XOR             | `a ^ b`       |
| `~`      | NOT             | `~a`          |
| `<<`     | Left shift      | `a << 1`      |
| `>>`     | Right shift     | `a >> 1`      |

---

### Construct a class (Car) in Python with attributes (make, model, and year of manufacture). The second function should be called describe. The examples to use are Honda Civic 2021 and Toyota Camry 2020.

## Creating a Car Class in Python

In this example, we create a `Car` class that has three attributes: `make`, `model`, and `year`. Additionally, we define a `describe` function that returns a string describing the car with its year, make, and model.

Here is the full code:

```python
class Car:
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year

    def describe(self):
        return f"{self.year} {self.make} {self.model}"

# Example usage:
car1 = Car("Honda", "Civic", 2021)
car2 = Car("Toyota", "Camry", 2020)

print(car1.describe())  # Output: 2021 Honda Civic
print(car2.describe())  # Output: 2020 Toyota Camry
```

### 1. Define the Car Class

First, we define the `Car` class with an `__init__` method that initializes the attributes of the class.

```python
class Car:
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
```

- `__init__(self, make, model, year)`: This is the constructor method that is called when a new object of the class is created. It initializes the `make`, `model`, and `year` attributes with the values passed when creating the object.
  - `self.make`: The make (brand) of the car, such as "Honda" or "Toyota".
  - `self.model`: The model of the car, such as "Civic" or "Camry".
  - `self.year`: The year of manufacture of the car.

### 2. Define the `describe` Method

The `describe` method is used to return a string describing the car in the format "Year Make Model".

```python
def describe(self):
    return f"{self.year} {self.make} {self.model}"
```

- The method `describe()` takes `self` as a parameter (the instance of the class), and it returns a string that combines the `year`, `make`, and `model` attributes.

### 3. Example Usage of the Car Class

Now, let's create instances of the `Car` class for two different cars and use the `describe` method to print their details.

```python
car1 = Car("Honda", "Civic", 2021)
car2 = Car("Toyota", "Camry", 2020)

print(car1.describe())  # Output: 2021 Honda Civic
print(car2.describe())  # Output: 2020 Toyota Camry
```

- `car1 = Car("Honda", "Civic", 2021)`: Creates a `Car` object for a 2021 Honda Civic.
- `car2 = Car("Toyota", "Camry", 2020)`: Creates a `Car` object for a 2020 Toyota Camry.
- `car1.describe()`: Calls the `describe()` method on `car1` to print the description: `2021 Honda Civic`.
- `car2.describe()`: Calls the `describe()` method on `car2` to print the description: `2020 Toyota Camry`.

---
