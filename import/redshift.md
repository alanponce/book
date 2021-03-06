# Amazon Redshift Database Data Import

You can quickly import data from your Amazon Redshift Database into Exploratory.

Here is a [blog post](https://blog.exploratory.io/exploratory-data-analysis-for-amazon-redshift-with-r-dplyr-9a14441020eb#.aqcbfa6h8) introducing this support in detail.

## 1. Create a connection for Amazon Redshift database

First, you want to create a connection for Amazon Redshift database.

![](images/redshift-3-1.png)

After filling the database information, click ‘Test Connection’ button to make sure the information is correct, before you save it.

![](images/redshift-2-1.png)


## 2. Open Redshift Import dialog

Select 'Import Remote Data' from Add New Data Frame menu.

![](images/import-remote-data.png)

Click Redshift to select.

![](images/redshift.png)


## 3. Preview and Import

Click Preview button to see the data back from your Redshift db.

![](images/redshift3.png)

If it looks ok, then you can click 'Import' to import the data into Exploratory.

## 4. Querying Random Sample Data

You might want to take a random sample of the data that would be reasonable size for your analysis.

You can use [md5](http://docs.aws.amazon.com/redshift/latest/dg/r_MD5.html) function to get random number generated and use it like below to get the random sample of the data.

```
SELECT *
   FROM airline_2016_01
   ORDER BY md5('randomSeed' || flight_num)
   LIMIT 100000
```

## 5. Using Variables in SQL

First, define a variable in R script file.

```
cutoff_date <- "\'2016-01-15\'"
```

Note that the ‘\’ (backslash) symbols are used to escape the single quotes, which are required to be used for characters in SQL queries.

Second, load the R script file.

![](images/variable-1.png)

![](images/variable-2.png)

Finally, you can use @{} to surround a variable name inside the query like below.

```
select *
from airline_2016_01
where fl_date > @{cutoff_date}
```

Here's a [blog post](https://blog.exploratory.io/using-variables-in-sql-query-2740924d9f20#.bdcn5v68x) for more detail. 

## 6. AWS Security Group Setup

![](images/redshift4.png)

If you encounter a database connection error, please go to AWS console and make sure you added your client PC's IP address to your Security Group (both Inbound and Outbound) associated with the Redshift cluster.
