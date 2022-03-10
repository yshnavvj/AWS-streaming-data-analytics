**Table of Contents**
1.	Introduction
2.	Pipeline
3.	Environment Set Up
4.	Repository Structure and Run Instructions

**Introduction**
Retail order :Transaction rate alarm on real time with Kinesis data analytics and analysis, visualize of inflation rates of Technology sector products in near real time with Firehose, Kinesis data analytics and Athena
The goal of this project is to make explore real time streaming and near real time streaming components of AWS big data analytics stack and build the above mentioned objective.
**Pipeline**
I built a data pipeline that uses of the generated order logs, in here is running on an Amazon EC2 instance. The retail order is generated with the help of a python script running on EC2 instance and Is collected on real time with “Kinesis data streams” on which the Kinesis data analytics detects anomalies in individual record columns for a really unusual number of order quantities in the data stream by making use of RANDOM_CUT_FOREST algorithm available in the component.  
This project also calculates the inflation rates of products of a particular sector using the SQL in Kinesis data analytics and then to persist data in S3 ,use it for dashboards visualization in Amazon athena.Amazon athena and AWS glue to crawl over S3 are the serverless manages services of AWS. 

**Environment set up**
1.	Create all the Kinesis stream components from the AWS services console.
2.	Create a new Kinesis data stream and change the agent config of Kinesis agent running on EC2 instance to have the following.

{
  "cloudwatch.emitMetrics": true,
  "kinesis.endpoint": "",
  "firehose.endpoint": "", 
  
  "flows": [
    {
      "filePattern": "/var/orderlog/retail_order*.csv",
      "kinesisStream": "orders",
      "partitionKeyOption": "RANDOM",
      "dataProcessingOptions": [
         {
            "optionName": "CSVTOJSON",
            "customFieldNames": ["ticker_symbol", "sector", "unit_price", "change", "quantity", "Customer"]
         }
      ]
    }
  ]
}

3.	Set up and configure Kinesis Firehose to export results to Amazon Kinesis data analytics by changing the flows in the agent config file in the Kinesis data agent on the EC2 instance.(in the above config file)

