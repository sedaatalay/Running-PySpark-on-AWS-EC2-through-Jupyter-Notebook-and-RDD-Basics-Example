# Running PySpark on an AWS EC2 through Jupyter Notebook and create RDD Basics Example
<p> <br/ >
 
### 1- Login to AWS and launch an instance
<img width="1234" alt="Ekran Resmi 2022-02-27 20 40 21" src="https://user-images.githubusercontent.com/91700155/155900685-8e6362b9-c33b-46e3-aa73-eb1f055f87f8.png">

### 2- Select Ubuntu Server 18.04 LTS
<img width="1407" alt="Ekran Resmi 2022-02-27 20 41 24" src="https://user-images.githubusercontent.com/91700155/155900701-9728d47c-8905-41f1-b72c-108ed8c4f33b.png">

### 3- Choose an Instance Type (Choose recommended)
<img width="1429" alt="Ekran Resmi 2022-02-27 20 41 58" src="https://user-images.githubusercontent.com/91700155/155900718-151b29a2-e149-4c28-8b02-c5857aebf1b6.png">

### 4- The instance Auto-assing Public IP has to be enable
<img width="1431" alt="Ekran Resmi 2022-02-27 20 42 23" src="https://user-images.githubusercontent.com/91700155/155900732-48b5faad-8d83-4319-974a-fa72fcb4637d.png">

### 5- For larger size installations we can add additional volume
<img width="1428" alt="Ekran Resmi 2022-02-27 20 42 58" src="https://user-images.githubusercontent.com/91700155/155900738-9e03df14-ff0d-4dd5-837d-dc1d18a480cc.png">

### 6- Make sure the security group type has all traffic allowed to inbound and outbound that you need
<img width="1164" alt="Ekran Resmi 2022-03-07 12 01 24" src="https://user-images.githubusercontent.com/91700155/157004601-694e2d72-cfb1-4956-a2b0-654b7509f642.png">

### 7- Create a Private Key file (.pem)
<img width="1421" alt="Ekran Resmi 2022-02-27 20 46 18" src="https://user-images.githubusercontent.com/91700155/155900755-7e00a8c4-3c11-4e8d-a272-9360948cf862.png">

### 8- Launch the Instance
<img width="1423" alt="Ekran Resmi 2022-02-27 20 46 38" src="https://user-images.githubusercontent.com/91700155/155900765-25266061-7b5b-40cf-90d9-c3711367a78b.png">

### 9- Change the permission of Private Key
```console
chmod 400 <pem file name>.pem
```
### 10- Connect using SSH
#### The -L specifies that the given port on the local (client) host will be forwarded to the given host and the port on the remote side #### (AWS). This means that anything running on the second port number (i.e. 8888) on AWS will appear on the first port number (i.e. 8000) #### on your local computer. You have to change 8888 to the port where Jupyter Notebook is running.
```console
ssh -i <pem file name> ubuntu@public-dns-name
```
<img width="947" alt="Ekran Resmi 2022-03-07 12 28 58" src="https://user-images.githubusercontent.com/91700155/157004677-96f667a7-f4a3-42ab-9561-1b65318c926e.png">
 
### 11- Update the packages
```console
sudo apt-get update
sudo apt-get upgrade
```
 
### 12- Installing Pip & JAVA 
#### Download Pip
```console
sudo apt install python3-pip
```
#### Check Pip version
```console
pip3 --version
```
#### Download Java
```console
pip3 install py4j
sudo apt-get install openjdk-8-jdk
``` 
 
### 13- Installing Jupyter Notebook
```console
sudo apt install jupyter-notebook
```
#### Configuring Jupyter Notebook settings
```console
jupyter notebook --generate-config
```
#### Jupyter Config File
```console
cd .jupyter
nano jupyter_notebook_config.py
```

#### Inrecase the "#c.NotebookApp.iopub_data_rate_limit" value to "100000000" and remove the # from the front.
```console
c.NotebookApp.iopub_data_rate_limit = 100000000
```
<img width="946" alt="Ekran Resmi 2022-03-07 12 18 15" src="https://user-images.githubusercontent.com/91700155/157008480-f1fdf0c2-f4da-45aa-9243-2b96cd99ea8c.png">

 
### 14- Installing Spark
#### Make sure you have Java 8 or higher installed on your computer and then, visit the Spark downloads page(https://spark.apache.org/downloads.html).
 
