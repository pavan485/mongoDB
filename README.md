# Nodejs-Influx
Catchpoint Integration with InfluxDB
---
We can use this integration to pull timeseries data from Catchpoint and store it in MongoDB for analysis or as a long-term test data storage solution.

This integration makes use of a Python script that runs at 15 minutes intervals to pull raw performance chart data from the Catchpoint GET: LastRaw API. It can be used to retrieve and store data for a list of tests in the same division.

### Prerequisites
---
1. Python v3.x
2. MongoDB 5.x
3. Catchpoint account with a REST API consumer

# Installation and Configuration

Copy the Python-Influx folder to your machine
Run following commands in the directory /Python-Influx
   - python -m pip install requests
   - pip install pyyaml
   - pip install logger
   - pip install pymongo
   
   
### Configuration
In the config.yaml file under config sub-directory, enter your Catchpoint API consumer key and secret
In the tests object of the config_catchpoint.yaml file, enter the test IDs you want to pull the data for in a dictionary of array format.

*Example:*

    test_ids: { 
              web : ['142619','142620','142621','142622'],
              traceroute : ['142607','142608','142609'], 
              api : ['142637','142638','142683','142689'],
              transaction: ['142602','142603'],
              dns : '142644','142645','142646','142647'],
              smtp : ['142604'],
              websocket: ['842700'],
              ping : ['142599','142600','142601']
              
          }
---       
In the config_influx.py file, enter your MongoDB url, database name and collection name where the data will be stored. The default MongoDB URL for a local installation is http://localhost:27017


### How to run

 
- Create a cronjob to run the application.py file every 15 minutes.

*Example crontab entry, if the file resides in /usr/local/bin/application.py*

`*/15 * * * * cd /usr/local/bin/ && python /usr/local/bin/application.py > /usr/local/bin/logs/cronlog.log 2>&1`


or 

- In the /python-mongo directory, run appliaction.py after uncommenting the while true: and time.sleep.

## File Structure

    Python-Mongo/
    ├── request_handler.py          ## Contains APIs related to authentication       
    ├── config
    | ├── config_catchpoint.yaml    ## Configuration file for Catchpoint 
    | ├── config_influx.yaml        ## Configuration file for mongoDB
    ├── log
    | ├── app.log                   ## Contains informational and error logs. 
    ├── application.py              ## main file
    ├── log.py
    ├── request_handler.py          ## Contains API requests for token and raw endpoint 
    ├── utils.py                    ##  utility fot partsing data, inserting it to mongoDB and validating configurations
           

Once the script starts running and data is inserted into MongoDB, it can queried using MongoDB shell or directly viewed using MongoDBCompass
