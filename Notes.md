# Pandas - Notes

Pandas is a powerful Python library used for data manipulation and analysis. It provides two main data structures:

1. **Series**: One-dimensional labeled array.
2. **DataFrame**: Two-dimensional labeled array.

---

## Getting Started

### Installation
```bash
pip install pandas
```

### Importing Pandas
```python
import pandas as pd
```

---

## Pandas Series
A Pandas Series is like a column in a table. It is one-dimensional.

### Creating a Series
```python
# From a list
s = pd.Series([10, 20, 30, 40])

# From a dictionary
data = {'a': 10, 'b': 20, 'c': 30}
s = pd.Series(data)
```

### Accessing Data in Series
```python
# By index
s[0]

# By label (if created with a dictionary)
s['a']
```

---

## Pandas DataFrame
A DataFrame is a 2D labeled data structure, like a table with rows and columns.

### Creating a DataFrame
```python
# From a dictionary
data = {
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'Salary': [50000, 60000, 70000]
}
df = pd.DataFrame(data)

# From a CSV file
df = pd.read_csv('filename.csv')

# From a Excel file
# Using excel need to install xlrd (pip install xlrd)
df = pd.read_excel('filename.xlsx')
```

### Viewing Data
```python
# Display the first few rows
df.head()

# Display the last few rows
df.tail()

# Summary of the DataFrame
df.info()

df['gender'].value_counts(dropna=False)
```

### Accessing Data in DataFrame
```python
# Access a column
df['Age']

# Access multiple columns
df[['Name', 'Salary']]

# Access by index (iloc => integer location)
df.iloc[0]  # First row
df.iloc[0, 4] # First row with 4th column
df.iloc[[2, 5], [1, 2]] # 2, 5 row with 1, 2 column
df.iloc[1:3]  # Second and third rows

# Access by label (loc => locate) 
df.loc[[3, 4], ['C', 'D']]  # Particular column and row
df.loc[[1,2], :] # All columns with 1, 2 rows
df.loc[:, ['C', 'D']] # All rows with C, D columns

# Access by condition
df[df['Age'] > 30] 
df[(df['Age'] <  18) & (df['Age'] > 12)]
```

---

## Data Manipulation

### Updating a value
```python
df.loc[0, 'Name'] = 'Genius'
```

### Adding a Column
```python
df['Bonus'] = df['Salary'] * 0.1
```

### Dropping a Column
```python
df = df.drop('Bonus', axis=1)  # axis=1 for columns, axis=0 for rows
```

### Renaming Columns
```python
df.rename(columns={'Name': 'Employee Name'}, inplace=True) # inplace change in original
```

### Reset Index
```python
df.reset_index() # But added a new column (index)
df.reset_index(drop=True, inplace=True) # No new column created

# Transpose (Columns 🔄️ rows)
df.T
```

### Handling Missing Data
```python
# Check for missing values
print(df.isnull())

# Drop rows with missing values
df = df.dropna()

# Drop rows that have all/any missing values
df.dropna(how='all') # default 'any'

# Fill missing values
df['Age'].fillna(30, inplace=True)

# Fill missing for object
data.fillna(method = 'ffill') # fill the value of above

# Fill missing for object
data.fillna(method = 'bfill') # fill the value of bottom

# Replace values
data['salary'].mean() # we can fill mean to numeric missing values
data['salary'] = data['salary'].replace(np.nan, 24400)

# Fill value to whole column
df.loc[:, ['Experienced']] = 'Yes'

```

### Handling Duplicates
```python
# Returns boolean value
print(df.duplicated())

# For particular column
print(df['id'].duplicated())
print(df['id'].duplicated().sum()) # total duplicate values

# Delete duplicate rows
df.drop_duplicates('id')

# Fill missing values
df['Age'].fillna(30, inplace=True)

# Fill value to whole column
df.loc[:, ['Experienced']] = 'Yes'

```

### Column Transformation
```python
# Merge two columns and transform cases (upper, lower, capitalize)
data['Full Name'] = data['First Name'].str.capitalize() +" "+ data['Last Name'].str.capitalize()

# Create new column with calculations
data['Bonus'] = (data['Salary']/100) * 20

# Map value to change
def  extract(value):
    return value[0:3]

a['Short_Months'] = a['Months'].map(extract)

```

---

## Useful Functions

### Statistics
```python
print(df.describe())  # Summary statistics
print(df['Age'].mean())  # Mean of a column
print(df['Salary'].sum())  # Sum of a column
```

### Sorting
```python
# Sort by a column
df = df.sort_values('Age')

# Sort by Index (axis for columns, ascending for rows)
df.sort_index(axis=0, ascending=False)
```

### Grouping
```python
grouped = df.groupby('Department').mean()
print(grouped)

groupByDept = df.groupby('Department').agg({'Id': 'count'})

groupByDeptGender = df.groupby(['Department', 'Gender']).agg('Id': 'count')

# Use (mean, max)
groupByAnnualSalaryMean = data.groupby('Country').agg({'Annual Salary': 'mean', 'Age': 'min'})

```

### Merging
```python
pd.merge(df1, df2, on = 'Emp Id')

# Merge left (All left rows)
pd.merge(left = df1, right = df2, on = 'Emp Id', how = 'left')

```

### Concatenate
```python
# Append to end
pd.concat([df1, df2])

```

### Compare
```python
df1.compare(df2, align_axis=0)
```

---

## Exporting Data
```python
# To CSV
df.to_csv('output.csv', index=False)

# To Excel
df.to_excel('output.xlsx', index=False)
```

---

## Conclusion
Pandas is a must-know library for data manipulation and analysis in Python. Start practicing with real datasets to explore its full potential!
