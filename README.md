This is a small web application that reproduces the bug behind [SPR-9209](https://jira.springsource.org/browse/SPR-9209).

# How to reproduce the bug

## Normal behavior

1. Start the webapp with `mvn jetty:run`.

2. In another console window, execute `curl -i localhost:8080/request1`. The application correctly returns
HTTP 500 Internal Server Error since the request handler simply throws an IllegalArgument.

3. Execute `curl -i localhost:8080/request2`. The application returns HTTP 400 Bad Request as the @ExceptionHandler
in WebController specifies.

4. Execute `curl -i localhost:8080/request1` again - application still returns HTTP 500.

## The buggy behavior

1. Kill the jetty server from the previous steps and start the server fresh with `mvn jetty:run`.

2. Execute `curl localhost:8080/request2` first this time. Notice that the server returns HTTP 400 and prints
out that it is handling a NullPointerException.

3. Now execute `curl localhost:8080/request`. Now the application incorrectly returns HTTP 400 and prints
"Handling NullPointerException" instead of returning HTTP 500 because of an uncaught exception!