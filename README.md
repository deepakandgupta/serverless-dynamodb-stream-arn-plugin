## Fetches and then adds existing DynamoDB Table streams to serverless.yml file using table name for serverless framework

This plugin calls AWS Describe Streams on you behalf and adds the stream to serverless config file.

### How to use

#### 1. Add to your serverless config plugins object

#### 2. Call the function on your stream arn key like below

> arn: ${fetchStreamARN(<tableName>)}

Code Example: 
```
functions:
  sourceLambda:
    handler: ./src/functions/dynamodbOperation
    events:
      - stream:
          arn: ${fetchStreamARN(<tableName>)} // This is all the change you need to do
          startingPosition: LATEST
          enabled: true // Any other parameters can be added as provided by serverless
```

Note: Passing region where the table exists is important, the plugin automatically takes the value from `serverless -> provider -> region`

This can be overwritten by passing region in command line property or
Optionally you can add region of where to fetch dynamodb tables by passing it as the second parameter to the fetchStreamArn Function 

> arn: ${fetchStreamARN(<tableName>, <region>)}