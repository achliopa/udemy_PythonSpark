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
* to see the column names of the df we use the .columns attribute. returns a python list
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
* if want to filter based on multiple conditions i use same python syntax like in pandas dataframes combined conditions. we make use to use parentheses in subconditions here to avoid py4j (java trnaslation) errors `df.filter( (df["Close"] < 200) & (df['Open'] > 200) ).show()`
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

## Section 9 - Spark DataFrame Project Exercise

### Lecure 30 - DataFrame Project Exercise

* with select we can extract multiple columns forming a new DataFrame. the columns might be transformed applying cast or other functions
```
result.select(result['summary'],
    format_number(result['Open'].cast('float'),2).alias('Open'),
    format_number(result['High'].cast('float'),2).alias('High'),
    format_number(result['Low'].cast('float'),2).alias('Low'),
    format_number(result['Close'].cast('float'),2).alias('Close'),
    result['Volume'].cast('int').alias('Volume')
    ).show()
```
* Create a new dataframe with a column called HV Ratio that is the ratio of the High Price versus volume of stock traded for a day.
* My solution:
```
df_ratio = df.select((df['High']/df['Volume']).alias('HV Ratio'))
df_ratio.show()
```
* Teachers Solution
```
df2 = df.withColumn("HV Ratio",df["High"]/df["Volume"])#.show()
# df2.show()
df2.select('HV Ratio').show()
```
* What day had the Peak High in Price?
* My solution
```
max = df.agg({'High':'max'}).collect()[0].asDict()['max(High)']
df.filter(df['High'] == max).select('Date').show()
```
* Teachers solution
```
df.orderBy(df["High"].desc()).head(1)[0][0]
```

## Section 10 - Introduction to Machine LEarning with MLlib

### Lecture 32 - Introduction to Machine Learning and ISLR

* Machine Learning Sections will have:
	* Suggested Reading Assignment
	* Basic Theory LEcture
	* Documentation Walkthrough
	* More realistic custom code example
	* Consulting Project
	* Consulting Project Solutions
* Consulting Projects are Looser, realistic project fo us to attempt with the skills just learned
* Machine Learning is a method of data analysis that automates analytical mnodel building
* Using algorithms that iteratively learn from data, machine learning allows computers to find hidden insights without being explicitly programmed where to look
* ML is used in many applications
* ML Flow is Analyze Data, Clean Data, SPlit Data, Iterate(Train Model , Evaluate Model) => Deploy Model
* Spark MLlib is mainly designed for Supervised and Unsupervised Learning tasks. Most of its algorithms fall in these categories.
* *Supervised learning* algorithms are trained using labeled examples. an inpute where the desired output is known. THe learning algo receives a set of inputs with the corresponding correct outputs and it learns by compaing its actual output with correct outputs to find errors. it then modifies model accordingly
* Though methods like classification, regression, prediction and gradient boosting, supervised  learining uses patterns to predict the values of the label on additional unlabeled data.
* Supervised learning is used where historical data predicts likely future events
* *Unsupervised learning* is used against data that has no historical labels
* the system is not told the "right answer" the algo mustfugure out what is shown
* the goal is to explore the data and find some structure within. 
* e.g it can find the main attributes that separate custmer segments from each other.
* popular techniques include: sel-organizing maps, nearest-neighbour mapping, k-means-clusterring, singular value decomposition
* one issue is that it can be difficult to evaluate results of an unsupervised model

### Lecture 33 - Machine Learning with Spark and Python with MLlib

* Spark has its own MLlib for Machine Learning
* The future of MLlib uses Spark 2.0 DataFrame syntax
* One of the main perks of MLlib is that we need to format our data so that eventually it just has one or tewo columns:
	* Features,Labels (Supervised)
	* Features (Unsupervised)
