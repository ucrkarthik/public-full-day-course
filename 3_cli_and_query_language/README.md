# 3. Working with the CLI and Query Language (45-60 min) 11:00-12:00

* Working with the InfluxDB CLI
* The InfluxDB Query Language

## By the end of this section students will be able to...

* Query InfluxDB using the InfluxDB CLI.
* Understand the structure of the data that is returned from a query.
* Articulate what InfluxQL can do.
* Create novel queries of their own.

## Quiz (20 min) 12:00-12:20
* Querying Data (Project)


## Write the data to your instance.
```
$ curl https://s3-us-west-2.amazonaws.com/influx-sample-data/stocks.txt > stocks.txt
$ influx -import -path=stocks.txt -precision=s
```

The data that we've loaded in the data for the SP500 from 2013, but where the timestamps have been adjusted to the current time.

Before you begin please set the `precision` to be `rfc3339`


#### 1. What is the schema of the data we installed on your instance?
> show measurements
name: measurements
------------------
name
stock_price

> show tag keys
name: stock_price
-----------------
tagKey
ticker

> show field keys
name: stock_price
-----------------
fieldKey
high
low
open
volume



##### Bonus: How many series are there?
24
select * into bogus from stock_price group by * limit 1


#### 2. What was the highest opening stock price in the last 80 days?
select max(open) from stock_price where time > now() - 80d

#### 3. What company had the highest opening price in the last 80 days?
select max(open), ticker from stock_price where time > now() - 80d

#### 4. What was the highest opening price for each of the last 80 days and which company had this price?
precision rfc3339
select max(open) from stock_price where time > now() - 80d and time < now() - 70d group by time(1d)


#### 5. How many of the last 110 days did the price of Google's stock (ticker='GOOG') go above $500?
select count(high) from stock_price where time > now() - 100d and ticker = 'GOOG' and high > 500
