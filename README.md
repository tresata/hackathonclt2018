Hackathon 2018
==============

Welcome to hackathonCLT 2018!

Download the kickoff deck for more information on the business problem, the hackathonCLT agenda, the data, and more! Find it in the data folder: [HackathonCLT 2018 kickoff deck](https://github.com/tresata/hackathon2018/blob/master/data/hackathon_kickoff_deck2018.pdf)

Table of Content: 

* [IMPORTANT INFORMATION](#important-information)
* [Getting Started](#getting-started)
* [Machines](#machines)
* [HDFS](#hdfs)
* [Hive](#hive)
* [Spark](#spark)
* [pySpark](#pyspark)
* [Anaconda Python](#anaconda-python)
* [Tresata Software](#tresata-software)
* [Resource Manager](#resource-manager)
* [PuTTY](#putty)
* [Data](#data)
* [Data Links](#data-links)

## IMPORTANT INFORMATION

* Please make sure you spread out on all the boxes. There are 4 servers available for login, make sure you spreadout and not all login to 1 box. 
* Use tmux once you login into the server. If not, your session could get terminated and you can lose your work. Just type "tmux" once you login. To re-attach a tmux session "tmux attach". https://tmux.github.io/

## Getting Started

You can obtain a username and login information from one of the Tresata representatives or from sponsor's room (downstairs).

ssh into a server where you can access the data.

    > ssh <username>@hack02.nscom.com OR
    > ssh <username>@hack03.nscom.com OR
    > ssh <username>@hack04.nscom.com OR
    > ssh <username>@hack05.nscom.com
    

and enter the password you were given.

We made Hive, Spark, pySpark, R and Anaconda Python command-line interfaces available, and included a tool to compile and run simple Scalding or Spark scripts on-the-fly.

## Machines

We have a Hadoop cluster with one master and four workers. The workers have 32 cores, 6 X 1TB data drives, and 128GB of RAM each. You will have ssh access to the workers.

Please spread yourselves out across the machines.

## HDFS
To access your HDFS location, you need to use hadoop fs commands (some reference: http://www.folkstalk.com/2013/09/hadoop-fs-shell-command-example-tutorial.html). For example, to take a look at your home directory on HDFS, use

    $ hadoop fs -ls

or

    $ hadoop fs -ls /user/username

**HDFS**

You can find the pre-downloaded data on HDFS in the /data/hackathon folder:

    /data/hackathon/housing
    /data/hackathon/education
    /data/hackathon/economics
    /data/hackathon/health
    /data/hackathon/transporation
    /data/hackathon/demographics
    /data/hackathon/crime

## Hive

Give Hive a whirl and run a sample query:

    $ hive

Try pasting the following query into the hive command-line interface:

    hive> show tables;
    OK
    active_match_details_new
    active_youth_outcome_reports_new
    all_child_volunteer_keys
    all_match_details_new
    all_match_details_old
    all_youth_outcome_reports_new
    all_youth_outcome_reports_old
    question_ids
    unsuccessful_match_details_new
    unsuccessful_match_details_old
    unsuccessful_youth_outcome_reports_new
    
    hive> set hive.cli.print.header=true;
    hive> select * from qol-education.csv limit 10;

This will return all the fields for the first ten items in the active_match_details_new table.

If you'd like to create a file from the command line, you can use a create table command:

    hive> create table test row format delimited fields terminated by '|' stored as textfile as select * from qol-education.csv limit 10;
    
We are also running hive-server on hack01.nscom.com. You can connect to them with JDBC/ODBC. For example to connect to hack01 with JDBC you would use this connect string:

    jdbc:hive2://hack01.nscom.com:10000
    
If you need to provide a username and password, use the username we provided for SSH login and leave the password blank. 

## Spark

**Spark-shell** can be found at /usr/local/lib/spark/bin

Now give the Spark-shell a test:

    $ /usr/local/lib/spark/bin/spark-shell --executor-cores 1 --executor-memory 1G


Read in the data and run a simple query that calcuates the unique count of ChildZip:

    val df = spark.sqlContext.read.parquet("/data/hackathon/education/mecklenburg-quality-of-life-survey/csv/qol-education.csv")
    df.groupBy("NPA").count().collect()


Note that for your "production" run on the dataset you might want to increase resources used on the cluster:

   --executor-memory 4G --executor-cores 4

Keep in mind that a spark-shell takes up these resources on the cluster even when you do not use them so please do not keep a spark-shell with "production" resources open unused.  


## pySpark

**pySpark** can be found at /usr/local/lib/spark/bin


You can also do the same query using a python version of the Spark shell.

    $ /usr/local/lib/spark/bin/pyspark --executor-cores 1 --executor-memory 1G

Read in the data and run a simple query that calcuates the unique count of ChildZip:

    df = sqlContext.read.parquet("data/hackathon/education/mecklenburg-quality-of-life-survey/csv/qol-education.csv")
    df.groupBy("NPA").count().collect()

Note that for your "production" run on the dataset you might want to increase resources used on the cluster:

    --executor-memory 4G --executor-cores 4

Keep in mind that a pyspark takes up these resources on the cluster even when you do not use them so please do not keep a pyspark shell (interpreter) with "production" resources open unused.  


## Anaconda Python
Anaconda is a completely free Python distribution from [Continuum Analytics](https://www.continuum.io). It includes more than 400 of the most popular Python packages for science, math, engineering, and data analysis. See [the packages included with Anaconda](http://docs.continuum.io/anaconda/pkg-docs).

**Anaconda** can be found here:

    /usr/local/lib/anaconda

Getting failimar with conda: http://conda.pydata.org/docs/using/index.html


## Tresata Software

#### TREK
Data Inventory Engine built specifically to catalog, profile and report data ontology, quality and format attributes for all data in Hadoop. TREK rapidly profiles and inventories “as-is” data stored in Hadoop across all rows and columns to create an informed view of all valuable enterprise data feeds stored in a single Hadoop cluster.

TREK can be accessed via http://hack01.nscom.com:5603

For login, it's the same username and password you use or SSH.

## YARN Resource Manager

http://hack01.nscom.com:8088/

## PuTTY
Here is the link to download puTTY for remote access to the data files. This is useful if you have a Windows computer. The download link is:

https://www.putty.org/

## Data

**Data Dictionary** : The links to the public source data dictionaries can be found in the next section. For any further questions please use the DATA slack channel.

**LOCAL**
You can find the data on local (all machines) in the /srv/data directory. We have provided the csv and parquet versions of the pre-downloaded files. 

```
├── crime
│   └── mecklenburg-quality-of-life-survey
│       ├── csv
│       └── parq
├── demographics
│   ├── census
│   │   ├── csv
│   │   └── parq
│   ├── mecklenburg-public-services-geojson
│   └── mecklenburg-quality-of-life-survey
│       ├── csv
│       └── parq
├── economics
│   ├── business-patterns
│   │   ├── bsv
│   │   └── parq
│   ├── consumer-financial-protection-bureau
│   │   ├── consumer-complaints
│   │   │   ├── csv
│   │   │   │   └── consumer-complaints.csv
│   │   │   └── parq
│   │   ├── financial-well-being-survey
│   │   │   └── csv
│   │   ├── home-mortgage-disclosure-act
│   │   │   ├── csv
│   │   │   └── parq
│   │   └── national-mortgage-rates
│   │       ├── csv
│   │       └── parq
│   └── mecklenburg-quality-of-life-survey
│       ├── csv
│       └── parq
├── education
│   ├── graduation-counts-including-summer-school
│   │   ├── csv
│   │   └── parq
│   ├── mecklenburg-quality-of-life-survey
│   │   ├── csv
│   │   └── parq
│   ├── nc-student-counts-grade-race-sex
│   │   ├── csv
│   │   └── parq
│   ├── principles-report-nc
│   │   ├── csv
│   │   └── parq
│   └── school-to-geocode-mapping
│       ├── csv
│       └── parq
├── geo-mappings
│   └── csv
├── health
│   ├── clean-air-carolinas
│   │   ├── csv
│   │   └── parq
│   ├── mecklenburg-quality-of-life-survey
│   │   ├── csv
│   │   └── parq
│   └── npa-mosquito-data
│       ├── bsv
│       └── parq
├── housing
│   ├── charlotte
│   ├── HUD
│   │   ├── affh
│   │   │   ├── mecklenburg
│   │   │   │   ├── csv
│   │   │   │   └── parq
│   │   │   ├── national
│   │   │   │   └── csv
│   │   │   └── raw-affh-data
│   │   └── fair-market-value
│   │       ├── bsv
│   │       └── parq
│   ├── mecklenburg-quality-of-life-survey
│   │   ├── csv
│   │   └── parq
│   └── zillow
│       ├── csv
│       └── parq
└── transportation
    └── mecklenburg-quality-of-life-survey
        ├── csv
        └── parq
```

## Data Links

**Housing**

[Housing and Urban Development](https://www.hudexchange.info/resource/4868/affh-raw-data/)

[Zillow](https://www.zillow.com/research/data/)

[Mecklenburg Quality of Life](https://mcmap.org/qol/)

[Census](https://factfinder.census.gov/faces/nav/jsf/pages/download_center.xhtml)

[Housing Choice Vouchers](https://egis-hud.opendata.arcgis.com/datasets/8d45c34f7f64433586ef6a448d00ca12_0?geometry=97.375%2C12.813%2C71.887%2C54.725)

[National Housing Preservation Database](http://preservationdatabase.org/)

**Economics**

[Consumer Financial Protection Bureau](https://www.consumerfinance.gov/data-research/mortgage-performance-trends/download-the-data/)

[Business Patterns](https://lehd.ces.census.gov/data/j2j_beta.html)

[Tax Parcel with CAMA Data](http://maps.co.mecklenburg.nc.us/openmapping/data.html)

**Education**

[North Carolina School Board](http://www.ncpublicschools.org/fbs/accounting/data/)

[Survey on US Public Schools](https://catalog.data.gov/dataset/2010-school-survey-on-crime-and-safety)

**Transportation**

[Environmental Protection Agency](https://catalog.data.gov/dataset/walkability-index)

[Mecklenburg Quality of Life](https://mcmap.org/qol/)

**Health**

[500 Cities Healthcare Data](https://catalog.data.gov/dataset/500-cities-census-tract-level-data-gis-friendly-format)

[Healthcare Cost & Utilization Project](https://www.ahrq.gov/research/data/hcup/index.html)

[Medicare Provider Utlization & Payment Data](https://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/Medicare-Provider-Charge-Data/Inpatient.html)

**Crime**

[Mecklenburg Quality of Life](https://mcmap.org/qol/)

[Charlote Metro PD](http://charlottenc.gov/CMPD/Safety/Pages/CrimeStats.aspx)

**Demographics**

[Census](https://factfinder.census.gov/faces/nav/jsf/pages/download_center.xhtml)
    
