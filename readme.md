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
