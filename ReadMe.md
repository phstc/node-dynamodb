## Basics

A DynamoDB driver for Node.js, with:

- Syntactic sweeteners/mapping to handle Amazon DynamoDB's JSON format in NodeJS
- Efficient and transparent authentication and authentication refresh using Amazon STS
- Currently in production with more than 80 write/s, 60 read/s

Discussion Group: http://groups.google.com/group/node-dynamodb

Supports the following operations:
   
    CreateTable
    ListTables
    DescribeTable
    GetItem
    PutItem
    DeleteItem

Any contribution is welcome! There's still a lot of work to be done on how to nicely
map the rather complex syntax of DynamoDB optional aguments into node space!

## Usage

    var ddb = require('dynamodb').ddb({ accessKeyId: '',
                                        secretAccessKey: '' });
    
    ddb.createTable('foo', { hash: ['id', ddb.types.string], 
                             range: ['time', ddb.types.number] }, 
                    {read: 10, write: 10}, function(err, details) {});
    // res: { "CreationDateTime": 1.310506263362E9,
    //        "KeySchema": { "HashKeyElement": { "AttributeName": "AttributeName1",
    //                                           "AttributeType": "S"},
    //                       "RangeKeyElement": { "AttributeName": "AttributeName2",
    //                                            "AttributeType": "N"} },
    //        "ProvisionedThroughput":{ "ReadCapacityUnits": 5,
    //                                  "WriteCapacityUnits": 10 },
    //        "TableName":"Table1",
    //        "TableStatus":"CREATING" }

    ddb.listTables({}, function(err, res) {});
    // res: { LastEvaluatedTableName: 'bar',
              TableNames: ['test','foo','bar'] }

    ddb.describeTable('a-table', function(err, res) {});
    // res: { ... }

    // flat [string, number, string array or number array] based json object
    var item = { score: 304,
                 date: (new Date).getTime(),
                 sha: '3d2d6963',
                 usr: 'spolu',
                 lng: ['node', 'c++'] };
    
    ddb.putItem('a-table', item, {}, function(err, res, cap) {});

    ddb.getItem('a-table', '3d2d6963', null, {}, function(err, res, cap) {});
    // res: { score: 304,
    //        date: 123012398234,
    //        sha: '3d2d6963',
    //        usr: 'spolu',
    //        lng: ['node', 'c++'] };

    ddb.deleteItem('a-table', 'sha', null, {}, function(err, res, cap) {});

    ddb.consumedCapacity();

More complete usage can be found in the examples directory

## Contributors

    karlseguin (Karl Seguin) [https://github.com/karlseguin]
    imekinox (Juan Carlos del Valle) [https://github.com/imekinox]