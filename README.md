# Running PySpark on AWS EC2 through Jupyter Notebook and RDD Basics Examples
<p> <br/ >
 
### 1- Login to AWS and launch an instance
<img width="1234" alt="Ekran Resmi 2022-02-27 20 40 21" src="https://user-images.githubusercontent.com/91700155/155900685-8e6362b9-c33b-46e3-aa73-eb1f055f87f8.png">

### 2- Select Ubuntu Server 18.04 LTS
<img width="1407" alt="Ekran Resmi 2022-02-27 20 41 24" src="https://user-images.githubusercontent.com/91700155/155900701-9728d47c-8905-41f1-b72c-108ed8c4f33b.png">

### 3- Choose an Instance Type (Choose recommended)
<img width="1435" alt="Ekran Resmi 2022-03-15 13 21 53" src="https://user-images.githubusercontent.com/91700155/158365354-220500fe-7132-434d-8efc-bb66532194a0.png">

### 4- For larger size installations we can add additional volume
<img width="1435" alt="Ekran Resmi 2022-03-15 13 23 41" src="https://user-images.githubusercontent.com/91700155/158365487-5f4289be-21c2-4b05-b33b-ca11234b6fc8.png">

### 5- Make sure the security group type has all traffic allowed to inbound and outbound that you need
<img width="1435" alt="Ekran Resmi 2022-03-15 13 24 08" src="https://user-images.githubusercontent.com/91700155/158365513-f2ee3cad-c78b-4cdd-99c3-c0c0b9990b40.png">

### 6- Create a Private Key file (.pem)
<img width="1435" alt="Ekran Resmi 2022-03-15 13 24 29" src="https://user-images.githubusercontent.com/91700155/158365566-e349753c-5816-442b-807b-6bddf11e4755.png">

### 7- Launch the Instance
<img width="1435" alt="Ekran Resmi 2022-03-15 13 24 44" src="https://user-images.githubusercontent.com/91700155/158365616-227395fb-331e-453d-bb65-8d09bbfed565.png">
<img width="1038" alt="Ekran Resmi 2022-03-15 13 26 31" src="https://user-images.githubusercontent.com/91700155/158366122-d2257a23-7d25-4bb1-aff9-f876ddc78c94.png">
 
### 8- Change the permission of Private Key
```console
chmod 400 <pem file name>.pem
```
### 9- Connect using SSH
#### The -L specifies that the given port on the local (client) host will be forwarded to the given host and the port on the remote side (AWS). This means that anything running on the second port number (i.e. 8888) on AWS will appear on the first port number (i.e. 8000) on your local computer. You have to change 8888 to the port where Jupyter Notebook is running.
```console
ssh -i <pem file name> ubuntu@public-dns-name
```
<img width="869" alt="Ekran Resmi 2022-03-15 13 28 50" src="https://user-images.githubusercontent.com/91700155/158366181-282ea2d3-a7ae-4410-93f1-1915f523bf91.png">

 
### 10- Update the packages
```console
sudo apt-get update
sudo apt-get upgrade
```
 
### 11- Installing Pip & JAVA 
#### Download pip
```console
sudo apt install python3-pip
```
<img width="869" alt="Ekran Resmi 2022-03-15 13 33 43" src="https://user-images.githubusercontent.com/91700155/158366298-a4c12745-77ce-479c-8e5e-1406e5acedd8.png">

#### Check pip version
```console
pip3 --version
```
<img width="815" alt="Ekran Resmi 2022-03-15 13 34 39" src="https://user-images.githubusercontent.com/91700155/158366318-c7e613fd-e74d-4829-b978-7eac50b452c1.png">

#### Download Java
```console
pip3 install py4j
sudo apt-get install openjdk-8-jdk
``` 
<img width="851" alt="Ekran Resmi 2022-03-15 13 36 47" src="https://user-images.githubusercontent.com/91700155/158366404-09ca8f8d-165e-45ff-898e-75014dc23ef0.png">


### 12- Installing Jupyter Notebook
```console
sudo apt install jupyter-notebook
```
<img width="869" alt="Ekran Resmi 2022-03-15 13 42 14" src="https://user-images.githubusercontent.com/91700155/158366538-5ba20124-0b18-4f0c-ab6c-163a122cb14a.png">

#### Configuring Jupyter Notebook settings
```console
jupyter notebook --generate-config
```
#### Jupyter Config File
```console
cd .jupyter
nano jupyter_notebook_config.py
```
<img width="869" alt="Ekran Resmi 2022-03-15 13 42 42" src="https://user-images.githubusercontent.com/91700155/158366579-034835c0-473f-402b-bde0-d7f4f91513e9.png">
 
