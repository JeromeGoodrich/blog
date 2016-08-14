---
layout: post
title: Service.Run() TDD
date: 2016-04-13
---

A server takes listens for a request on a specified port once a client
makes a request a connection is created called a socket. When a client
makes a request, that request is in the form of raw data, bytes which is
transmitted to the server through some sort of I/O stream contained
within the socket. Once the server receives the request it  needs to
parse that request into something more useful to the server application,
usually some sort of request object. once the data is in a usable form
another class will handle that request meaning, it will go do whatever
the request wants it to (as long as it is supported) and return some
sort of response. that response will then be written back to client via
the socket. and then the socket will close. In my HTTP Server, the class
that handles all this logic is the Service class, and more specifically
the Run method. There's a lot of behavior to test here and as such this
is a fairly tricky class to design. But let's see if we can give it a
go. Here's a more simplified version of what we'd expect to happen in
the run method.

-   Given a Socket that has a Stream
-   Parser takes Stream and returns Request
-   Handler takes Request and returns Response
-   Response writes to Stream
-   Socket is closed

Where to begin? it might make sense to start at the end in this case. We
need to verify the behavior that a socket is getting closed. One way to
do this is to assert that it is open prior to the Service.Run() method
being called and then assert that it is closed after the method is
called. That might look something like this.

```c#
Assert.Equal(mockSocket.isClosed(), False)
service.Run()
Assert.Equal(mockSocket.isClosed(), True)
```


This tells us that we need to instantiate some object mockSocket that
has an isClosed() method that returns a boolean. We also know that we
will need to instantiate a service object and also that we will need to
pass the mockSocket object to the Service class so that we know that the
mockSocket we are testing is the same as the mockSocket that is being
closed in the run method. We now have:

```c#
var mockSocket = new MockSocket();
var service = new Service(mockSocket);

Assert.Equal(mockSocket.IsClosed(), False)
service.Run();
Assert.Equal(mockSocket.IsClosed(), True);
```

If we have done the correct implementation, the test should pass. Next
we want to verify that the run method is parsing a stream into a request
object. to do this we have to assert three things. One, that the correct
method is being called, and two that the method is called with a
verifiable stream and three, that it returns a verifiable request
object.

```c#
var mockSocket = new MockSocket();
var service = new Service(mockSocket);

Assert.Equal(mockSocket.IsClosed(), False);
Assert.Equal(mockParser.GetCallsToParse(), 0);
service.Run();
Assert.Equal(mockParser.GetCallsToParse(), 1);
Assert.Equal(mockParser.GetLastStreamPassedToParse(), ioStream);
Assert.Equal(mockParser.Parse(mockSocket.GetStream()), request);
Assert.Equal(mockSocket.IsClosed(), True);
```


In order to get the test to pass there's a lot we have to do. We must
create a mockParser object with methods GetCallsToParse(),
Parse(ioStream) and GetLastStreamPassedToParse(). In order to verify
that the mockSocket.GetStream() return the same ioStream as ioStream we
pass an ioStream object to the mockSocket constructor similarly we pass
the request object to the mockParser to ensure that the request that
mockParser.Parse returns is the same as the request in our test.

```c#
var ioStream = new MemoryStream(new byte[1]);
var mockSocket = new mockSocket(ioStream);
var request = new Request()
var mockParser = new MockParser(request);
var service = new Service(mockSocket, mockParser,);

Assert.Equal(mockSocket.IsClosed(), False);
Assert.Equal(mockParser.GetCallsToParse(), 0);
service.Run();
Assert.Equal(mockParser.GetCallsToParse(), 1);
Assert.Equal(mockParser.GetLastStreamPassedToParse(), ioStream);
Assert.Equal(mockParser.Parse(ioStream), request);
Assert.Equal(mockSocket.IsClosed(), True);
```

After correct implementation, the tests should pass. We employ similar
strategies for testing the handler and sending the response:

```c#
var ioStream = new MemoryStream(new byte[1]);
var mockSocket = new mockSocket(ioStream);
var request = new Request()
var mockParser = new MockParser(request);
var mockResponse = new Response();
var mockHandler = new MockHandler(mockResponse);
var service = new Service(mockSocket, mockParser, mockHandler);

Assert.Equal(mockSocket.IsClosed(), False);
Assert.Equal(mockParser.GetCallsToParse(), 0);
Assert.Equal(mockHandler.GetCallsToHandle(), 0);
Assert.Equal(mockResponse.GetCallsToSend(), 0);
service.Run();

Assert.Equal(mockParser.GetCallsToParse(), 1);
Assert.Equal(mockParser.GetLastStreamPassedToParse(), ioStream);
Assert.Equal(mockParser.Parse(ioStream), request);

Assert.Equal(mockHandler.GetCallsToHandle(), 1);
Assert.Equal(mockHandler.GetLastRequestPassedToHandle(), request);
Assert.Equal(mockHandler.Handle(request), response);

Assert.Equal(mockResponse.GetCallsToSend(), 1);
Assert.Equal(mockResponse.GetLastStreamPassedToSend(), ioStream);

Assert.Equal(mockSocket.IsClosed(), True);
```

Done!
