---
layout: post
title: How the Internet works pt. 1
date: 2016-02-23
---

Let's talk a little bit about how the internet works. To begin, we'll
need to know a little bit about telecommunication. Telecommunication is
simply an exchange of information through the use of technology.
Technology is very broad and could mean anything - smoke signals,
semaphore flags, telegraphs, cellphones and computers all qualify. For
each of these methods of communication, there is a system of rules that
allow the entities exchanging information to understand one another.
This is what we call a protocol.

When computers can exchange data, that is "talk to each other", they are
said to be in a network. The Internet is a network of networks that uses
a suite of protocols (aptly) called the Internet protocol suite to be
able to communicate with one another.  The many protocols in the
Internet protocol suite are separated into four broad layers of
functionality.

1.  The Application Layer
2.  The Transport Layer
3.  The Internet Layer
4.  The Link Layer

**The Application Layer:** The point of networking applications is to
allow devices within the network to send different types of information
to each other we can call this information data. The protocols in the
application layer specify how data taken from an application can be used
in the network. The most well known networking protocol is the Hypertext
Transfer Protocol (HTTP), which we'll get into in a bit.

**The Transport Layer:** The transport layer is below the application
layer. It takes the data from the application layer and adds a "header"
to the data, forming a segment. Because the protocols in this layer are
responsible for ensuring the reliable transport of data, the header is
typically comprised of a source port and a destination port, as well as
some other things based on which protocol is being used. The two most
common protocols in the transport layer are the Transmission Control
Protocol (TCP) and the User Datagram Protocol (UDP). The difference:


![Screen Shot 2016-02-23 at 11.49.19
AM](%7B%7B%20site.baseurl%20%7D%7D/assets/screen-shot-2016-02-23-at-11-49-19-am1.png)


**The Internet Layer:** The internet layer takes the segment from the
transport layer and adds another header to it forming an IP
datagram/packet. Because Protocols in this layer specify how and *where*
data ought to be shipped this header will typically include a source IP
address and a destination IP address, which are essentially labels to
individual devices in the network. IP is an acronym for the most used
protocol in this layer, the Internet Protocol (IP).

**The Link Layer:** The link layer takes a packet from the internet
layer adds a header and tail to it forming a frame. Because Protocols in
this layer specify how to send the frames over the network, and
connecting to a network requires both software and hardware, a frame
 usually consists of a source and destination Media Access Control (MAC)
address/physical address. The MAC address is linked to the hardware of a
network adapter. In there words, an IP address is to software as a MAC
address is to hardware. A frame contains everything
needed to *route *the data to a destination.

The frame is then converted into bits where transmitted at the physical
layer.

Whew! that was a lot and very technical. I found the following diagrams
helpful for my own understanding as well as the [RFC
1122](https://tools.ietf.org/html/rfc1122) . Part 2. will be all about
the application layer and specifically, HTTP.

![](%7B%7B%20site.baseurl%20%7D%7D/assets/30fig08.jpg){.alignleft
width="285"
height="282"}![](%7B%7B%20site.baseurl%20%7D%7D/assets/Protocol-Data-Unit-PDU-and-Layer-Addressing-in-Data-Encapsulation-Cisco-Inter-networking.jpg){.alignnone
width="354" height="265"}

 

Images:

*- http://www.cyberciti.biz/faq/key-differences-between-tcp-and-udp-protocols/\
- http://ptgmedia.pearsoncmg.com/imprint\_downloads/informit/learninglabs/9780134213736/graphics/30fig08.jpg\
- http://www.learn44.com/wp-content/uploads/2013/06/Protocol-Data-Unit-PDU-and-Layer-Addressing-in-Data-Encapsulation-Cisco-Inter-networking.jpg*

 
