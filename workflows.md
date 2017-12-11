# Data Structures
## Convert list of dicts to Pandas DataFrame 
df = pandas.DataFrame(list_of_dicts)

## Save Pandas DataFrame to disk using Parquet
from pyarrow import parquet
import pyarrow

table = pyarrow.Table.from_pandas(df)
parquet.write_table(table, 'filename.parquet')

## Read CSV from disk
dataframe = pandas.read_csv('data.csv')

## Read Parquet from disk and convert to Pandas DataFrame
import pyarrow.parquet as pq

arrow_table = pq.read_table('filename.parquet')
df = arrow_table.to_pandas()

### Use multithreaded reads when data columns are expensive to decode. 
Use the parameter: nthreads=NUM
### Read only a subset of columns for speed. Pass them as an argument:
columns=['one', 'three']

# Design
## Operating on a slice of dataframe (Expand on this and write about resetting indexes)
Do not slice/filter a dataframe and then try to assign new data to it using regular methods.
For example, you filter out rows based on a date column for dates before a certain date.


# Development Tools
## Automatically load environment variables
pipenv will automatically load variables defined in a .env file

## Import a set of packages into iPython shell on startup
Define a Python file that contains all imports, then:
export PYTHONSTARTUP=./startup.py

iPython will read $PYTHONSTARTUP and run the script



## Apache Avro
### Installing under Python 3 
The pip package does not work with Python3, or at least has a bug where some Python2 code exists.

Download the latest release from http://apache.claz.org/avro/ 
### Documentation and Resources
Current documentation for the official Python Avro library is lacking,
so I'm collecting a dump of links that seem useful 

https://gist.github.com/meatcar/081f9c852928934a7029
https://blog.sandipb.net/2015/05/20/serializing-structured-data-into-avro-using-python/
http://layer0.authentise.com/getting-started-with-avro-and-python-3.html

https://grisha.org/blog/2013/06/21/on-converting-json-to-avro/
https://github.com/grisha/json2avro

This is out of date and uses Python 2
https://github.com/phunt/avro-rpc-quickstart
Possibly out of date but clear and concise examples of reading and writing Avro
https://gist.github.com/dondoco7/2715560

Info on best practices, uses, schema examples, and code examples in Java that are still useful
http://cloudurable.com/blog/avro/index.html

Appears to be the most recent version of Apache Avro (current is 1.8.x) that lays out the Python API:
https://avro.apache.org/docs/1.2.0/

Generates schemas from Java classes, neat idea: https://github.com/sksamuel/avro4s

Avro for JavaScript, works in-browser as well: https://github.com/mtth/avsc

Suggests using fastavro for Python for better performance: http://pythonwise.blogspot.com/2012/03/reading-avro-files-faster-than-java.html


### Schemas
For JSON-formatted data from 3rd party resources, it can be advantageous to leave part of the schema as a string to avoid complications.
Order matters for Avro schemas, watch out for inconsistently ordered fields.

#### TODO: Using Apache Spark to generate schemas

```
from pyspark.sql import SparkSession

```
https://stackoverflow.com/questions/24549146/generating-an-avro-schema-from-a-json-document


## Apache Kafka
### Benchmarks
Python libraries: http://activisiongamescience.github.io/2016/06/15/Kafka-Client-Benchmarking/
Compression: http://blog.yaorenjie.com/2017/01/03/Kafka-0-10-Compression-Benchmark/

### If having issues where clients have issue with resolving hosts
Brokers use advertised.listeners, and will try to connect to these.
Must ensure that a client can resolve the host by adding to /etc/hosts on the client.
(Solution found here: https://stackoverflow.com/questions/43103167/failed-to-resolve-kafka9092-name-or-service-not-known-docker-php-rdkafka)

### Using librdkafka when built from source
Follow the usual procedures (./configure; make; make install) then run ldconfig

## pipenv
Current workaround for installing local packages: "pipenv run pip -e ." from within package directory.

## Python requests library
"All exceptions that Requests explicitly raises inherit from requests.exceptions.RequestException.

"You can tell Requests to stop waiting for a response after a given number of seconds with the timeout parameter. Nearly all production code should use this parameter in nearly all requests. Failure to do so can cause your program to hang indefinitely"

- ConnectionError: raised on a network problem "(e.g. DNS failure, refused connection, etc)"
- HTTPError: raised when using Response.raise_for_status() when the request returns with an unsuccessful status code
- Timeout: raised when a request times out, catching this catches ConnectTimeout and ReadTimeout
- TooManyRedirects: raised when the configured max redirects is exceeded

What about: HTTPSConnectionPool


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
## Split a date range string column into two columns
Assuming data is formatting as "10/7/2017 - 11/1/2017":
df['start date'], df['end date'] = df['date range'].str.split(' - ')str

# Visualizing
## Display plots in Jupyter
Use: %matplotlib inline



## Show a distribution plot
import seaborn as sns

sns.set(color_codes=True)

import matplotlib.pyplot as plt

sns.distplot(df['change'][1:], bins=OPTIONAL)

plt.show()



