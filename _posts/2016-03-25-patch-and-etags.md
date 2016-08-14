---
layout: post
title: Patch and Etags
date: 2016-03-25
---

The PATCH HTTP method allows for partial resource modification and was
conceived due to the limitations of PUT, which only allows for the
complete replacement of a resource. Compared to other HTTP methods,
PATCH is relatively new; the document describing the method, RFC 5789,
is dated March, 2010.

The difference, between PATCH and PUT :

> In a PUT request, the enclosed entity is considered to be a modified
> version of the resource stored on the origin server, and the client is
> requesting that the stored version be replaced. With PATCH, however,
> the enclosed entity contains a set of instructions describing how a
> resource currently residing on the origin server should be modified to
> produce a new version.

PATCH is normally neither safe of idempotent. Meaning that a user is
requesting a side-effect and should be aware of it, and that repeated
PATCH requests will not have the same result as calling PATCH once. In
contrast, a safe and idempotent method like GET will produce the same
result no matter how many times it is called, and the user should not
expect side-effects.

One of the cool things about PATCH is that while it is inherently *not*
idempotent it can be issued in a way that it is, which helps the
following scenario:

1.  Client A GETs a resource
2.  Client B GETs the same resource
3.  Client B updates the resource with a PATCH (beat Client A to
    the punch)
4.  Client A doesn’t know about Client B’s update and overwrites it with
    a PATCH of its own

It accomplishes this through the use of conditional requests, that will
fail if the resource has been updated since the client last accessed the
resource. Etags are just one of the validators that are used in
conditional requests. Etags or entity-tags are a opaque quoted strings
that may be prefixed by a weakness indicator. They "tag" the current
version of a resource. So now in order for another user to update the
resource they need provide the most current Etag. This is done by
specifying a an "If-Match" header in the request.
