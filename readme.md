# Course Transcript - Spark and Python for Big Data with PySpark

* [Course Link](https://www.udemy.com/spark-and-python-for-big-data-with-pyspark/learn/v4/overview)

## Section 1 - Introduction to Course

### Lecture 2 - Course Overview

* do this course if you have done PYthon for DataScience and MachineLearning
* Python for Spark is for enginerrs that want to run their models on Big Data >30GB
* This course is for BigData that standard python libs (numpy,pandas,sciki-learn) can't handle
* This course works on
	* PySpark dataframes
	* Spark 2.0
	* working with very large datasets
	* Spark ecosystem e.g Spark Streaming
* Various installation paths for PySpark. we can use our own (we have one running in AWS from DSMLBootcamp)
course uses jupyter notebook
* Course Curriculum
	* Spark and BIgData Basics
	* Set up Spark in various ways
	* Python Crash Course
	* Python and Spark 2.0 DataFrames
	* PySpark Project Exercise
	* Intro to Machine Learning
	* Linear Regression
	* Logistic Regression
	* Dec Trees and Random Forests
	* Gradient Boosted Trees
	* K-Means Clustering
	* Rewcommender Systems
	* NLP
	* Spark Streaming (LOcal and Twitter)
* New Topics like Pipelines and Cross Validation

### Lecture 4 - What is Spark? Why Python?

* same Lecture like Lecture 120 & 121 in PythonDSMLBootcamp.. we wont keep notes here as we have them in that transcript
* A Master in a Spark Cluster sends Tasks to Slaves and gets back the results. In slave nodes data are cached in RAM/Disk
* Transformations are recipes to follow
* Actions perform what the recipe says to do and return something back
* this behaviour is seen in the source code syntax
* most of the times we will write a method call, but we see no result untill we call the action
* this lazy evaluation makes sense in a large dataset. we dont want to calculate transformations untill we are sure we wnat them performed
* When talking about Spark syntax we see RDD vs DataFrame syntax show up
* With Spark 2.0 Spark is moving towards a DataFrame based syntax, but files are distibuted as RDDs. its the syntax that changes
* Spark DataFrames are now the standard way of using Spark's Machine Learning Capabilities
* Spark DataFrame docs are new and sparse
* [DataFrames docs](http://spark.apache.org/docs/latest/sql-programming-guide.html)
* Spark is a framework written in Scala. Scala is Java based. Python API is a little behind
* Also Scala and SPark is usually faster
* [Mlib Guide](http://spark.apache.org/docs/latest/ml-guide.html) we will use it a lot in the course. it uses RDD API but a DataFrame API is taking over

## Section 2 - Setting up Python with Spark

### Lecture 5 - Set-Up Overview

* there are levtures for 4 installation options
* Realistically Spark won't be running on a single machine, it will run on  a cluster on a service like AWS
* These cluster services will always be Linux based
* Unbderstanding how to set up everything through a linux terminal is essential to get Spark going in the real world
 * our options will be based on Linux (but should work on all OS)
 * our options are Linux (Debian Ubuntu) based and will work locally or remotely
 * the four methods we will cover are:
 	* *Ubuntu+Spark+Python on VirtualBox* : setup a VirtualBox on our local computer and have Ubuntu,spark and python installed locally on this virtual machine
 	* *Amazon EC2 with Python and Spark* (we have it from PythonDSMLBootcamp course) : setup a free micro instance on AWS EC2 to which we connect with SSH
 	* *Databrics Notebook System* : databriks is a company founded by the creator of Spark. there is a freeley hosted Notebook platform supporting various Spark APIs
 	* *AWS EMR (Elstic Map Reduce) Notebook (not free!!)* : allows to quickly setup clusters. NOT FREE 1$/h. allows very quick setup of large cluster. it connects to S3 DB
 * We can get creative. RaspberryPIs , Docker Swarms

## Section 4 - AWS EC2 PySpark Setup 

### Lecture 13 - Installations on EC2

* Done it in PythonDSMLBootcamp course
* here he does not use anaconda but straight python
command script
```
sudo apt-get update
sudo apt get-install python3-pip
pip3 install jupyter
sudo apt-get install default-jre
java -version
sudo apt-get install scala
scala -version
pip3 install py4j
wget http://archive.apache.org/dist/spark/spark-2.1.1/spark-2.1.1-bin-haddop2.7.tgz
sudo tar -zxvf spark-2.1.1-bin-haddop2.7.tgz
ls
cd spark-2.1.1-bin-haddop2.7
pwd
cd ..
pip3 install findspark
jupyter notebook --generate-config
mkdir certs
cd certs
sudo openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem
cd ~/.jupyter/
vi jupyter_notebook_config.py
```
* in vim editor press i
* enter
```
c = get_config()
# Notebook config this is where you saved your pem cert
c.NotebookApp.certfile = u'/home/ubuntu/certs/mycert.pem' 
# Run on all IP addresses of your instance
c.NotebookApp.ip = '*'
# Don't open browser by default
c.NotebookApp.open_browser = False  
# Fix port to 8888
c.NotebookApp.port = 8888
```
* save with ESC and then `:wq`
* continue script
```
cd ~
jupyter notebook
```
* notebook runs
* we visit 8888 port using dns from aws in our remote browser and see the notebook
* we use the link provided in console replacing the localhost with dns name from aws console
* we test with `import pyspark` and get an error. we use 
```
import findspark
findspark.init('/home/ubuntu/spark-2.1.1-bin-haddop2.7')
import pyspark
```

## Section 5 - Databricks Setup

## Section 6 - AWS EMR Cluster Setup

## Section 7 - Python Crash Course

* have done it in PYthon DSML Bootcamp. Exercices are the same.

## Section 8 - Spark DataFrame Basics

### Lecture 23 - Introduction to Spark DataFrames

* similar to pandas dataframes
* Spark DataFrames hold data in a column and row format
* Each column represents some feature or variable
* Each row represents an individual data point
* Spark began with RDD syntax that was ugly and trucky to learn
* Spark @.0 and higger shifted to DataFrame syntax that is cleaner ans easier to work with
* More resembling Scala+Spark
* Spark DataFrames are able to input and output data from a varity of sources.
* We can use these DataFrames to apply various transformations on the data
* At the end of the transformation calls we can either show or collect the results to display or for final processing
* In this section we'll cover main feats of working with DataFrames
* Once familiar with them we ll move on to utilize DataFrame MLib API for Machine Learning
* After this section we ll have a section for a DataFrame POroject
* The PRoject will be an analysis of some historical stock data information using all the Spark knowledge from this section to test our ne skills

### Lecture 25 - Spark DataFrame Basics

* we import SparkSession `from pyspark.sql import SparkSession`
* we start SparkSession by applying it `spark - SparkSession.builder.appName('Basics').getOrCreate()`
* we read in a dataset (people.json) included in course notes `df = spark.read.json('people.json')` provide the path to the file
* to show the dataframe we run `df.show()` we note that some data is missing. we have a 3 row dataset of 2 columns 'age' and 'name' with their labels. one age is missing. spark replaces missing data with *null*
* to see the scema of the dataframe we use `df.printSchema()`. we get the datatype as well
* to see the column names of the df we use the .column attribute. returns a python list
* to get statistical summary of the df we use like pandas `df.describe()`. it returns a dataframe. to see it we need to chain .show() method. the info is only for numerical columns
* in our case the schema is easy to infer as the data is simple. in many case data is complex and we need to define the correct schema
* to do this first we import the datatypes and objects to use. `from pyspark.sql.types import StructField,StringType,IntegerType,StructType`
* next we need to create a list of structurefields (name,datatype, nullable)
```
data_schema = [
	StructField('age',IntegerType(),True),
	StructField('name',StringType(),True),
]
```
* then we set the schema we are expecting from our data `final_struc = StructType(fields=data_schema)`
* we then use the final_struc as the schema when we parse the file in a dataframe `df = spark.read.json('people.json',schema=final_struc)`
* if we print the schema of the df `df.printschema()` we see it is as we defined it so we get predictable resutls
* SPARK is good at infering schemas from data but we have this option when things get nasty

### Lecture 26 - Spark DataFrame Basics Part Two

* if i grabn a column like in a pandas dataframe `df['age']` i get a column object of Column type
* if i want to get a dtaframe of that single column to work on the data i have to use the .select() method `df.select('age')` to see the data i have to chain the .show() method
* if i want to see the first two rows in a dataset i use `df.head(2)`. what i get back is a list of row objects. to get the first of the 2 ican use `df.head(2)[0]`
* The reason that Spark uses special objects for Rows and Columns is because of its ability to read data f4rom distributed sources and then map them out to a single data set
* if i want to select multiple column we use select passing a list of column names `df.select(['age','name'])`
* we can create new columns we use .withCoumn() . this method returns a new datarame with anew column ore replacing an existing one. we pass the name of our new column and then a Column object. `df.withColumn('newage',df['age'])` we can do math operations on the Column object `df.withColumn('double_age',df['age']*2)` . the returned datafram is not persistent. we need to assign it to a new var to save it. so it is NOT an *inplace* operation
* to rename a column we use the withColumn() passing the new name `df.withColumn('age','superage')`
* We can use pure SQL to interact with the dataframe
* to use SQL on the datafram we need to register it as a *SQL temporary view* using a special method on the dataframe `df.createOrReplaceTempView('people')` passing the name of the view
* i can then get the results as a spark dataframe using sql queries `results = spark.sql("SELECT * FROM people")`
* we can issue more complex queries `spark.sql("SELECT * FROM people WHERE age=30").show()`

### Lecture 27 - Spark DataFrame Basic Operations

* we will now learn how to filter data once we grab them.
* we import SparkSession `from pyspark.sql import SparkSession`
* we create a sparksession running instance `spark = SparkSession.builder.appName("Operations").getOrCreate()`
* we will read in anew file a csv into a dataframe infering the scehma (a csv option) . also we tell it that the first row is the header `df = spark.read.csv('appl_stock.csv',inferSchema=True,header=True)`
* we print the schema with .printSchema() . we have 7 columns with inof about the apple stock for each date. we cahn .show() the df . it has a lot of rows
* spark is able to filter out data based on conditions. 
* spark datarames are built on top of spark sql platform.
* if we know sql we can quickly grab data using sql commands. 
* but in this coursw we will use FataFrame methods to operate on data
* we can use SQL syntax in the filter() method `df.filter("Close < 500").show()` or select from the filetered data chaining .select() method `df.filter("Close<500").select('Open').show()` or passign a list of columns in select
* we can use python style syntax (like pandas) in filter() instead `df.filter(df['Close'] < 500).show()` or `df.filter(df['Close'] < 500).select('Volume').show()`
* if want to filter based on multiple conditions i use same python syntax like in pandas dataframes combined conditions. we make use to use parenthesses in subconditions here to avoid py4j (java trnaslation) errors `df.filter( (df["Close"] < 200) & (df['Open'] > 200) ).show()`
* NOT operator in python syntax is ~ , not ! .`~(df['Open'] < 200)`
* just showing the filtered results is not always useful. many times we need to work on the data. we use the .collect() old RDD style spark method to make them in-memory lists to work with `result = df.filter(df["Low"] == 197.16).collect()`. what we get back is a list of row objects
* we can store it in a variable and work on it later on e.g grab the row, make it a dictionary and extract data
```
row = result[0]
row.asDict()['Volume']
```
* or itrate through row and get data 
```
for item in result[0]:
    print(item)
```

### Lecture 27 - GroupBy and Aggregate Operations

* we import and create a SparkSession with name *groupbyagg*
* we parse a csv to a dataframe `df = spark.read.csv('sales_info.csv',inferSchema=True,header=True)` and infer the schema
* we print the schema (3 columns Company,Person and Sales) and view the df
* we want to groupby the company column. the method is like pandas `df.groupBy('Company')`. what we get back is a GroupedData object
* we can chain aggregate methods like .mean() .sum() .max() ,count() and get a Dataframe returned much like pandas dataframes
* instead of groupby we can use the .agg() method to get aggregates of the whole dataset passing the critaria as a dictionary and get a DatafRame back. `df.agg({'Sales':'sum'}).show()`
* we can apply the agg method on a GroupedData object
```
group_data = df.groupBy('Company')
group_data.agg({'Sales':'max'}).show()
```
* there are many advances statistical methods we can import from spark. e.g `from pyspark.sql.functions import countDistict,avg,stddev`
* we can apply the imported functions with a .select() call `df.select(countDistict('Sales')).show()` passing teh column we want to apply it on. It returns a DataFrame
* we can pass an alias for the column label `df.select(avg('Sales').alias('Average Salse')).show()`
* we can format long numbers to improve their looks. we import format_number `from pyspark.sql.functions import format_number`. we use it by passing it in the select() call specifying the column to apply to and the decimals we want to show
```
sales_std = df.select(stddev("Sales").alias('std'))
sales_std.select(format_number('std',2)).show()
```
* again we need to pass alias as format_number is used in column label
* we can order data with .orderBy() `df.orderBy('Sales').show()` it orders in ascending order.
* if we want to sort in descending order we need to pass a column object and apply .desc() on it `df.orderBy(df['Salse'].desc()).show()`

### Lecture 28 - Missing Data

* we have 3 options when we have missing data in our dataset
	* keep them as nulls
	* drop missing points incuding all the row
	* fill them with another value
* we import and create a sparksession
* we parse a csv file with nulls in a dataframe
* we have 3 columns and 4 rows. 1 row where both data are missing, 2 rows where one data is missing and 1 row complete
* we have access to null handling methods in `df.na.` we have .df() .drop() .fill() and .replace()
* if we use drop `df.na.drop().show()` it will drop any row containing missing data. if we pass a thresh param. it only drops rows with  or more missing data than the threshold `df.na.drop(thresh=2).show()` 
* we can use the how param instead of threshold using keywords like 'all' (drop if all vals are null) or 'any' if even one is null `df.na.drop(how='any').show()`
* we can pass the subset param passing a list of column names. this considers these columns for nulls (with how='any' or therh=1) to drop `df.na.drop(subset=["Sales"]).show()`
* instead of drop we can fill the missing values . if i pass a string `df.na.fill('FILL VAL').show()` sparks sees we pass a string and fill only nulls in string columns with the value. if we pass a num val it fills only num columns
* usually we speciffy the column that we want to fill  the nulls with the specific val `df.na.fill('No Name',subset=['Name']).show()`
* we can get the mean value of Sales column using techniques we have seen so far and then use it to fill the nulls
```
from pyspark.sql.functions import mean
mean_val = df.select(mean(df['Sales'])).collect()
mean_sales = mean_val[0][0]
df.na.fill(mean_sales,["Sales"]).show()
```
* we can do it in an 1liner `df.na.fill(df.select(mean(df['Sales'])).collect()[0][0],['Sales']).show()`

### Lecture 29 - Dates and Timestamps

* we import , create a session and parse  data from a csv (apple_stock,csv) to a dataframe
* our first column is 'Date'. in schema it appears as timestamp. in the Row object as datetime.datetime
* we can extract data from datetime object. to do so we import helper functions from spark `from pyspark.sql.functions import (dayofmonth,hour,dayofyear,month,year,weekofyear,format_number,date_format)`
* we apply them with select passing in as param the Column object. what we get back is a new dataframe eg `df.select(dayofmonth(df['Date'])).show()`
* if we want to know the avg closing price per year we do => apply year with select to Date column => creaa new column .withColumn() => store it as a new dataframe => groupby per year => apply ,mean() => select the columns i want
```
newdf = df.withColumn("Year",year(df['Date']))
result = newdf.groupBy("Year").mean()[['avg(Year)','avg(Close)']]
result = result.withColumnRenamed("avg(Year)","Year")
result = result.select('Year',format_number('avg(Close)',2).alias("Mean Close")).show()
```