```console
pip3 install findspark
wget https://archive.apache.org/dist/spark/spark-3.0.1/spark-3.0.1-bin-hadoop2.7.tgz 
```
#### Unzip it and move it to your /opt folder 
```console
tar xvf spark-*
sudo mv spark-3.0.1-bin-hadoop2.7 /opt/spark-3.0.1
```
#### Create a symbolic link
```console
sudo ln -s /opt/spark-3.0.1 /opt/spark
```

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
#### After saving path, you need to run
```console
source ~/.bashrc
```

### 15- Connecting the Jupyter Notebook from your Web-Browser 
```console
pyspark
```
##### -As you can see, there is a URL given in the last line. Copy the contents of the URL after token=, i.e. c6f2835c731caf65c9413a866113c015ae2a589c0a9abc31 in my case.
 
<img width="921" alt="Z8G5B" src="https://user-images.githubusercontent.com/91700155/157012433-24514ed4-2328-4680-bda8-833acfecc183.png">

### 16- Jupyter Notebook Web UI
```console
https://<your public dns>:8888/
```
<img width="1407" alt="Ekran Resmi 2022-03-07 12 20 07" src="https://user-images.githubusercontent.com/91700155/157009243-2b3cab16-fb6c-446a-8da2-f6748935602f.png">

##### -Paste the copied token and create a new password if you want
<img width="686" alt="Ekran Resmi 2022-03-07 12 22 09" src="https://user-images.githubusercontent.com/91700155/157012388-7ae3b1e5-8a1b-4b7e-96dd-1b2ce5935bc1.png">


#### To stop jupyter notebook services
```console
jupyter notebook stop 8888
```

### 18- Create Sample Rdd Examples
#### Upload a sample file to your jupyter notebook. (i.e. .txt, .csv, .parquet..). I choose .txt file. Then open new python3 file.
 

##### All we need is to install findspark package via pip install findspark. Then, on Jupyter, simply import Apache Spark via
```console
import findspark
findspark.init()
import pyspark
from pyspark.sql import SparkSession
from pyspark import SparkConf, SparkContext
```
 
#### RDD Basics
##### The RDD API is the most basic way of dealing with data in Spark. RDD stands for “Resilient Distributed Dataset.” Although more abstracted, higher-level APIs such as Spark SQL or Spark dataframes are becoming increasingly popular, thus challenging RDD’s standing as a means of accessing and transforming data, it is a useful structure to learn nonetheless. One salient feature of RDDs is that computation in an RDD is parallelized across the cluster.

<p>  <br />
</p>
 
##### Spark Context
###### A Spark context is the entry point to Spark that is needed to deal with RDDs
```console
conf = SparkConf().setMaster("local").setAppName("app")
sc = SparkContext.getOrCreate()
spark = SparkSession.builder.getOrCreate()
```

##### We convert the uploaded text file to Rdd.
```console
rdd = sc.textFile(f'BTC-GBP.txt')
rdd.take(4)
``` 
 
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
##### Flatmap
###### flatMap(). For this example, we load a text file containing prime numbers and create a RDD.
```console
flatmap = rdd.flatMap(lambda line: line.split(' ')).map(str)
print(flatmap.take(5))
``` 
 
#### Pair RDD
##### Let’s turn our attention to another type of widely used RDDs: pair RDDs. Pair RDDs are widely used because they are, in a way, like dictionaries with key-value pairs. 
 
##### -Map
###### We might want to apply some map function on the values of the RDD while leaving the keys unchanged.
```console
rdd2=rdd.map(lambda x: (x,1))
for element in rdd2.collect():
    print(element)
``` 
 
##### -Key/Value Pairs
```console
flatmap = rdd.flatMap(lambda line: line.split(' ')).map(str)
first = flatmap.mapValues(lambda value: value[0])
print(first.take(10))
``` 

##### -ReduceByKey
###### .reduce() is a way of reducing a RDD into something like a single value. The equivalent for pair RDDs is reduceByKey().
```console
flatmap_rdd = rdd.flatMap(lambda line: line.split(' ')).map(str)
raw_pair = flatmap_rdd.map(lambda word: (word, 1))
pair_nums = raw_pair.reduceByKey(lambda x, y: 1 + y)
print(pair_nums.take(5))
``` 



 
<p>  <br />
</p>

### Seda Atalay
