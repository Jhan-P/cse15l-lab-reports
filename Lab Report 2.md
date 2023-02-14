# Lab Report 2

## Part 1 - StringServer showcase

StringServer.java creates a local server on port 7777. By navigating to http://localhost:7777/, the user can go to path http://localhost:7777/add-message?s= and enter any word or legal characters after the `=`. Doing so will output each input the server receives in order. 

Here is an example:

![Image](Hello.jpg)

Here, the query is "Hello". Navigating to the `/add-message` path calls the `handleRequest` method in StringServer.java. The string of characters after `/add-message/` is saved in an array called `parameters`, with index 0 containing everything to the left of the `=`, and index 1 containing everything to the right of the `=`. The characters after the `=` will be saved in a String called `result`. 

![Image](there.jpg)

In this case, the query is "there". We once again call the `handleRequest` method in StringServer.java by accessing the`/add-message` path. The `?s=there` part of the URL will be saved in the aforementioned `parameters` variable, with index 0 containing everything to the left of the `=`, and index 1 containing everything to the right of the `=`. Again, the characters after the `=` will be saved in a String called `result`. 

Calling this method twice does not change the core aspects of the server, as the server was instantiated by me locally. However, variables such as `parameters` and `result` are subject to change every time the `/add-message/` path is accessed. It is also worth mentioning that attempting to access any other path will be met with the error message `404 Not Found!`.

## StringServer.java:
```java
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.
    String result = "";

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return String.format(result);
        } 
        else {
            System.out.println("Path: " + url.getPath());
            if (url.getPath().contains("/add-message")) {
                String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("s")) {
                    String query = parameters[1];
                    result += query + "\n";
                    return String.format(result);
                }
            }
            else if (url.getPath().contains("/reset")) {
            result = "";
            return "Result has been reset";  
        }
        }
            return "404 Not Found!";
        }
    }


class StringServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

## Server.java
```java
// A simple web server using Java's built-in HttpServer

// Examples from https://dzone.com/articles/simple-http-server-in-java were useful references

import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;
import java.net.URI;

import com.sun.net.httpserver.HttpExchange;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpServer;

interface URLHandler {
    String handleRequest(URI url);
}

class ServerHttpHandler implements HttpHandler {
    URLHandler handler;
    ServerHttpHandler(URLHandler handler) {
      this.handler = handler;
    }
    public void handle(final HttpExchange exchange) throws IOException {
        // form return body after being handled by program
        try {
            String ret = handler.handleRequest(exchange.getRequestURI());
            // form the return string and write it on the browser
            exchange.sendResponseHeaders(200, ret.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(ret.getBytes());
            os.close();
        } catch(Exception e) {
            String response = e.toString();
            exchange.sendResponseHeaders(500, response.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }
}

public class Server {
    public static void start(int port, URLHandler handler) throws IOException {
        HttpServer server = HttpServer.create(new InetSocketAddress(port), 0);

        //create request entrypoint
        server.createContext("/", new ServerHttpHandler(handler));

        //start the server
        server.start();
        System.out.println("Server Started! Visit http://localhost:" + port + " to visit.");
    }
}
```

## Part 2 - Bug Troubleshooting

For this section, I will be discussing the `reverseInPlace` method from `ArrayExamples.java` from Lab 3.

The method had been given to us in this form:
```java
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```
The purpose of this method was to reverse an array without creating a new one. 

When running this JUnit test for this method,
```java
@Test
 public void testReverseInPlace2() {
   int[] input1 = { 1, 2, 3, 4, 5 }; // Failure inducing input
   ArrayExamples.reverseInPlace(input1);
   assertArrayEquals(new int[]{ 5, 4, 3, 2, 1 }, input1);
 }
```
we were met with the following error messages:
```java
testReverseInPlace2(ArrayTests)
arrays first differed at element [3]; expected:<2> but was:<4>  // No reversal occurred
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:418)
        at org.junit.Assert.assertArrayEquals(Assert.java:429)
        at ArrayTests.testReverseInPlace2(ArrayTests.java:16)
        ... 32 trimmed
Caused by: java.lang.AssertionError: expected:<2> but was:<4>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
        ... 38 more
2) testReversed2(ArrayTests)
arrays first differed at element [0]; expected:<9> but was:<0>  // Old array was being returned instead of the new one
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:418)
        at org.junit.Assert.assertArrayEquals(Assert.java:429)
        at ArrayTests.testReversed2(ArrayTests.java:29)
        ... 32 trimmed
Caused by: java.lang.AssertionError: expected:<9> but was:<0>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
        ... 38 more
```

When passing any array through the method, the same exact array was returned, unreversed; for instance, inputting {1, 2, 3, 4, 5} returned {1, 2, 3, 4, 5}.
An example of an array that passed the JUnit test was {3}, since this array is the same forwards and backwards, as shown in the following code:

```java
@Test
public void reverseInPlaceNoFailure() {
    int[] input1 = { 3 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 3 }, input1);
}
```

Here is an updated, working version of the method, with the reasoning for each line provided in comments:

```java
 static void reverseInPlace(int[] arr) {
   for(int i = 0; i < arr.length/2; i++){ //Only need to traverse half the list
     int curr = arr[i]; // Need to store arr[i] so it doesn’t get lost
     arr[i] = arr[arr.length - i - 1]; // Swap values in corresponding indices
     arr[arr.length - i - 1] = curr; // Swap values in corresponding indices
   }
```
Storing `arr[i]` in a separate variable ensures that its value doesn't get lost. Traversing only the first half of the list ensures we don't swap any values we have already swapped.

## Part 3 - Reflection

These past two weeks in lab, I learned how to remotely set up a server. I have done projects before where I have used free online servers to host content, but I had never hosted one on my own machine (or on a remote machine for that matter).

