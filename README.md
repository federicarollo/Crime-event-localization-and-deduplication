# Crime event localization and deduplication
This repository contains the source code of two applications: 
1. the Crime Ingestion App is a Java application and aims at extracting, geolocalizing and deduplicating crime-related news articles from two online newspapers of the province of Modena in Italy (ModenaToday https://www.modenatoday.it/ and Gazzetta di Modena https://gazzettadimodena.gelocal.it/modena), the information extracted by the application is stored in a PostgreSQL database,
2. the Crime Visualization App is a Python application and allows visualizing in a web application the crime-related data stored in the PostgreSQL database (online version is available at the link https://dbgroup.ing.unimore.it/crimemap).

<img src="https://github.com/federicarollo/Crime-event-localization-and-deduplication/blob/master/Crime%20Visualization%20App/screen.png" width="100%" height="100%" />


The <a href="https://github.com/federicarollo/Crime-event-localization-and-deduplication/blob/master/crimedb.sql">crimedb.sql</a> file contains the structure of the PostgreSQL database in which the data of the news articles are stored (the database has to be created before running the Crime Ingestion App).

<img src="https://github.com/federicarollo/Crime-event-localization-and-deduplication/blob/master/crimedb.png" width="70%" height="70%" />


The two applications have been executed on a Microsoft Windows 10 Pro computer with Intel i7 and 16GB RAM.
However, in the following, the instructions to execute the applications on both Windows and Unix are listed.


## Crime Ingestion App

### Requirements
Java (JDK 8+)

PostgreSQL 9.6

PostGIS 2.3


### How to run the application

After installing PostgreSQL on your machine, create the database by using the <a href="https://github.com/federicarollo/Crime-event-localization-and-deduplication/blob/master/crimedb.sql">crimedb.sql</a> file.

Then, download the <a href="https://github.com/federicarollo/Crime-event-localization-and-deduplication/tree/master/Crime%20Ingestion%20App">Crime Ingestion App folder</a>.

NOTE: modify the config.json file in the configuration folder with the configuration parameters to connect to your database (username and password).

In the config.json file, the "num_of_pages" parameter represents the number of web pages of each online newspaper that will be queried to download the news articles.
If this number is equal to 1, only the news articles in the first web page of each newspaper (the most recent news articles) will be downloaded.

Unzip the tagger.zip file.

#### Windows

Run the following commands on the terminal:

<div class="highlight highlight-source-shell"><pre>
mvn clean package
xcopy configuration target /s /e
cd target
java -jar Crime_Ingestion_App-1.0.0.jar
</pre></div>

After the last command, the database will be populated with the information extracted from the online newspapers (the urls of the web pages are listed in the files modenatoday.json and gazzettadimodena.json with the type of crime of the news articles published on that newspaper).


#### Linux
Run the following commands on the terminal:

<div class="highlight highlight-source-shell"><pre>
mvn clean package
cp -R configuration target
cd target
java -jar Crime_Ingestion_App-1.0.0.jar
</pre></div>


Then, create the main.sh file with the following commands:

<div class="highlight highlight-source-shell"><pre>
#!/bin/bash

export CLASSPATH=""
b="/path to the folder/target"
cd "/path to the folder/"
for i in $b/bin $b/lib/*.jar
do
&nbsp;&nbsp;&nbsp;&nbsp;export CLASSPATH=$CLASSPATH:$i
done
echo $CLASSPATH<br>
java Main</pre></div>

Then, run the application with the command:
<div class="highlight highlight-source-shell"><pre>
bash main.sh</pre></div>

## Crime Visualization App

### Requirements
PostgreSQL 9.6

PostGIS 2.3

Python 3.5

### How to run the application
Download the <a href="https://github.com/federicarollo/Crime-event-localization-and-deduplication/tree/master/Crime%20Visualization%20App">Crime Visualization App folder</a>.

Modify the crime_visualization_app.py file adding the credentials to access to your database:

<div class="highlight highlight-source-shell"><pre>connection_string="dbname='crime_news' user='************' host='localhost' port=5432 password='***************'"</pre></div>

Install the requirements with the command:

<div class="highlight highlight-source-shell"><pre>pip install -r requirements.txt</pre></div>

Run the command:

<div class="highlight highlight-source-shell"><pre>python crime_visualization_app.py</pre></div>

Open http://localhost:9018/crimemap?request=GetCrimes on your browser and visualize the crimes!