#### Increase the "#c.NotebookApp.iopub_data_rate_limit" value to "100000000" and remove the # from the front.
```console
c.NotebookApp.iopub_data_rate_limit = 100000000
```
<img width="869" alt="Ekran Resmi 2022-03-15 13 43 27" src="https://user-images.githubusercontent.com/91700155/158366597-9bbd9863-0973-4599-a6e9-b1d7fe3eafe3.png">
<img width="869" alt="Ekran Resmi 2022-03-15 13 45 30" src="https://user-images.githubusercontent.com/91700155/158366686-96e3192e-d7d0-4644-9296-b228e8031733.png">

 
### 13- Installing Spark
#### Make sure you have Java 8 or higher installed on your computer and then, visit the Spark downloads page (https://spark.apache.org/downloads.html).
 
```console
pip3 install findspark
wget https://archive.apache.org/dist/spark/spark-3.0.1/spark-3.0.1-bin-hadoop2.7.tgz 
```
<img width="854" alt="Ekran Resmi 2022-03-15 13 36 54" src="https://user-images.githubusercontent.com/91700155/158366826-20dfbdf8-256d-445e-a30e-def86319b6ee.png">
<img width="869" alt="Ekran Resmi 2022-03-15 13 46 53" src="https://user-images.githubusercontent.com/91700155/158366789-f74f5bc9-9b55-4617-9f54-f9c1ad291870.png">

#### Unzip it and move it to your /opt folder and create a symbolic link
```console
tar xvf spark-*
sudo mv spark-3.0.1-bin-hadoop2.7 /opt/spark-3.0.1
sudo ln -s /opt/spark-3.0.1 /opt/spark
```
<img width="869" alt="Ekran Resmi 2022-03-15 13 57 10" src="https://user-images.githubusercontent.com/91700155/158366938-5c6ace36-c661-4a25-bec2-6888800a9038.png">

#### Configure Spark & Java & PySpark driver to use Jupyter Notebook
```console
nano ~/.bashrc
```
##### -Write into:
```console
export SPARK_HOME=/opt/spark
export PATH=$SPARK_HOME/bin:$PATH
export JAVA_HOME='/usr/lib/jvm/java-8-openjdk-amd64'
export PATH=$JAVA_HOME/bin:$PATH
export PYSPARK_PYTHON=python3
export PYSPARK_DRIVER_PYTHON=jupyter
export PYSPARK_DRIVER_PYTHON_OPTS='notebook --ip 0.0.0.0 --port=8888'
```
<img width="869" alt="Ekran Resmi 2022-03-15 13 56 39" src="https://user-images.githubusercontent.com/91700155/158367099-a6d7161b-4cc6-4d36-8134-18c9cf82bd34.png">
 
#### After saving path, you need to run
```console
source ~/.bashrc
```
<img width="869" alt="Ekran Resmi 2022-03-15 13 57 25" src="https://user-images.githubusercontent.com/91700155/158367160-11cc8f4c-5164-4616-b008-bc753b27ec6b.png">

 
### 14- Connecting the Jupyter Notebook from your Web-Browser 
```console
pyspark
```
##### As you can see, there is a URL given in the last line. Copy the contents of the URL after token=, i.e. c6f2835c731caf65c9413a866113c015ae2a589c0a9abc31 in my case.
 
<img width="869" alt="Ekran Resmi 2022-03-15 13 57 48" src="https://user-images.githubusercontent.com/91700155/158367223-9aae431f-7c07-4ee4-b8a4-11d903e04cf3.png">
 

### 15- Jupyter Notebook Web UI
```console
https://<your public dns>:8888/
```
##### -Paste the copied token and create a new password if you want
<img width="1419" alt="Ekran Resmi 2022-03-15 13 58 45" src="https://user-images.githubusercontent.com/91700155/158367261-5ebab40c-10cb-4ccc-b2be-fae41004d24b.png">

#### To stop jupyter notebook services
```console
jupyter notebook stop 8888
```

<p>  <br />
</p>
 
 
###  Create Sample Rdd Examples
#### 1- Upload a sample file to your jupyter notebook. (i.e. .txt, .csv, .parquet..). I choose .txt file. Then open new python3 file.
 
<img width="1170" alt="Ekran Resmi 2022-03-15 14 01 11" src="https://user-images.githubusercontent.com/91700155/158367302-dd1525a9-7b46-4147-a49e-bd752a477ba0.png">
<img width="224" alt="Ekran Resmi 2022-03-15 14 02 19" src="https://user-images.githubusercontent.com/91700155/158367331-b4155f4e-ee49-4343-8848-0789b949d5c3.png">

