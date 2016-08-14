---
layout: post
title: How the Internet works pt. 2
date: 2016-03-03
---

In part 1, we talked a bit about the Internet, a global system of
interconnected computer networks (a networks of networks) that uses the
Internet protocol suite to communicate with each other. Now let's talk
about the Internet we see when we start-up Chrome, Safari or any other
browser. What we're looking at is not actually the Internet, it's the
World Wide Web, or the Web for short. The Web is simply a massive
hypertext\* browser/editor

> [Hypertext](https://en.wikipedia.org/wiki/Hypertext "Hypertext") is
> structured text that uses logical links [hyperlinks](https://en.wikipedia.org/wiki/Hyperlinks "Hyperlinks")
> between [nodes](https://en.wikipedia.org/wiki/Node_(computer_science)) containing text.

that is accessed via the Internet. When the Web was created in 1991 as a
way to marry hypertext with the Internet, it came along with three huge
technological innovations that are responsible for it's ubiquitous use
today:

1.  Uniform Resource Locator/Identifier (URL/URI aka. web address)
2.  Hypertext Markup Language (HTML)
3.  Hypertext Transfer Protocol  (HTTP)

Let's say I type a URL into the address bar of my browser. It points to
a resource which happens to be a HTML document comprised of hypertext.
My browser will read the HTML document and render it is as a visible
webpage. Before we get into what HTTP is and does let's take a closer
look at this simple example.

In the example, the browser is requesting a service. It wants to display
a webpage. We call service requesters *clients*. On the other side we
have the service provider, or the *server* that is providing the HTML
document to the client. In order for a client to request something and
the server to provide it, the client and server must be connected. We
can connect the client and server through sockets. Sockets are objects
that represents the connection between two machines. When machines are
connected via a socket they have information about each other, including
the the IP address and TCP port. Ports are given numbers 1-65,535. Those
numbers are unique identifiers representing applications running on the
server. Many commonly used applications have preassigned port numbers. A
HTTP web server will run on port 80, Telnet on port 23, etc. Without
these port numbers the server would have no way of understanding which
application the client wanted to connect to.

![A client's connection
request](%7B%7B%20site.baseurl%20%7D%7D/assets/5connect.gif){.aligncenter
width="301" height="73"}![The connection is
made](%7B%7B%20site.baseurl%20%7D%7D/assets/6connect.gif){.aligncenter
width="301" height="88"}

>  A *socket* is one endpoint of a two-way communication link between
> two programs running on the network. A socket is bound to a port
> number so that the TCP layer can identify the application that data is
> destined to be sent to.

It's like in spy or heist movies where someone on top of a tall building
is attempting to zip line into the building across the street. The zip
liner has an accomplice inside of the building on a certain floor at a
certain window. If the zip liner attempt to send his grappling hook to
the wrong window the heist/infiltration/operation will fail. The
building across the street is the server, the window is the port and the
accomplice is the application that the zip liner wants to connect to.
The zip liner is the request that the client, the building the zip liner
is on initially, makes and the zip line itself is the socket connection.
Once the the zip liner makes contact with the accomplice and the
valuables are retrieved the zip liner, now the acting as the response)
goes back up the zip line (using some sort of machine) and returns to
the rooftop of the client building having obtained the resource that she
wanted.

Before the operation occurred the client zip liner and accomplice had a
plan in place so that everything went smoothly, including what window to
use, where to go once inside the building, what to do (steal, plant
bugs, etc.) and  what equipment to use. In other words they had a
protocol.

The Web uses the HTTP protocol. It specifies how a client (usually a
browser) should format a request so that it's understood by a web server
and how the web server should respond to the client so that it is also
understood. Take a look at the visual representations of a typical HTTP
request and response below. They are very similar.

![](%7B%7B%20site.baseurl%20%7D%7D/assets/HTTPrequest.jpg)

![](%7B%7B%20site.baseurl%20%7D%7D/assets/HTTPresponse.jpg)

As mentioned earlier, once a client and server are connected they have
information about each other. The HTTP requests and responses are the
manifestation of that information. Together they are called a HTTP
message.

 

The first line of  HTTP requests and responses are crucial.  In a
request, the first line is called the request line.

> ``` {.newpage}
> Request-Line  = Method SP Request-URI SP HTTP-Version CRLF
> ```

It includes a method, which can be thought of as the type of request
that the client wants to make  (GET, HEAD, POST, PUT, DELETE, DELETE,
TRACE, OPTIONS, CONNECT, PATCH), the method is followed by a space and
then the Request-URI, the address of the resource the client wants to
reach. The Request-URI is followed by another space and the HTTP-Version
(e.g. HTTP/1.1), which is then followed by the newline character
Carriage Return (CR) and Line Feed (LF).

The first-line of the response follows a similar format. It is called
the status line, and includes many of the same elements as the
request-line.

> ```
>  Status-Line = HTTP-Version SP Status-Code SP Reason-Phrase CRLF
> ```

This time, the HTTP version comes first. It's followed by a space and
then a 3 digit status-code indicating information about the status of
the request.For example, if a response is successful the Status-Code
will be 200 if a resource is not found the Status-Code is 404. The
Status-Code is followed by another space and a human readable
Reason-Phrase like "OK" or "Not Found" that corresponds to the
Status-Code. It can be thought of as the reason for the server
responding with the Status-Code it did.

Following the first line of the request and response are request and
response headers. headers, which convey additional information about the
client and server  and provide information about the message-body. They
include things like location, the type of content requested, the length
of that content, the types of requests accepted by the server, etc. They
are all formatted as follows:

>```
> header field name:valueCRLF
>```

The final piece of the an HTTP response or request is what is called the
message-body.

> **HTTP Message Body** is the data bytes transmitted in an
> [HTTP](https://en.wikipedia.org/wiki/HTTP "HTTP")
> transaction message immediately following the
> [headers](https://en.wikipedia.org/wiki/List_of_HTTP_headers "List of HTTP headers")

If it exists, it is preceded by CRLF.

That's about it! Stay tuned...
