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

### Lecture 37 - Linear Regression Example COde Along

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
