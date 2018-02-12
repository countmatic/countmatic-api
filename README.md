# countmatic API
Open API/Swagger description of the countmatic backend. Follow us also on twitter: https://twitter.com/countmatic

## Request a new counter
```
GET https://api.countmatic.io/v2/counter/new?name=MyCounter
Result: {'token':'ehdu75gdj#98hjdt'}
```
This creates a new counter for you and returns your access token.
## Get the current reading of your counter
```
GET https://api.countmatic.io/v2/counter/current?token=ehdu75gdj#98hjdt
Result: [{'name':'MyCounter', 'count':'0'}]
```
This returns the name and current reading of the requested counter. It is an array because it can return more than one counter.
## Increment your counter
```
GET https://api.countmatic.io/v2/counter/next?token=ehdu75gdj#98hjdt
Result: {'name':'MyCounter', 'count':'1'}
```
This increments your counter and returns the new value.
## Decrement your counter
```
GET https://api.countmatic.io/v2/counter/previous?token=ehdu75gdj#98hjdt
Result: {'name':'MyCounter', 'count':'0'}
```
This decrements your counter and returns the new value.
## Reset your counter
```
GET https://api.countmatic.io/v2/counter/reset?token=ehdu75gdj#98hjdt
Result: {'name':'MyCounter', 'count':'1'}
```
This resets your counter to 1.
## Request read-only token for your counter
```
GET https://api.countmatic.io/v2/counter/readonly?token=ehdu75gdj#98hjdt
Result: {'token':'h7ZtR5gdjKKKhjdt'}
```
This token can only be used for the [current] call. Give it to people or machines that need to read-only your counter.
## Multicounter - More counters for one token

You can create new counters with new tokens. But you can also create another counter for your existing token.
## Create another counter for your token
```
GET https://api.countmatic.io/v2/counter/add?token=ehdu75gdj#98hjdt&name=MyOtherCounter
Result: {'name':'MyOtherCounter', 'count':'0'}
```
This adds a new counter to your existing token. If you want to operate on one single counter of your multicounter, you have to specify the name now:
```
GET https://api.countmatic.io/v2/counter/next?token=ehdu75gdj#98hjdt&name=MyOtherCounter
Result: {'name':'MyOtherCounter', 'count':'1'}
```
## Get the current reading of your multicounters
```
GET https://api.countmatic.io/v2/counter/current?token=ehdu75gdj#98hjdt
Result: [{'name':'MyCounter', 'count':'1'}, {'name':'MyOtherCounter', 'count':'1'}]
```
This returns the name and current reading of all your enumerators for that token now.

# Simple JAVA example

```java
package io.countmtic.test;

import io.countmatic.api_v2.ApiException;
import io.countmatic.api_v2.CounterApi;
import io.countmatic.api_v2.model.Counter;
import io.countmatic.api_v2.model.Counters;
import io.countmatic.api_v2.model.Token;

public class JavaTestclient {

	static final String COUNTER_NAME = "JavaTestCounter";
	static final String ANOTHER_COUNTER = "AnotherJavaTestCounter";
	
	public static void main(String[] args) throws ApiException {
		// get a api client
		CounterApi counterApi = new CounterApi();
		System.out.println("Testing countmatic.io");
		
		// create a new counter
		Token token = counterApi.getNewCounter(COUNTER_NAME, 10l);
		System.out.println("\n(1) Got a token: " + token.getToken());
		
		// increment counter
		Counter counter = counterApi.nextNumber(token.getToken(), COUNTER_NAME, 1l);
		System.out.println("\n(2) Got a counter: " + counter.toString());		
		
		// add another counter with 42 initial
		counter = counterApi.addCounter(token.getToken(), ANOTHER_COUNTER, 42l);
		System.out.println("\n(3) Got another counter: " + counter.toString());	
		
		// get readonly token
		Token readonlyToken = counterApi.getReadOnlyToken(token.getToken());
		System.out.println("\n(4) Got a readonly-token: " + readonlyToken.getToken());
		
		// read the current values with the readonly token
		Counters counters = counterApi.getCurrentReading(readonlyToken.getToken(), null);
		System.out.println("\n(5) Got some readings: " + counters.toString());	
		
	}

}
```
Add the dep to your pom.xml:

```
  	<dependency>
  		<groupId>io.countmatic.api</groupId>
  		<artifactId>cm-java7client-v2</artifactId>
  		<version>2.0.1</version>
  	</dependency>
```

Output looks like:

```
Testing countmatic.io

(1) Got a token: 0c6d0737-b81c-43f2-909b-f1578daba785-rw

(2) Got a counter: class Counter {
    name: JavaTestCounter
    count: 11
    modified: 0
}

(3) Got another counter: class Counter {
    name: AnotherJavaTestCounter
    count: 42
    modified: 1518446153593
}

(4) Got a readonly-token: 5c02c4fd-32e7-4b8a-af12-2c0c5b8e0974-ro

(5) Got some readings: class Counters {
    [class Counter {
        name: JavaTestCounter
        count: 11
        modified: 1518446153566
    }, class Counter {
        name: AnotherJavaTestCounter
        count: 42
        modified: 1518446153593
    }]
}

```


