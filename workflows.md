# Data Structures
## Convert list of dicts to Pandas DataFrame 
df = pandas.DataFrame(list_of_dicts)

## Save Pandas DataFrame to disk using Parquet
import pyarrow.parquet as pq

table = pyarrow.Table.from_pandas(df)
pq.write_table(table, 'filename.parquet')

## Read Parquet from disk
table = pq.read_table('filename.parquet')
### Use multithreaded reads when data columns are expensive to decode. 
Use the parameter: nthreads=NUM
### Read only a subset of columns for speed. Pass them as an argument:
columns=['one', 'three']
# Development Tools
## Automatically load environment variables
pipenv will automatically load variables defined in a .env file

## Import a set of packages into iPython shell on startup
Define a Python file that contains all imports, then:
export PYTHONSTARTUP=./startup.py

iPython will read $PYTHONSTARTUP and run the script



# Creating new data from data
## Add row to a Pandas DataFrame of the change in values of another row
If you want the differences starting with first and second:

df['new_row'] = df['data'] - df['data'].shift()


Use .shift(-1) to start with last and next to last.


## Datetime from date str
pandas.to_datetime(dataframe['column'], format='...')

Format can be omitted and Pandas will attempt to infer the format.



## Sampling using dataset as source
sample = dataframe.iloc[numpy.random.choice(dataframe.index.values, SAMPLE_NUMBER)]

# Visualizing
## Display plots in Jupyter
Use: %matplotlib inline



## Show a distribution plot
import seaborn as sns

sns.set(color_codes=True)

import matplotlib.pyplot as plt

sns.distplot(df['change'][1:], bins=OPTIONAL)

plt.show()



