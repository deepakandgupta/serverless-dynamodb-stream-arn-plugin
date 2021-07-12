 ### serverless-dynamodb-stream-arn-plugin
___
Fetches and then adds existing DynamoDB Table streams to serverless.yml file using table name for serverless framework

This plugin calls AWS Describe Streams on you behalf and adds the stream to serverless config file.

***

How to use

1. Add to your serverless config plugins object

```

plugins: 
  - other plugins
  - serverless-dynamodb-stream-arn-plugin

```

2. Call the function on your stream arn key like below

`arn: ${fetchStreamARN(<tableName>)}`

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

The plugin will automatically be called on command `serverless deploy`

Note: 
1. Depending on your Iam Role, you might also be required to add this in your serverless config

```
        - Effect: Allow
          Action:
            - "dynamodb:ListStreams"
          Resource: '*' 
```
`Resource property can be more specific for example`
`arn:aws:dynamodb:${self:provider.region}:*:table/*/stream/*`

2. Passing region where the table exists is important, 

    1. The plugin automatically takes the value of region from `serverless -> provider -> region`
    2. Can also be passed as region `--region <my region>` in command line
    3. Add region by passing it as the second parameter to the fetchStreamArn Function `arn: ${fetchStreamARN(<tableName>, <region>)}`