Not officially supported by Sumo Logic, work in progress, use at your own risk:
- ~~Doesn't actually create proper CSV output~~
- ~~Deal with records also (aggregation queries)~~
- ~~API frontend async~~
- ~~Should send SNS notification at the end?~~
- ~~Yucky event properties validation~~
- ~~Thread ID of the search request through all logs~~
- ~~https://github.com/ACloudGuru/serverless-plugin-cloudwatch-sumologic https://serverless.com/blog/serverless-ops-logs/~~
- API frontend that waits for X seconds and returns or returns a pointer
- Error handling
- Tests!!!

In the meantime:

```
npm install -g serverless
git clone https://github.com/SumoLogic/serverless-sumo-search-query.git
cd serverless-sumo-search-query
npm install # Ignore warnings
serverless deploy
serverless invoke -f search --data \
'{"endpoint": "[prod|us2|...]", "accessId": "[sumo-access-id]", "accessKey": "[sumo-access-key]", "query": "error | count", "from": "2019-01-19T16:00:00", "to": "2019-01-19T16:01:00", "timeZone": "America/Los_Angeles", "messages": true, "records": true, "s3Bucket": "[your-bucket]", "s3KeyPrefix": "[your-prefix]", "snsTopic": "[your-SNS-topic"}}'
```

Of course, it is possible to use the exposed API as well:

```
curl -d '{"endpoint": "[prod|us2|..", "accessId": "[sumo-access-id]", "accessKey": "[sumo-access-key]", "query": "error | count", "from": "2019-01-19T00:00:00", "to": "2019-01-19T00:00:01", "timeZone": "America/Los_Angeles", "messages": true, "records": true, "s3Bucket": "[your-bucket]", "s3KeyPrefix": "[your-prefix]", "snsTopic": "[your-SNS-topic"}' -X POST https://jjzqfhmtjl.execute-api.us-west-2.amazonaws.com/dev/search
```

Note: Need to have AWS credentials in `~/.aws/credentials` under profile `s1ss_sls` 
(or change profile name in `serverless.yml`).

Note: This is my first rodeo with Serverless and Step Functions. Please suggest more idiosyncratic
ways of doing things where appropriate.
