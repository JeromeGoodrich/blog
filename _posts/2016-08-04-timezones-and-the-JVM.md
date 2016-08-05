---
layout: post
title: "HTTPeek: Timezones and Scheduling"
date: 2016-08-04
---

I started the day off with a strange bug. I was creating a test to verify that when a user inputs an expiration-time the handler correctly
creates a bin with that expiration time. The correct expiration time would be successfully put into the database, but when I extracted it
the time would be different. I found this so puzzling that I enlisted the aid of another apprentice and together we quickly realized that the
culprit was an issue with how the timezones were being applied. Luckily a lot of people have had similar issues and we were able to quickly fix it
after reading this [Stack Overflow post](http://stackoverflow.com/questions/25123054/timezone-issues-with-mysql-and-clj-time).

Here's an excerpt from an [excellent post](http://www.javaworld.com/article/2073786/core-java/what-s-your-time-zone-.html?page=2) that demystifies this issue


>The source code for the java.util.TimeZone class's getDefault method shows it eventually invokes the sun.util.calendar.ZoneInfo class's
>getTimeZone method. This method takes a String parameter that is the ID for the required time zone. The default time zone ID is obtained
>from the user.timezone (System) property. If the user.timezone property is not defined, it tries to get the ID using a combination of the
>user.country and java.home (System) properties. If it doesn't succeed in finding a time zone ID, it uses a "fallback" GMT value. In other
>words, if it can't figure out your time zone ID, it uses GMT as your default time zone.

>Note that the System properties are initialized in the java.lang.System class's initProperties method. This is a native method, so the source code
>is unavailable—unless you want to dig into the native code libraries that come with the J2SE distribution. However, I believe that the System properties are
>initialized from the Windows registry on Windows systems and from environment variables on Linux/Unix systems. The initProperties method's Javadoc claims that
>certain properties are "guaranteed to be defined" and lists them. Of the three System properties used by the java.util.TimeZone class's getDefault method, only
>java.home is listed as a guaranteed property in the Javadoc.


The solution proposed in stack overflow works because it sets the `user.timezone` to UTC, which is the same as the default for the PostgreSQL type: timestamp. I'd imagine
another way around this, would be use to use the PostgreSQL extension `timestamptz` and make the expiration field in my table adhere to whatever timezone the JVM is set to.



