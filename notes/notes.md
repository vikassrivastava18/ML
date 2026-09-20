### Import pandas
` import pandas as pd `

### Importing data
```
# Load the dataset and save it to the df variable
# https://drive.google.com/file/d/1iUCkOLJogog3nu687ChBJiio5_Z0i80A/view?usp=sharing
df = pd.read_csv('data/world_happiness.csv')
 ```

### View the dataframe

``` 
df.info()
df.head()
display(df)
df.tail(20)
```

### Index and column names

```
df.index
df.columns
```

### Rename columns

```
columns_to_rename = {i: "_".join(i.split(" ")).lower() for i in df.columns}

df = df.rename(columns=columns_to_rename)
```

### Data types

```
df.dtypes

float_columns = [i for i in df.columns if i not in ["country_name", "year"]]
# Change the type of all the float columns
df = df.astype({i: float for i in float_columns})
```

### Selecting columns

```
# Select the life_ladder column and store it in x
x = df.life_ladder

print(f"type(x): \n {type(x)}\n")
print(f"x: \n{x}")
```

### Selecting rows

```
df[2:5]
```

### Iterating over rows
```
index, row = next(df.iterrows())
row
```

### Boolean indexing

```
df[df["year"] == 2022]
df[df["life_ladder"] > 5]

## reset index
new_df = df[df["year"] == 2022]
new_df = new_df.reset_index(drop=True)
new_df
```

### Summary statistics
` df.describe() `

### Plot
``` 
df.plot()
df.plot(kind='scatter', x='log_gdp_per_capita', y='life_ladder')

# Create a dictionary to map the country names to colors
cmap = {
    'Brazil': 'Green',
    'Slovenia': 'Orange',
    'India': 'purple'
}

df.plot(
    kind='scatter',
    x='log_gdp_per_capita',
    y='life_ladder',
    c=[cmap.get(c, 'yellow') for c in df.country_name], # Set the colors
    s=2 # Set the size of the points
)

df.hist("life_ladder")

# Seaborn 
import seaborn as sns

sns.pairplot(df)

```

### Operations on columns
```
df["this_column_makes_no_sense"] = df["year"] + df["life_ladder"]

# Rescaling
df["life_ladder_rescaled"] = df["life_ladder"].apply(lambda x: x / 10)

def my_function(x):
    return x * 2

df["life_ladder_rescaled"] = df["life_ladder"].apply(my_function)

# Show the new dataframe
df.head()
```