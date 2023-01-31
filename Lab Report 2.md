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

