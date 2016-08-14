---
layout: post
title: Dependency Inversion in Java
date: 2016-03-08
---

There are two things that stuck with me from my last IPM: creating short
feedback loops, and dependency inversion.  I wrote about feedback
 yesterday, so today I'll try and tackle dependency inversion and how I
attempted to create it in my JAVA HTTP server.

The principle of dependency inversion simply states that the more a
thing changes the less other things should depend on it and that the
more stable something is the more other things can depend on it.
Typically, the more abstract something is the more stable it becomes.
Conversely the more concrete something is the more it is likely to
change.

In JAVA this distinction is made rather explicit. Abstract classes and
interfaces are abstract, and the classes that implement them are more
concrete. A JAVA method or class that uses instances of these classes by
either having them as a dependency in the arguments to a method or
instantiating an instance of them within the method are said to have
concrete dependencies, which means that if we had to change one of those
concretions we'd also have to change all the methods that depend on it.
Take a look at my original WebService class:

```java
public class WebService implements Runnable {

  private Socket clientSocket;

  public WebService(Socket clientSocket) {
    this.clientSocket = clientSocket;
  }

  public void run() {
    try {
      DataOutputStream outToClient = new
      DataOutputStream(clientSocket.getOutputStream());
      InputStreamReader inputReader = new
      InputStreamReader(clientSocket.getInputStream());
      BufferedReader inFromClient = new BufferedReader(inputReader);

      String rawRequest = inFromClient.readLine();
      Request request = new Request();
      request.parse(rawRequest);
      RequestHandler handler = new RequestHandler();
      Response response = handler.handle(request);
      ResponseFormatter formatter = new ResponseFormatter();
      byte[] formattedResponse = formatter.format(response);

      outToClient.write(formattedResponse);
      outToClient.flush();

      clientSocket.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
  }
}
```

There are a ton of concrete dependencies!

1.  Socket in the constructor
2.  DataOutputStream
3.  InputStream
4.  BufferedReader
5.  Request
6.  RequestHandler
7.  Response
8.  ResponseFormatter

If anyone of these concrete dependencies changed, I would have to alter
the run method. In addition, testing the run method with this current
implementation is very difficult.

Here's what I have now. It's far from perfect but it reduced the number
of concrete dependencies I have from 8 to 1:

```java
public class Service implements Runnable {

  private Parser parser;
  private Handler handler;
  private ClientServerIO io;
  private Request request;

  public Service(ClientServerIO io, Handler handler, Parser parser) {
    this.io = io;
    this.parser = parser;
    this.handler = handler;
  }

  public void run() {
    try {
      request = parser.parse(io.inFromClient());
      Response response = handler.handle(request);
      ResponseFormatter formatter = new ResponseFormatter();
      byte[] formattedResponse = formatter.format(response);

      io.outToClient().write(formattedResponse);
      io.outToClient().flush();

      io.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
  }
}
```

The class now has 3 arguments to its constructor. Each of these
arguments are interfaces, abstractions which allows me to pass in any
class that implements the interface. This is called dependency injection
and I found out it is crucial in order to adhere to the dependency
inversion principle in Java.

The flexibility that comes with this implementation allows this Service
to operate independent of of a lot of things, it could be HTTP, FTP or
something else as long as it is within a client server model with some
tweaks this server should be able to deal with many different kinds of
inputs and return many different outputs.
