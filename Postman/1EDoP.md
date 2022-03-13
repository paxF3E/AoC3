## Day 00
Monitoring : test an API for different requests, using scripts mentioning the intervals to be run in

Monitoring an Endpoint : using a collection of all variants of an endpoint in diff requests, to cover the endpoint completely

Monitoring entire API : same, just storing common API hosts in a env-var, to differ path of requests to endpoints; chains data across the requests

Running an API : with interlinked endpoints, response can be passed to another as a request, thus careful management is required; as env-vars; or `stringify`-ing the complex objects and arrays

`responseTime` -> latency; `responseCode` -> HTTP response codes

## Day 01

