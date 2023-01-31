# Lab Report 2

## Part 1 - StringServer showcase

StringServer.java creates a local server on port 7777. By navigating to http://localhost:7777/, the user can go to path http://localhost:7777/add-message?s= and enter any word or legal characters after the `=`. Doing so will output each input the server receives in order. 

Here is an example:

![Image](Hello.jpg)

Here, the query is "Hello". Navigating to the `/add-message` path calls the `handleRequest` method in StringServer.java. The string of characters after `/add-message/` is saved in an array called `parameters`, with index 0 containing everything to the left of the `=`, and index 1 containing everything to the right of the `=`. The characters after the `=` will be saved in a String called `result`. 

![Image](there.jpg)

In this case, the query is "there". We once again call the `handleRequest` method in StringServer.java by accessing the`/add-message` path. The `?s=there` part of the URL will be saved in the aforementioned `parameters` variable, with index 0 containing everything to the left of the `=`, and index 1 containing everything to the right of the `=`. Again, the characters after the `=` will be saved in a String called `result`. 

Calling this method twice does not change the core aspects of the server, as the server was instantiated by me locally. However, variables such as `parameters` and `result` are subject to change every time the `/add-message/` path is accessed. It is also worth mentioning that attempting to access any other path will be met with the error message `404 Not Found!`.
