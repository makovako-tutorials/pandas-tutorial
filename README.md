# Pandas tutorials

Some notes and code from [this](https://www.youtube.com/playlist?list=PL-osiE80TeTsWmV9i9c58mdDCSskIFdDS)

## Data

Data came from Stack Overflow 2019 survey. Download them and put them do data folder, so notebooks can function properly.

# 1 Installation

- data analysis library (csv, excel)
- `jupyter notebook` - run notebook
- dataframe - rows and columns of data
- df
  - shape
  - info() - object type usually means string
  - pd.set_option('display.max_columns', 85) - how many columns shoul be displayed
  - pd.set_option('display.max_rows', 85)
  - head(x) - first x items
  - tail(x) - last x items

# 2 Dataframe and Series

- rows and columns like a table
- df is like dicitonary, where values are lists
- keys ale column names and list is the column values
- series datatype - 1D array - row of single column
- dataframe - contianer for multiple series objects
- bracket and dot notation, dot cannot be used with reserved keywords like count
- get columns
  - `df[["last","email"]]`
- get rows
  - loc - searching by labeles - for rows those are indexes
  - iloc - integer location
  - also allow to filter out columns
  - `df.iloc[[1,2],[3,4]]`
  - can use slicing `0:2` - inclusive

# 3 Indexes

- `df.set_index('name of the column')` - to choose some locumn as index for rows
- `df.set_index('name of the column', inplace=True)` - to choose some locumn as index for rows, changes original df
- when loading `index_col="colname"`

# 4 Filtering

- `df["last] == "Doe"`
- `df[df["last"] == "DOE"]`
- `df.loc(df["last] == "DOE)`
- filtering gives us rows of boolean values
- loc will filter them as it should, then can give column names to get filtered cells
- it is like applying mask to the df
- `& | ~` - and or negation

# 5 Update

- renaming columns
  - `df.columns = ['one', 'two', 'three]`
  - `df.columns = [x.upper() for x in df.columns]`
  - `df.columns = df.columns.str.replace(' ', '_')`
  - `df.rename(columns={'frst_name':'first', 'last_name':'last'}, inplace=True)`
- updating data in rows
  - `df.loc[2] = ['John','Smith','email']`
  - `df.loc[2,'first'] = "lala"`
  - `df.at[2,'last'] = 'lele'`
  - `df['email'] = df['email'].str.lower()`
- methods for updating
  - apply - calling function on values - works on df and series
    - `df['email'].apply(len)`
    - or custom or lambda function
    - `.apply(lambda x: x.upper())`
    - `df.apply` - applying function to each series in dataframe, NOT each value
    - `df.apply(fn, axis='rows')`
    - `df.apply(pd.Series.min)`
  - map
    - only on series, substituting each value with different value, dictionary for substitution
    - not substituted values will be converted to NaN, if not wanted, use replace method
    - df['first'].map({'Corey':'James'})
  - applymap
    - only works on dataframes, applys function to each value in dataframe
    - `df.applymap(len)`
  - replace
    - works as map, but doesnt use NaN values
- view or copy of dataset, on copy the changes cannot be done

# 6 Add/REmove

- add column
  - `df['name'] = df['first'] + ' ' + df['last']`
  - needs the bracket notation
- remove column
  - `df.drop(columns=['first','last'])`
  - `df['name'].str.split(' ', expand=True)` - expand creates df instead of lists
  - `df[['first','last']] = df...split...`
- add rows
  - `df.append({'first':'TOny', ignore_index=True})` - ignoreindex will automatically create one, other column values will have NaN
  - `df.append(df2, ignore_index=True, sort=False)`
  - no inplace method need to do df = df.append...
- remove rows
  - `df.drop(index=4)` - drop row with index of 4
  - `df.drop(index=df[df['last'] == 'Doe'].index)`

# 7 Sorting

- sorting columns, grabing max min
- `df.sort_values(by='last')` - sort by columnanme last
- `df.sort_values(by='last',ascending=False)` - sort by columnanme last in descending order
- `df.sort_values(by=['last','first'])` - sort by columnanme last, on match sort on first name
- `df.sort_values(by=['last','first'], ascending=[False, True])` - sort by last and first name, frst desc, second asc
- `df.sort_values(by=['last','first'], ascending=[False, True], inplace=True)` - sort by last and first name, frst desc, second asc
- `df.sort_index()` - sort df by index
- `df['last].sort_values()`
- `df['Salary'].nlargest(10)` - 10 largest salaries
- `df.nlargest(10,'Salary')` - on whole df on some column
- `df.nsmallest(10,''Salary)` - 10 smallest salaries on whole df

# 8 Grouping and Agregating

- median
  - on series - normal median
  - on df - median on every column series, that has numerical values
- describe
  - some stats of dataframe
  - some stats of series
- mean
  - effected by outliers
- count
  - count of not missing (NaN) rows
- value_counts
  - breakdown of counts of each value
  - normalize=True - percentage
- Grouping of data
  - when we want results per specifinc column, we need to gorup that column
  - group_by - splitting, applying function, combine results
  - `df.group_by([listofcolumns to gorup by])`
  - DataFrameGroupBy - it has groups
  - `df.groupby(...).get_group('name of group')` - getting groups
  - without grouping - `df.loc[df['Country'] == 'United States']['SocialMedia].value_counts()` - but only for one country
  - `df.groupby(...)['Social Media'].value_counts()` - get social media counts per country
- Aggregation
  - agg function, put there all aggregating functions to apply
- concat functions - appending more series `pd.DataFrame.concat(series...)`

# 9 Handling Missing values and cleaning data

- empty values
  - np.nan
  - None
- drop missing values
  - `df.dropna()` - it deletes rows with missing values
  - default arguments - axis='index', how='any'
  - axis - index/columns - drop rows or columns
  - how - criteria for droping
    - all - drop only if all arguments are missing
  - missing values only on specific column
    - argument subset='columnname' - for checking
  - custom missing values - like string with word missing
    - we can replace our values if loaded from dictionary
    - `.replace('NA', np.nan, inplace=True)
    - `.replace('Missing', np.nan, inplace=True)
    - if loaded from csv - na_values argument while loading, expects list of such values
  - df.isna() - gives boolean mask if cell is classified as na
  - df.fillna('value') - replaces na values with custom value
- casting data types
  - on nan values need to use float type
  - `df['age] = df['age'].astype(int)` - cast to integer, cant have nan values
- .unique - returns unique values of column - so we can see which vlaues it has, if we can convert or run computations
