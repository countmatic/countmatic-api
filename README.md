# openapi
Open API description of the countmatic backend

## Request a new counter
```
GET https://api.countmatic.io/v1/counter/new?name=MyCounter
Result: {'token':'ehdu75gdj#98hjdt'}
```
This creates a new counter for you and returns your access token.
## Get the current reading of your counter
```
GET https://api.countmatic.io/v1/counter/current?token=ehdu75gdj#98hjdt
Result: [{'name':'MyCounter', 'count':'0'}]
```
This returns the name and current reading of the requested counter. It is an array because it can return more than one counter.
## Increment your counter
```
GET https://api.countmatic.io/v1/counter/next?token=ehdu75gdj#98hjdt
Result: {'name':'MyCounter', 'count':'1'}
```
This increments your counter and returns the new value.
## Decrement your counter
```
GET https://api.countmatic.io/v1/counter/previous?token=ehdu75gdj#98hjdt
Result: {'name':'MyCounter', 'count':'0'}
```
This decrements your counter and returns the new value.
## Reset your counter
```
GET https://api.countmatic.io/v1/counter/reset?token=ehdu75gdj#98hjdt
Result: {'name':'MyCounter', 'count':'1'}
```
This resets your counter to 1.
## Request read-only token for your counter
```
GET https://api.countmatic.io/v1/counter/readonly?token=ehdu75gdj#98hjdt
Result: {'token':'h7ZtR5gdjKKKhjdt'}
```
This token can only be used for the [current] call. Give it to people or machines that need to read-only your counter.
## Multicounter - More counters for one token

You can create new counters with new tokens. But you can also create another counter for your existing token.
## Create another counter for your token
```
GET https://api.countmatic.io/v1/counter/add?token=ehdu75gdj#98hjdt&name=MyOtherCounter
Result: {'name':'MyOtherCounter', 'count':'0'}
```
This adds a new counter to your existing token. If you want to operate on one single counter of your multicounter, you have to specify the name now:
```
GET https://api.countmatic.io/v1/counter/next?token=ehdu75gdj#98hjdt&name=MyOtherCounter
Result: {'name':'MyOtherCounter', 'count':'1'}
```
## Get the current reading of your multicounters
```
GET https://api.countmatic.io/v1/counter/current?token=ehdu75gdj#98hjdt
Result: [{'name':'MyCounter', 'count':'1'}, {'name':'MyOtherCounter', 'count':'1'}]
```
This returns the name and current reading of all your enumerators for that token now.