* So if we have multple feature columns  in our dataframe we need to condense it to a single colun where each row is an array of the old entries
* This requires more data processing than other ML libs, but owr syntax is applicable to distributed big data
* In owr documetation examples are with nicelly formated data.
* in the custom code-along examples the data will be realistic and messy
* the project will have real-world data
* To get good at MLlib we must familiarize ourselves with the documentation
* [MLlib docs](https://spark.apache.org/docs/latest/ml-guide.html)
* it has documentation for all mahjor algorithms
* Extracting,Transforming and Selecting feats is essential to prepare our datafor MLib

## Section 11 - Linear Regression

### Lecture 34 - Linear Regression Theory and Reading

* Theory is same as in PythonDSMLBootcamp. so we skip the lecture

### Lecture 35 - Linear Regression Documentation Example

* we will use [MLlib documentation page](https://spark.apache.org/docs/latest/ml-classification-regression.html#linear-regression), Data from Documentation, Linear_regression_Example.ipynb, New Untitled Notebook
* we import SparkSession `from pyspark.sql import SparkSession`
* we create a session `spark = SparkSession.builder.appName('lrex').getOrCreate()`
* we import LinearRegression from MLlib regression group of models. `from pyspark.ml.regression import LinearRegression`
* we load in our training data as a DataFrame using a new format. libsvm `training = spark.read.format('libsvm').load('sample_linear_regression_data.txt')`
* we view the dataframe `training.show()` . its already formatted and ready for MLlib. feats are one column of arrays with all feats (vector of feats)
* we create an instance of our model passing the necessary params. which is the feats col name, the labels col name, the new prediction col name `lr = LinearRegression(featuresCol='features',labelCol='label',predictionCol='prediction')`
* then we fit our model passign teh complete dataframe `lrModel = lr.fit(training)`
* we can see the coeficients of the model `lrModel.coefficients` and the intercept `lrModel.intercept`. the coefficient means the feature importance
* we can view in the model summary a lot of specific attributes like r2 (variance explained by our model) or `lrModel.summary.rootMeanSquaredError`
* in the doc examples data are never split into training and test. so we trained on all our available data
* we want to do the train test plit. so we reload the data from the file as all_data `all_data = spark.read.format('libsvm').load('sample_linear_regression_data.txt')`
* we call random_split() method available to all dataframers passing the split ratio as an array `train_data,test_data = all_data.randomSplit([0.7,0.3])`, we get a list of 2 dataframes , first has 70% of the data the second 30%. usually we du tuple unpacking to train and test data
* we fit our model to the train data `current_model = lr.fit(train-data)`
* we use the evaluate method on the model to get the  predictions based on teh test data `test_resutls = current_model.evaluate(test_data)`
* we can now get inof on the test results `test_resutls.residuals.show()` or get the routsquareerror
* using evaluate on test_data we are comparing our predictions to the labels that where assigned to the test data
* we can use the metrics of evaluation to tune various model params to get better results
* we usually deploy our model to data with no label
* we simulate the process by selecting the features column from test-data `unlabeled_data = test_data.select('features')`
* we can now get the predictions using the transform method on the model passing the unlabeled data `predictions = current_model.transform(unlabeled_data)`
* predictions is a complete dataframe with assigned labels ('predictions') and feats column
* when we predict unlabeled data we cannot get evaluation metrics as we dont have a reference

### Lecture 36 - Regression Evaluation

* we ll evaluate regression in general. any model that attempts to predict continuous values (unlike categorical values, which is called classification)
* Accuracy and Recall evaluation metrics are not useful for regression problems. we need metrics designed for continuous values
* The most common evaluation metrics for regression are:
	* Mean Absolute Error (MAE). the mean of absolute value of errors , average error
	* Mean Squared Error (MSE), the mean of the squared errors. larger error are noted more than with MAE, thus MSE is more popular. but we work with squared units, which is not very useful on perception
	* Root Mean Square Error (RMSE) is MSE but rooted so the units are like MAE so more useful. is the MOst popular
	* R Squared Value R2 (more a statistical property of the model) is the coeffiient of determination
* R2 by itself does not tell the whole story. its a measure of how much  variance our model accounts for
* Takes values between 0 and 1 (0 - 100%)
* there are different ways to get is like adjusted R squared. some ways can yield a negative value (see wiki). use adjusted R2
* Rsquared can enhance our understanding of a model, help compare models but not be used  in isolation as the only source of evaluation

### Lecture 37 - Linear Regression Example Code Along

* we ll examine an ecommerce customer data for a companys website and mobile app
* we ll try to predict customers total amount expenditure (continuous money val)
* we ll see how to convert realistic data into  a format accepted by Sparks MLlib
* we import the SparkSession `from pyspark.sql import SparkSession`
* we create a SparkSession `spark = SparkSession.builder.appName('lr_example').getOrCreate()`
* we import LinearRegression `from pyspark.sql.ml.regression import LinearRegression`
* we fetch our data `data = spark.rea.csv('Ecommerce_Customers.csv',header=True,inferSchema=True)`
* we print the schema `data.printSchema()`
* we view the data `data.show()`
* we view first row
```
for item in data.head(1)[0]:
	print(item)
```
* we need to prepare our dataset for MLlib we import Vector Assembler 
```
from pyspark.ml.linalg import Vectors
from pyspark.ml.feature import VectorAsembler
```
* we see the columns `data.columns`
* we are interested only into the numeric ones for our feats while Yearly Amount Spent will be our lables column
* we are now ready to build our vectors. we create our VrectorAssebler passing as input an array of the col labels  and as output the vectors array col name
```
assembler = VectorAssembler(
    inputCols=["Avg Session Length", "Time on App", 
               "Time on Website",'Length of Membership'],
    outputCol="features")
```
* we now have to transform the data into the vector using the assembler  `output = assembler.transform(data)`
we check the table schema `output.printSchema()` we have the dataset as it was + the features column
* we check this volumn `output.select('features').show()`. we see the first row `output.head(1)` and see the featues col is a DenseVector
* we extract the columns of interest for MLlib `final_data = output.select('features','Yearly Amount Spent')`
* we split our data into a train and test set `train_data,test_data = final_data.randomSplit([0.7,0.3])`
* we see the properties of both sets with .describe().show()
* we now create our linear regression model instance `lr = LinearRegression(labelCol='Yearly Amount Spent')`
* we now train our model `lr_model = lr.fit(train_data)`
* we  evaluate the model getting the results passing the test data `test_results = lr_model.evaluate(test_data)`
* we check the residials (diff between pred and actual test data) `test_results.residuals.show()`
* we no check the regression metrics `test_results.rootMeanSquareError` or `test_results.r2` . the values we get are good so our model is working OK. to get a measure of comparison with the label vaules we ptint `final_data.describe().show()`. we compare RSME to the mean value . r2 says our model explais 98% of varianve in the data.
* usually when i get really good metrics we double check our dataset
* how we deploy our model? `unlabeled_data. = test_data.select('features')` has only feats so it is good to simulate input data. we do our predictions `predictions = lr_model.transform(unlabeled_data)` which has now the predicted labels col

### Lecture 38 - Linear Regression Consulting Project

* we ve been contacted by Hyundai Heavy Industies to help them build a predictive model for ships
* they need our help to give accurate estimates on how many crew members a ship will require
* they are selling ships to new customers and want us to create amodel and use it to predict how many crew members the ship will need
* they provide us with a dataset with these feats:
	* Ship Name
	* Cruise Line
	* Age (as of 2013)
	* Tonnage (1000s of tons)
	* passengers (100s)
	* Length (100s of feet)
	* cabinbs (100s)
	* passenger density 
	* Crew (100s)
* Our job is to create a regression model that will help predict how many crew
* we should use the feats we think will be useful to predict the value
* cruise lines differ in acceptable crew counts so we should use it. we should use StringIndexer

## Section 12 - Logistic Regression

### Lecture 40 - Logistic Regression Theory and Reading

* Same as in PythonDSMLBootcamp
* Accuracy is (ΣΤP + ΣΤΝ)/Σ Τotal Population
* Good metrics is dependent on the application (usually we care about accuracy or recall)
* Binary Classification has its own classification metrics (vizualizations of metrics from the confusion matrix)
* The receiver operator curve (ROC) was developed in WWII to analyze radar data
* ROC curve is a plot of Sensitivity (True Positive rate) / (1 - Specificity(False Positive Rate))
* the diagonal line on the plot is the random guess (50% chance to get it right) the top left corner is the Prfect Classification. if we are in the half above the diagonal we are perfrming better than a random guess

### Lecture 41 - Logistic Regression Example Code Along

* We will introduce the concept of *Evaluators*
* Evaluators behave similar to Machine Learning Algorithm objects, but are designed to take in evaluation DataFrames `model.evaluate(test_data)`
* Evaluators are technically still experimental acording to the MLlib docs, so we ll use them with caution in production code
* They are part of Spark since v1.4 so they are stable
* we ll work on sample_libsim_data.txt
* we import a sparksession 	`from pyspark.sql import SparkSession`
* we create a session `spark = SparkSession.builder.appName('mylogreg').getOrCreate()`
* we import the model `from pyspark.ml.classification import LogisticRegression`
* we grab our training data `my_data = spark.read.format('libsvm')`.load('sample_libsvm_data.txt')
* we view our data `my_data.show()` they are already formated for MLlib. a labels column (binary) and a feats column of vestors
* we create our model `my_log_reg_model = LogisticRegression()`
* we fit our model on the training data `fitted_logreg = my_log_reg_model.fit(my_data)`
* we can get a summary of our fitted model `log_summary = fitted_logreg.summary
* we print out the schema of predictions DataFrame `log_summary.predictions.printSchema()`. it has the actual label, the actual features. the raw prediction, the probability of the prediction and the prediction value as a label. what we are interested is to see if the label column matches the rpediction column
* we will now see how to evaluate results using evaluators. to do so we first split our data into training and test sets
* lr_train,lr_test = we use random split `my_data.random_split([0.7,0.3])`
* we retrain our model on the training data 
```
final_model = LogisticRegression()
fit_final = final_model.fit(lr_train)
```
* we use evaluate to get the predicted dataset `prediction_and_labels = fit_final.evaluate(lr_test)`
* like in previous example with the summary we can call `predictions_and_labels.predictions.show()` and view the datast with actual and rpedicted lables. but now it is not a 100% match as we used test-data
* we import our evLUtora for binary and multiclass `from pyspark.ml.evaluation import BinaryClassificationEvaluator,MulticlassClassificationEvaluator`
* evaluator works on the predictions DF and requires a metricsName (areaunderROC). to get more metrics (accuray recall etc) we need to use the MulticlassClassificationEvaluator
* we create an evaluator `my_eval = BinaryClassificationEvaluator()`
* we use it to evaluate passing the parms it needs `my_final_roc = my_eval.evaluate(prediction_and_labels.predictions)` what we get is a 1.0 (area under ROC) so its aperfect fit

### Lecture 42 - Logistic Regression Code Along

* we ll work on a 'classic' classification example . the titanic dataset (AGAIN...)
* we will see a better way to deal with categorical data with a two step process
* we will see how to use pipleines to set stages and build reusable models
* our data will have missing information so we will have to prepare it
* we import sparksession and create a session named 'myproj'
* we grab in the data `df = spark.read.csv('titanic.csv',inferSchema=True,header=True)`
* we print the schema  `df.printSchema()` we have amultitude of columns
* we select only columns relevant ot our problem `my_cols = df.select(['Survived', 'Pclass', 'Sex', 'Age', 'SibSp', 'Parch', 'Fare', 'Embarked'])`
* we have to deal with missing data. we will just drop them `my_final_data = my_cols.na.drop()`
* we now have to wor on our categorical columns. irst we import helpers from ml.feature lib `from pyspark.ml.feature import VectorAssembler,VectorIndexer,OneHotEncoder,StringIndexer`
* we will use StringIndexer on categorical columns and the HotEncode them to produce numerical vals outr of string categories `gender_indexer = StringIndexer(inputCol='Sex', outputCol='SexIndex')`
* we will now onehotencode them. transform the indexes for the cats into one hot encoding which is an array with 0 and 1 indicating the category e.g A,B,C => 0,1,2 => [1,0,0],[0,1,0],[0,0,1] `gender_encoder = OneHotEncoder(inputCol='SexIndex',outputCol='SexVec')`
* we follow the same approach for embarked column
```
embark_indexer = StringIndexer(inputCol='Embarked',outputCol='EmbarkIndex')
embark_encoder = OneHotEncoder(inputCol='EmbarkIndex',outputCol='EmbarkVec')
```
* we are now ready to assemble all feat cols into a vector
```
assembler = VectorAssembler(inputCols=['Pclass', 'SexVec', 'Age', 'SibSp', 'Parch', 'Fare', 'EmbarkedVec'], outputCol='features')
```
* we are now ready to train our model w/ data
* we import our model `from pyspark.ml.classification import LogisticRegression`
* we also import the pipeline `from pyspark.ml import Pipeline`. pipeline builds a pipeline of stages for each step
* we create the model specing the col names to expect `log_reg_titanic = LogisticRegression(featuresCol='features', labelCol='Survived')`
* now we create our pipeline instance where we put the already defined stages of data prep and model building `pipeline = Pipeline(stages=[gender_indexer, embark_indexer, gender_encoder,embark_encoder, assembler,log_reg_titanic])` Ordr matters
* we split our data `train_data, test_data = my_final_data.randomSplit([0.7,0.3])`
* we can now use the pipeline as a model where input data are passed in all steps. `fit_model = pipeline.fit(train_data)`
* we now get the results of our model evaluation by transforming out test_data `results = fit_model.transorm(test_data)`
* we now import the BinaryEvaluator `from pyspark.ml.evaluation import BinaryClassificationEvaluator`
* we instantiate it passing the actual col labels of our results df `my_eval = BinaryClassificationEvaluator(rawPredictionCol='prediction', labelCol='Survived')` prediction is the default coname for transform outpu and Survived the labels colname in our original data
* we ge tthe metric using the evaluate() method `AUC = my_eval.evaluate(results)` (area under the curve) the val is 0.76 which needs improvement

### Lecture 43 - Logistic Regression Consulting Project

* we have been contacted by a top marketing agency to help them out with customer churn
* we need to help them predict custiomer churn
* a marketing agency has many coustomers that use thir service to produce ads for the clientcustomer websites
* they noticed they have quite a bit of churn in clients (lose clients)
* they currently randomly assign account managers, but want us to create a machine learning model that will help predict which customers will churn (stop buying the service) so thatt they can correctly assign the customers at risk an account manager
* they have historical data
* we have to create a classification algorithm that will help them classify whether or not a customer churned
* our goal is to create a model that will accurately predict if the customer will churn
* account manager is currently randomly assigned

## Section 13 - Decision Trees and Random Forests

### Lecture 45 - Tree Methods Theory and Reading

* We have done this Lecture in PythonDSMLBootcamp
* Entropy and Information Gain are the Mathematicla Methods of choosing the best Split
* In Random Forests we use many trees. where a new random set of sample featuees is chosen for every single tree ant every split
* Random Forest works for Classification AND Regression (average of predicted values and use that as the label)
* *Gradient Boosted Trees* involve three elements:
	* a loss function to be optimized
	* a weak learner to make predictions
	* an additive model to add weak learners to minimize the loss function
* Loss Function:
	* a loss function in basic terms is the function/equation we will use to determine how  far off our predictions are
	* Regression might use a squared error anf classification may use logarithmic loss
	* We wont have to deal with htis directly using Spark (it happens under the hood)
* Weak Learner: 
	* Decision Trees are used as the weak leaner in gradient boosting
	* It is common to constrain the weak learners: max num of  layers, nodes,splits or leaf nodes
* Additive model: 
	* Trees are added one at a time and existing trees in the model are not changed
	* A gradient descent procedure is used to minimize the loss when adding trees
* Whats the  most intuitive way to think about all this if Spark does all for us?
* We can memorize it thinking of it as 3 'easy' steps:
	* 1. Train a weak model m using data samples drawn according to some weight  distribution
	* 2. Increase the weight of samples that are misclassified by model m and decrease the weight of samples that are classified correctly by model m
	* 3. Train next weak model using samples drawn according to the updated weight distribution
* In this way the algorithm trains models using data samples  that are difficult to learn in previous rounds. this results in an ensemble of models that are good at learning different parts of training data, boosting weights of samples that were difficult to get correct (end of Chapter 8 of ISLR)
* Spark does all this under the hood. we can use the defaults if we want or dive into Theory and play with the params.

### Lecture 46 - Tree Methods Documentation Examples

* we ll work through Decision Trees, Random Forests, Gradient Boosted Trees
* we will show some useful evaluation feats and how to use multiclass evaluatiors in binary data
* we import and create a sparksession
* we import pipeline and classification models
```
from pyspark.ml import Pipeline
from pyspark.ml.classification import RandomForestClassifier,GBTClassifier, DecisionTreeClassifier
```
* these models are avaialble in regression library for regression problems
* we read in the data `data = spark.read.format("libsvm").load("sample_libsvm_data.txt")`
* data are already formated for MLlib
* we split our data `train_data,test_data = data.randomSplit([0.7,0.3])`
* we start creating our classifier objects 
```
dtc = DecisionTreeClassifier()
rfc = RandomForestClassifier(numTrees=100)
gbt = GBTClassifier()
``` 
* these models  have a lot of params for tweaking, but we use defaults for now
* we fit our models
```
dtc_model = dtc.fit(train_data)
rfc_model = rfc.fit(train_data)
gbt_model = gbt.fit(train_data)
```

* we now use them for doing predictions
```
dtc_preds = dtc_model.transform(test_data)
rfc_preds = rfc_model.transform(test_data)
gbt_preds = gbt_model.transform(test_data)
```
* the preds are dataframes containing the actual labels and predictions (labelwise are same) so we can run evaluator on them to get the metrics. we dont even have to set the columns as they are default
* we import the evaluator `from pyspark.ml.evaluation import MulticlassClassificationEvaluator`
* we create an evaluator `acc_eval = MulticlassClassificationEvaluator(metricName='accuracy')` 
```
dtc_accuracy = acc_eval.evaluate(dtc_preds)
rfc_accuracy = acc_eval.evaluate(rfc_preds)
gbt_accuracy = acc_eval.evaluate(gbt_preds)
``` 
* even dtc_accuracy is 1.0 so we have perfect fit (data are artifical)
* we will now learn how to get feature importance. we apply .featureImportances on the model. what we get back is a SparcezVector with the feat and its iomportance, difficult to transalte as its coded

### Lecture 47 - Decision Trees and Random Forest Code Along Examples

* we ll see all 3 methods and compare their rewsults on a real workd dataset (public private labeled unis)
* we import and create a Spark Session
* we load the data `data = spark.read.csv('College.csv',inferSchema=True,header=True)`
* we check the inferedschema. it has a lot of feats
* we need to format data for MLlib so we import assembler `from pyspark.ml.feature import VectorAssembler` 
* we print the columns to use cp to pass them in assembler `data.columns`
* we create the assbler passing all numbeirc feats
```
assembler = VectorAssembler(
  inputCols=['Apps',
             'Accept',
             'Enroll',
             'Top10perc',
             'Top25perc',
             'F_Undergrad',
             'P_Undergrad',
             'Outstate',
             'Room_Board',
             'Books',
             'Personal',
             'PhD',
             'Terminal',
             'S_F_Ratio',
             'perc_alumni',
             'Expend',
             'Grad_Rate'],
              outputCol="features")
```
* we create the output using the assembler to transform the data `output = assembler.transform(data)`
* we now have to make our categorical  label column a vector that MLlib can understand
* we import the indexer `from pyspark.ml.feature import StringIndexer`
* we create the indexer `indexer = StringIndexer(inputCol='Private',outputCol='Privateindex')`
* we get our output fixed with `output_fixed = indexer.fit(output).transform(output)`
* we dont use pipeline as we dont have to repeat the process
* we print out the output_fixed dfs schema. we still have the 2 string cols and the feature columns we should drop (we dont need to as we declare cols that are used in the model) `final_data = output_fixed.select('features'.'privateIndex')`
* we split our data and import the 3 classification models
* we import pipeline
* we create our 3 models fit them and get the predictions
```
dtc = DecisionTreeClassifier(labelCol='PrivateIndex',featuresCol='features')
rfc = RandomForestClassifier(labelCol='PrivateIndex',featuresCol='features')
gbt = GBTClassifier(labelCol='PrivateIndex',featuresCol='features')
dtc_model = dtc.fit(train_data)
rfc_model = rfc.fit(train_data)
gbt_model = gbt.fit(train_data)
dtc_preds = dtc_model.transform(test_data)
rfc_preds = rfc_model.transform(test_data)
gbt_preds = gbt_model.transform(test_data)
```
* we import the BinaryClassificationEvaluator and create the evaluator `my_binary_eval = BinaryClassificationEvaluator(labelCol='PrivateIndex')` 
* we print the UAC for dtc `my_binary_eval.evaluate(dtc_preds)` its 90.9%. RFC gives 97% with default params
* gbt predictions dont have the rawPrediction and probability columns so we have to tweak the BinaryEvaluator to get the prediction column as rawprediction `my_binary_eval2 = BinaryClassificationEvaluator(labelCol='PrivateIndex',rawPredictionCol='prediction')` 
* gbt uac `my_binary_eval2.evaluate(gbt_preds)` is 89% so the worse of all so we need to adjust its params
* we import the MulticlassClassificationEvaluator to get more eval metrics `acc_eval = MulticlassClassificationEvaluator(labelCol='PrivateIndex',metricName='accuracy')`
* we get rfc accurace `rfc_acc = acc_eval.evaluate(rfc_preds)`

### Lecture 48 - Random Forest Classifications Consulting Project

* we been hired by purina to predict why some batches of their dog food spoil much sooner than intendent.
* the dogs food company has not upgraded to the latest machinery meaning that the amounts of the five chemicals they are using very a lot. but which is the chemical with the strongest effect?
* they mix a batch of preservative containing 	4 chemicals (a,b,d,c) and then is completed by a filler chemical
* scientists believe that one of A<B<C<D chemicals is causing the problem but we have to figure out which
* we will use Machine Learning with RF to find out which param had the most predictive power. thus finding the cause
* we have to create a model and then find out how we can decide which chemical is the problem
* the data look like this. Pres_A, Pres_B, Pres_C, Pres_D: ppercentage of preservative in the mix: Spoiled label if the dog batch food was spoiled
* what we actually want is feature importance from the model 

## Section 14 - K-Means Clustering

### Lecture 50 K-means Clustering Theory and Reading

* we ve worked with labeled data. but what about unlabeled ones
* many times we will try to create groups of data, instead of trying to predict classes or vals
* this is a clustering problem. like trying to label the data
* we input ulabeled data and the *unsupervised algo* returns possible clusters of data
* so we have data that only contains feats and we want to see if there are patterns in the data that wouold allow us to create groups or clusters
* by the nature of this problem it can be difficult to evaluate the groups or clusters for 'correctnes'
* so it comes down to domain knowledge to be able to interpret the clusters assigned
* so maybe we have customer data and then cluster them into distinct groups. it will up to us to decide whjat the groups actually represent. soms dat is makkelijk som dat is echte moelijk.
* eg we could cluster tumors in two groups (hoping to separate them  between benign and malignant)
* but there is no guarantee the clusters will fall along these lines. it will just split into the two most separable groups
* depending on the algo it might be up to us to decide beforehand how many clusters we expect to create
* a lot of probs have no 100% correct approach or answer. that the nature of unsuperviosed learning
* About K-Means Clustering we can see our notes from PythonDSMLBootcamp and ISLR book
* pyspark does not support a plotting mechanism. we could use collect() and then plot the results with matplotlib or other visualization libraries
* We should not take the elbow rule as a strict rule when choosing a K value
* a lot of depends on the context of the situation (domain knowledge)
* we will use ML to solve it but without the typical train/test workflow

### Lecture 51 - KMeans Clustering Documentation Example

* we don't need the label column (as we are doing clustering - unsupervised learning)
* the MLlib documentation example is a bit wierd as the dataset is very small
* we import and create a pyspark session
* we import our model `from pyspark.ml.clustering import KMeans`
* we import the data `dataset = spark.read.format("libsvm").load("sample_kmeans_data.txt")`
* we view our data. it is prepared and contains feats vector and label
* we get only the feats `final_data = dataset.select('features')`
* we create our model seting the k num. `kmeans = KMeans().setK(2).setSeed(1)` we also set a seed value for a rng generator. as the initial position of Ks is random
* we fit the model `model = kmeans.fit(final_data)`
* we will try to evaluate our model so we will calc the sum of  square errors SSE `wssse = model.computeCost(final_data)` it s 0.12
* next we will get the cluster centers . as the gfeats vecors are 3 dimensional we expect 3dimensions for the center `centers = model.clusterCenters()` which returns an array with the cluster centers.
* we now want to label our input data so assign a lable to each row. we do this with transform() `results = model.transform(final_data)`
* in unsupervised learning there is no reason to do a train test split
* we increase K and regit the model. wssse goes up
* we see the new centers. they make no added value given the dataset

### Lecture 52 - Clustering Example Code Along

* we will work on a real dataset containing some data on three distingt seed types
* for certain machine learning algirithms it is a good idea to scale your data.
* drops in model performance can occur with highly dimensional data (curse of dimensionality). so we will practice scaling feats using pySpark
* we dont have the original labels to produce evaluation metrics
* we import and create spark session
* we import the data `dataset = spark.read.csv("seeds_dataset.csv",header=True,inferSchema=True)`
* we check the schema and we have vaious numeric columns of feats. the feats are about seeds. they represent three diferent varities of wheats (Khama, Rosa, Canadien) . they xrayed seed samples measuring their feats
* values are similar amng feats so scling is not necessary. we will do it for the learning value of it.
* there is no label for the wheats. we know there are 3 groups.
* we import KMeans `from pyspark.ml.clustering import KMeans`
* we now need to format our data so we import assembler `from pyspark.ml.feature import VectorAssembler` we check the columns to input them to the vector. we need all of them so we pass them all `assembler = VectorAssembler(inputCols = dataset.columns, outputCol='features')`
* we transform our dataset to vectors `final_data = assembler.transform(dataset)`
* we now want to scale our data so we import the scaler `from pyspark.ml.feature import StandardScaler` it works like the assembler object `scaler = StandardScaler(inputCol='features', outputCol='scaledFeatures')` scaler accepts parameters on how we want to scale (withMean or withStd)
* we now dtrain the scaler fitting our data on the scaler `scaler_model = scaler.fit(final_data)` and we transform our dataset using the scaler `final_data = scaler_model.transform(final_data)`
* we view our dataset 	`final_data.head(1)` and it has all the inital cols + the vectorized and the scaled vectorized feats' there is not much difference with the unscaled vals
* we are now ready to work on our model
```
kmeans = KMeans(featuresCol='scaledFeatures',k=3)
```
* we fit our model to teh data `model = kmeans.fit(final_data)`
* we print the wssse `print('WSSSE {}'.format(model.computeCost(final_data)))`
* we get the cluster centers `centers = model.clusterCenters()` they apply to a 7dimensional space
* we transform the data using the model to get the labels `model.transform(final_data).select('prediction').show()`

### Lecture 53 - KMeans Clustering Consulting Project

* a technology start-up in California needs our services
* they ve been hacked and they need our help to find out about the hackers
* the forensic engineers have grabbed valuable data about the hacks inclusding info about session time, locations, wpm typing speed etc
* the forensic engineer relates what they have gathered so far. they have grabbed meta-data of each session that the hackers used to connect to their servers
* the technology firm has 3 potential hackers that did the attack. they are certain of the first 2 but they aren't very sure if the third was involved orr not
* they want us to figure out if the third suspect had anything to do with the attack or it was just 2 hackers.
* the forensic engineer knows the hackers trade off attacks. they should each have roughly the same ammount of attacks

## Section 15

### Lecture 55 - Introduction to Recommender Systems

* we will learn how to build a recommender system with Spark and Python
* there is no Consulting Project or Documentation Example in this section. the eas of use of Spark doe not lend itself to be tested on the subject
* the challenge of recommender systems is not on running the model but getting the data and organizing the data or building an app to get the data. this is out of scope of the course
* what Spark can do is to take the formatted data and quickly build the recommender system
* for further theoretical insigight see the Recommender Systems book by jannach and Zanker
* theory is explaied also in our PythonDSMLBootcamp notes
* Fully developed and deployed recommendation systems can be complex and resource intensive
* Usually we would put someone with previous experience on the subject implemente a production recommendation system
* even companies that rely heavily on recommender systems (Netflix) dont get it right in the first try
* Netflix changed mny times its system from stars rating to like/dislike and then a percentage of recommendation
* Full recommender systems require a heavy linear algebra background. this lecture will provide a high level overview
* the 2 basic types are Content Based and Collaborative Filtering (check notes of previous course)
* These technbiques aim to fill in the missing entries of a user-item association matrix
* spark.ml currently supports model-based collaborative filtering, in which users and products are described by a small set of latent factors that can be used to predict missing entries 
* spark.ml uses the alternating least squares (ALS) algorithm to learn these latent factors
* Our data needs to be in a specific format to work with Sparks ALS Recomendation Algorithm
* ALS is a Matrix Factorization approach. To implement a recommendation algorithm we decompose our large user/item matrix into lower dimensional user factors and item factors
* To fully understand this model we need strong background in linear algebra
* The intuitive uderstanding of a recommender system is the following:
	* Imagine we have 3 customers: 1,2,3
	* We also have some movies: A,B,C
	* Coustomers 1 and 2 really enjoy movies A and B and rate them 5/5 stars
	* 1 and 2 dislike movie C and give it a 1/5 star rating
	* Customer 3 comes. he havent seen any movies yes. he sees movie a and likes it rates it 5/5
	* What movie should we recommend? B or C?
	* based of collaborative filtering we recommend movie B because users 1.2  also liked that along with movie A
	* We use wisdom of the crowd
* A content based system wouldn't need to take users into account
* It would just group movies together based on feats (length,genre, actors, etc)
* Often real recommendation systems combine both methods

### Lecture 56 - Recommender System Code Along Project

* we will use the movielend datasetand the ALS methods from Spark
* we import and create a SparkSession
* we import the models we wil use `from pyspark.ml.recommendation import ALS`
* we also import a  Regression evaluator `from pyspark.ml.evaluation import RegressionEvaluator`
* we import our data `data = spark.read.csv( 'movielens_ratings.csv', inferSchema=True, header=True)`
* this is a stripped down dataset. for the actual dataset we would need a cluster and that would cost money
* we visuzlize our dataset. it has movie id, movie rating and userId
* movie lens has a second dataset that connects movie id with the actual movie
* we see our dataset stats with .describe() it has 1500 entries (reviews), 100 movies and 30 users
* we need to split our dfata to training and testing set as we want to evaluate our dataset in the end. `training,test = data.randomSplit([0.8,0.2])`
* when subjectivity is involved recommendation systems are hard to get right
* we create our als model. we can set many params in it. we use mostly defaults. also the lag needs 3 columns user, item and rating. in our case they are already there. `als = ALS(maxIter=5,regParam=0.01,userCol='userId',itemCol='movieId',rating='rating')`
* we create our model by fitting in thee training data `model = als.fit(training)`
* we get our predictions from the model passing int the test data `predictions = model.transform(test)`
* we visualize our predictions dataframe. it has the 3 existing column of our test datafram + a predictions column. the results are pretty wierd. we get negative va lpredictions. as we treat the ratings as continuous vals we can get negative values. our results are pretty off
* we create an evaluator to formally evaluate our model `evaluator = RegressionEvaluator(metricName='rmse', labelCol='rating',predictionCol='prediction')`
* we get our rmse `rmse = evaluator.evaluate(predictions)` is is 1.88... 1.88 out of 5 is not good
* we can blame our dataset for the error as it is very small for such a problem
* how can we use the model on a fresh user. we selct a random user and extract movieid and user id columns for his ratings `single_user = test.filter(test['userid'] == 11).select(['movieid','userid'])`
* we will now produce the prediction for these movies for the user. based on the rating we get in prediction we would recommend it to him or not `recommendations = model.transform(single_user)`
* we sort the predictions AKA recommendsations in descending order `recommendations.orderBy('prediction', ascending=False).show()` so we would recommend him movie 18 and 19
* in such systems a problem called cold start is how we treat a user that just enters the system . we tackle that by asking questions about his preferences or by trending most popular movies

## Section 16 - Natural Language Processing

### Lecture 57 - Introduction to natural Language Processing

* NLP is a field of machine learning that focuses on creating models (predictive or even unsupervised) from a text data source (straight from articles of words)
* for NLP we will just contain a single custom code along example. the MLlib documentation doesn't really have a full example and the custom code along is a larger multi-step process.
* NLP is a very large field of ML with its unique challenges and dsets of algorithms and feats. here we will scratch just the surface of it.
* NLTK library is a nNLP library from python separate from Spark.
Foundations of Statistical Natural language Processing (manning) is good theory book
* Examples of NLP
	* clustering news articles
	* suggesting similar books
	* grouping legal docs
	* analyzing consumer feedback
	* spam email detection
* Our basic process with NLP is:
	* Compile all documents (corpus)
	* featurize the words to numerics
	* compare features of documents
* Astandard way of doing this is using TF-IDF method
* Check notes from PYthonDSMLBootcamp as lecture is the same from now on
* Spark has a lot of pySpark.ml.feature tools to help out with this process and make our life easy

### Lecture 58 - NLP Tools Part 1

* we'll explore a few of the tools spark offers for dealing with text data
* we import and create a spark session
* we will now talk about tokenizers. okenization is the process of taking text and breaking it into individual terms (usually words)
* spark comes with a tokenizer class. we will use a regex tokenizer. this allows advanced tokenization based on regex
* the default regex params pattern uses delimiters or spaces to split the text into tokens. but we can pass in our own params or regex patern to control the split
* first we import it `from pyspark.ml.feature import Tokenizer,RegexTokenizer`
* we also import helper funcs `from pyspark.ml.functions import col,udf` to work on cols and create our user defined functions (udf) or lambda functions to pass in the tokenizer
* we also import some types `from pyspark.sql.types import IntegerType`
* we create a dataframe full of sentences passing in a list of tuples for  rows and a list of strings for labels 
```
sentenceDataFrame = spark.createDataFrame([
    (0, "Hi I heard about Spark"),
    (1, "I wish Java could use case classes"),
    (2, "Logistic,regression,models,are,neat")
], ["id", "sentence"])
```
* we now create a tokenzer passing in the label of cols it will work on and the output col `tokenizer = Tokenizer(inputCol="sentence", outputCol="words")`
* we also create a regex tokenizer passing the regex pattern. `regexTokenizer = RegexTokenizer(pattern='\\W', inputCol="sentence", outputCol="words")`
* we will define a udf to count the tokens `count_tokens = udf(lambda words: len(words), IntegerType())` note we need to pass in the return val datatype
* we now transform our df using the tokenizer `tokenized = tokenizer.transform(sen_df)`
* our new column words has a list of tokens. for the 3rd sentence we cannot tell the num of tokens as the sentense was comma separated. we use our newly defined func
* we create a new column for the counts `tokenized.withColumn('tokens',count_tokens(col('words'))).show()` 3rd sentence is treated as one big word is not split on comma using default tokenizer
* we use the regex tokenizer `rg_Tokenized = regexTokenizer.transform(sentenceDataFrame)` 
* we apply the same method and selction to view the counts `rg_tokenized.withColumn('tokens',count_tokens(col('words'))).show()` now the tokenization is correct
* we would like to remove common trivial words 'stopwords' to make our algo more efficient. we will use a stopword remover `from pyspark.ml.feature import StopWordsRemover`
* to test it we make a new dataframe simulating an already tokenized text df with stopwords
```
sentenceDataFrame = spark.createDataFrame([
    (0, ["I", "saw", "the", "green", "horse"]),
    (1, ["Mary", "had", "a", "little", "lamb"])
], ["id", "raw"])
```
* we make a stopword remover object specing the input and output columns `remover = StopWordsRemover(inputCol='tokens',outputCol='filtered')`
* we transfrom our dataset using the remover `remover.transform(sentenceData).show(truncate=False)` truncate=False forces spark to show the complete view
* n-grams are sequences of a toke for some integer. a squence of n tokens for  integer n
* it is used to transform input feats to n-grams. it usually takes the output of a tokenizer and the parameter n is used to define the number of terms in its n-gram
* the output will consist of a sequence of these n-grams where each one is represennted by a space delimited string  of n consecutive words
* we import the ngram `from pyspark.ml.feature import NGram`
* we pass a tokenized dataframe
```
wordDataFrame = spark.createDataFrame([
    (0, ["Hi", "I", "heard", "about", "Spark"]),
    (1, ["I", "wish", "Java", "could", "use", "case", "classes"]),
    (2, ["Logistic", "regression", "models", "are", "neat"])
], ["id", "words"])
```
* we create the ngram instance `ngram = NGram(n=2, inputCol="words", outputCol="ngrams")`
* we transfrom our dataset with the ngram and show its new column 
```
ngramDataFrame = ngram.transform(wordDataFrame)
ngramDataFrame.select("ngrams").show(truncate=False)
```
* the list of tokens is now grouped in subsentences containing n consecutve words in each possible combination. this is useful when the relationships between words matter

### Lecture 59 - NLP Tools Part 2

* we will see TF-IDF and count vectorizations
* TF-IDF is a feature vectorization used with text to reflect the importance of a term to the doc and the corpus itself
* we import HashingTF,IDF,Tokenizer `from pyspark.ml.feature import HashingTF, IDF, Tokenizer`
* we import as small dataset passing the label of the document the sentence belongs to
```
sentenceData = spark.createDataFrame([
    (0.0, "Hi I heard about Spark"),
    (0.0, "I wish Java could use case classes"),
    (1.0, "Logistic regression models are neat")
], ["label", "sentence"])
```
* we instantiate a tokenizer object `tokenizer = Tokenizer(inputCol="sentence", outputCol="words")`
* we transform our dataset with it. `words_data = tokenizer.transform(sentenceData)`
* next we want to see the term frequency using the hashing tf `hashing_tf = HashingTF(inputCol='words',outputCol='rawFeatures')`
* we transform our dataset using the hashingtf `featurized_data = hashing_tf.transform(words_data)`
* we are now ready to apply the idf using the output of tf as input `idf = IDF(inputCol="rawFeatures", outputCol="features")`
* we now use idf to fit it to our hashed tf data (train it) `idfModel = idf.fit(featurized_data)`
* then we use the model to trasnform the data `rescaled_data = idf_model.transform(featurized_data)`
* what we have as an output is a ready dataset for any ML algorithm. our dataset has the labels and the tf-idf vector with the vlaues for each vector element
* count vectorizer helps transform a collection of text documents into vectors of words counts
* we import it `from pyspark.ml.feature import CountVectorizer` and we drab a dataset to evaluate it
```
df = spark.createDataFrame([
    (0, "a b c".split(" ")),
    (1, "a b b c a".split(" "))
], ["id", "words"])
```
* we create an object `cv = CountVectorizer(inputCol="words", outputCol="features", vocabSize=3, minDF=2.0)` the params are selfexplanatory, vocabSize is the max num of words of the set and the minDF is the minimum number of documents that a word has to appear in to be included in the vocabulary so it affects the fitting process 
we fit it to our set `model - cv.fit(df)`
* we transform the dataset with it `result = model.transform(df)` 
* this si the bag of words method that we can use along tf-idf

### Lecture 60 - Natural Language Processing Code Along Project

* we will build a spam detection filter using python and spark. 
* the data set consists of human text from  a study in singapore and spam texts from UK.
* we import and create a sparksession
* we import the data `data = spark.read.csv("smsspamcollection/SMSSpamCollection",inferSchema=True,sep='\t')`
* we relabel our data `data = data.withColumnRenamed('_c0','class').withColumnRenamed('_c1','text')`
* we will now create a new length feature to our dataset 
```
from pyspark.sql.functions import length
data = data.withColumn('length',length(data['text']))
``` 
* we will now see if there is a major difference of the length in spam and ham messages
* we will use groupby and aggregates `data.groupBy('class').mean().show()`
* we see that there is clear difference. so this is a nice feature. this approach of infering usefule feats is called feature engineering
* we will now prepare our TF_IDF using the tokenizer et al. we import them `from pyspark.ml.feature import Tokenizer,StopWordsRemover, CountVectorizer,IDF,StringIndexer`
* we start with the tokenizer `tokenizer = Tokenizer(inputCol='text',outputCol='token_text')`
* then we seup the stopword remover `stop_remove = StopWordsRemover(inputCol='token_text',outputCol='stop_token')`
* we do countvectorization `count_vec = CountVectorizer(inputCol='stop_token',outputCol='c_vec')`
* we then do idf `idf = IDF(inputCol="c_vec", outputCol="tf_idf")`
* we then make our classes to numberics `ham_to_spam_numeric = StringIndexer(inputCol='class',outputCol='label')`
* we also need to assemble our feats to vector for MLlib
```
from pyspark.ml.feature import VectorAssembler
clean_up = VectorAssembler(inputCols=['tf_idf','length'],outputCol='features')
```
* we are now ready to build our model. a common classification alg for NLP is Naive Bayes we will use it 
```
from pyspark.ml.classification import NaiveBayes
nb = Naivebayes()
```
* with all these steps a pipeline is  really handy to reuse our model. we import and intantiate it passing all the steps in order to streamline our data prep. then we fit it to our set and and trasform the set with it
```
from pyspark.ml import Pipeline
data_prep_pipe = Pipeline(stages=[ham_spam_to_num,tokenizer,stopremove,count_vec,idf,clean_up])
cleaner = data_prep_pipe.fit(data)
clean_data = cleaner.transform(data)
```
* we now strip the dataset of all nonwanted cols keeping only the ones we will use in the classification algorithm `clean_data = clean_data.select(['label','features'])`
* we split our data `training,testing = clean_data.randomSplit([0.7,0.3])`
* we fit our alg tto the data `spam_predictor = nb.fit(training)`
* we transform our test data to get the results `test_results = spam_predictor.transform(testing)`
* we use the multiclass evaluator to ge the accuracy metric
```
from pyspark.ml.evaluation import MulticlassClassificationEvaluator
acc_eval = MulticlassClassificationEvaluator()
acc = acc_eval.evaluate(test_results)
```

## Section 17 - Spark Streaming with Python

### Lecture 61 - Introduction to Streaming with Spark

* Spark Streamin is an extension of the core Spark API that enables scalable,high-throughput, fault-tolerant stream processing of live data streams
* Data can be ingested from many sources like [Kafka](https://kafka.apache.org/), [Flume](https://flume.apache.org/), [Kinesis](https://aws.amazon.com/kinesis/) or TCP sockets
* Data can be processes using complex algorithms expressed with high-level functions like map,reduce,join and window
* so we get a data stream from a source and using Spark Streaming we perform ML on it outputing the results to a HDFS,DB or Dashboards
* Internally Spark Streaming receives live input data streams and divides the data into batches, which are then processed by the Spark engine to generate the final stream of results in batches
* we have: Input Data Stream => Spark Streaming => batches of input data => Spark Engine => batches of preocessed data
* For this course we will first work through a simple streaming example
* We will need to simultaneously use jupyter notebook and a terminal (to stream as input)for this
* the easiest way to follow is using a local installation in a virtual box
* after that we will do a twitter analysis project
* to follow along we will need visualization libs and a twitter developers account
* Realistically streaming through actual data sources (Kafka,Flume,Kinesis) cannot be shown in a single computer setting
* if our project necessitates using these sources, Spark provides integration guides
* not every version of ka source (e.g kafka) is available with a python API
* [Structured Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) uses normal spark sessions for streaming
* it uses spark streaming on top of spark sql so we can use dataframe api. it's the way for streaming
* each new data in the stream in a new row in an unbounded table (aka dataframe)
* so the input data files in streaming are unbounded. also there are triggers when new data arrive

* [Spark Streaming](https://spark.apache.org/docs/latest/streaming-programming-guide.html) is the most stable version
* in next lecture we will do the quick example from the spark streaming docs   where we will use stremainbg context and spark context. 
* spark context was the way to use spark before v2 and spark.sql and the dataframes and sparksession
* Spark streaming supports MLlib and input from Dstreams and Receivers
* Doc talks about advanced resources. all sources now have a python API

### Lecture 62 - Spark Streaming Documentation Example

* because we use Spark Streaming not Stuctured Streaming we need to use the older RDD syntax
* this is because we have to use SparkContext instead of SparrkSession
* we will build a very simpple app that connects to a local stream of data (a terminal) through a socket connection
* it will then count the words for each line we type in
* THe steps fo doing the streaming will be:
	* Create a SparkContext
	* Create a StreamingContext
	* Create a Socket Text Stream
	* Read in the lines as a DStream
* THe steps for working with the data:
	* Split the input line into a list of words
	* Map each word to a tuple (word,1)
	* THen group (reduce) the tuples by the word (key) and sum up the second argument
	* that will then provide us with a word count in the form ('hello',3) for each line
	* As a quick note, the RDD syntax relies heavily on lambda expressions
* we open 2 terminals and a jupyter notebook in our remote instance (we use EC2 with 2 ssh terminals and a web based jupyter)
* in the first terminal we run jupyter notebook and in the second we will do the streaming
* we create a new notebook
* we import SparkContext `from pyspark import SparkContext`
* we import a StreamingContext `from pyspark.streaming import StreamingContext`
* we create anew context specifying its name and that we will work with 2 local threads `sc = SparkContext("local[2]", "NetworkWordCount")`
* we use the context to create a stremingcontext setting the stream interval to 1 (second) `ssc = StreamingContext(sc, 1)`
* we use the streaming context to create a Dstream (socket) that will connect to a localhost port `lines =  ssc.socketTextStream('localhost':9999)`
* we will use this line object to create alist of words using flatMap `words = lines.flatMap(lambda line: line.split(' '))`
* with the list of words in hand we will now create the tuples with the counter set to 1 `pairs = words.map(lambda word: (word,1))`
* with the pairs in hand we can now get our wordcounts `word_counts = pairs.reduceByKey(lambda num1,num2: num1+num2)` reduce by key finds tuples withsame key and performs an operation in their second element. its is common in RDD that tuples are common
* we do prettyprint of our wordcounts `word_counts.pprint()`
* we now go to our opent terminal an run `nc -lk 9999` openning a socket comm to port 9999. the terminal is now ready to accept input
* to start the streaming context we do `ssc.start()`
* now our notebook spits the rstreming input . when we write a sentence in the terminal and hiot enter it gets anaylzed by spark in rela time SWEET>>>

### Lecture 63 0 Spark Streaming Twitter Project Part 1

* now its time for our project.
* we ll create asimple application that plots out the popylarity of tags associated with incoming tweets streamed live from Twitter
* to do this we need to create a Twitter Developer Account to get our access codes
* then we need to install the *twwepy* lib as well as matplotlib and seaborn for visualization
* we go to [Devs Twitter][apps.twitter.com]
* we sign in with our twitter account