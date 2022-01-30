# sam-app

## Apigateway integrations

Direct api gate to dynamodb integration, based on blog post:

https://itnext.io/setting-up-api-gateway-with-dynamodb-using-cloudformation-a8ab3e70f494


## Setup

1. Setup aws credentials using aws configure
1. Copy .envrc.exampl to .envrc.
1. Edit .envrc
1. npm i
1. Run the initial deploy in guided mode with -g, edit the sam-deploy script in package json

## Testing the api using curl

***Get list of all dragons***

curl -H "content-type: application/json" ${API_URI}/dragons

***Add new dragon***

curl -X POST -d @events/new-dragon.json -H "content-type: application/json" ${API_URI}/dragons

***Fetch a particular dragon***

curl -H "content-type: application/json" ${API_URI}/dragon/013733b8-5e50-4bc6-9f9c-09d3d448313f

***Update a dragons name***

curl -H "content-type: application/json" -X PUT -d @events/update-dragon.json ${API_URI}/dragon/2

***Delete a dragon***

curl -H "content-type: application/json" -X DELETE -d @events/update-dragon.json ${API_URI}/dragon/70a3a85a-6957-4ec4-8781-c0e513e5a154


## Hints

If you don't specify json content type then you may be passing through the request payload as is.
This leads to validation errors complaining about null tableName and null item. This is because
the mapping template did not get applied.

The raw dynamodb response can be returned to aid in debugging. Use a response mapping tempalte
like this:

```
#set($inputRoot = $input.path("$"))
{
  "dynamodbResponse": $inputRoot
}
```