##### All we need is to install findspark package via pip install findspark. Then, on Jupyter, simply import Apache Spark via
```console
import findspark
findspark.init()
import pyspark
from pyspark.sql import SparkSession
from pyspark import SparkConf, SparkContext
```
<img width="1026" alt="Ekran Resmi 2022-03-15 12 19 55" src="https://user-images.githubusercontent.com/91700155/158367368-bd673cb7-9650-4372-815b-63e2d46fd886.png">

#### 2- RDD Basics
##### The RDD API is the most basic way of dealing with data in Spark. RDD stands for “Resilient Distributed Dataset.” Although more abstracted, higher-level APIs such as Spark SQL or Spark dataframes are becoming increasingly popular, thus challenging RDD’s standing as a means of accessing and transforming data, it is a useful structure to learn nonetheless. One salient feature of RDDs is that computation in an RDD is parallelized across the cluster.

##### Spark Context
###### A Spark context is the entry point to Spark that is needed to deal with RDDs
```console
conf = SparkConf().setMaster("local").setAppName("app")
sc = SparkContext.getOrCreate()
spark = SparkSession.builder.getOrCreate()
```
<img width="1015" alt="Ekran Resmi 2022-03-15 12 20 00" src="https://user-images.githubusercontent.com/91700155/158367420-e6afd8f1-7649-4355-9cc9-5ca416b53594.png">

##### We convert the uploaded text file to Rdd.
```console
rdd = sc.textFile(f'BTC-GBP.txt')
rdd.take(4)
``` 
<img width="1017" alt="Ekran Resmi 2022-03-15 12 20 07" src="https://user-images.githubusercontent.com/91700155/158367551-c51a3a9e-bd56-45e6-b742-e13d0af1fc02.png">
 
##### We use .collect() to turn the RDD into an iterable, specifically a list
```console
rdd_list = rdd.collect()
words = sc.parallelize(rdd_list)
``` 
##### -Count, CountbyValue
###### Another useful function is .count() and .countByValue(). As you might have easily guessed, these functions are literally used to count the number of elements itself or their number of occurrences. 
```console
countByValue = words.countByValue()
print(countByValue)
``` 
<img width="990" alt="Ekran Resmi 2022-03-15 12 24 58" src="https://user-images.githubusercontent.com/91700155/158367808-47c31775-415c-49e6-b0ad-e34a627c0b45.png">

 
##### Flatmap
###### flatMap(). For this example, we load a text file containing prime numbers and create a RDD.
```console
flatmap = rdd.flatMap(lambda line: line.split(' ')).map(str)
print(flatmap.take(5))
``` 
<img width="1012" alt="Ekran Resmi 2022-03-15 12 21 28" src="https://user-images.githubusercontent.com/91700155/158367933-18adfb16-513a-4484-a457-ef0976095766.png">

 
#### 3- Pair RDD
##### Let’s turn our attention to another type of widely used RDDs: pair RDDs. Pair RDDs are widely used because they are, in a way, like dictionaries with key-value pairs. 
 
##### Map
###### We might want to apply some map function on the values of the RDD while leaving the keys unchanged.
```console
rdd2=rdd.map(lambda x: (x,1))
for element in rdd2.collect():
    print(element)
``` 
<img width="987" alt="Ekran Resmi 2022-03-15 12 20 17" src="https://user-images.githubusercontent.com/91700155/158367864-74aabca4-c3ea-42b7-93a2-cbb772f5bbdf.png">

##### Key/Value Pairs
###### Key/value RDDs are commonly used to perform aggregations, and often we will do some initial ETL (extract, transform, and load) to get our data into a key/value format.
```console
flatmap = rdd.flatMap(lambda line: line.split(' ')).map(str)
first = flatmap.mapValues(lambda value: value[0])
print(first.take(10))
``` 
<img width="1019" alt="Ekran Resmi 2022-03-15 12 21 37" src="https://user-images.githubusercontent.com/91700155/158367991-95ef99e8-49f9-4f2d-b792-51fe6ae4e4a7.png">

##### ReduceByKey
###### .reduce() is a way of reducing a RDD into something like a single value. The equivalent for pair RDDs is reduceByKey().
```console
flatmap_rdd = rdd.flatMap(lambda line: line.split(' ')).map(str)
raw_pair = flatmap_rdd.map(lambda word: (word, 1))
pair_nums = raw_pair.reduceByKey(lambda x, y: 1 + y)
print(pair_nums.take(5))
``` 
<img width="1011" alt="Ekran Resmi 2022-03-15 12 22 28" src="https://user-images.githubusercontent.com/91700155/158368053-baf6c021-904c-494f-8082-26f54159a220.png">



#### Thank you :) 
<p>  <br /><br />
</p>

### Seda Atalay
