# The Eight Fallacies of Distributed Computing

## Background/History

I first heard about the Eight Fallacies of Distrubuted computing at a Java One conference when James Gosling mentioned them. Gosling attribted them to Peter Deutsch, and as you can see the latest history indicates that apparently he had some other help along the way. I didn't make up the eight fallacies, I just like them as a way to capture some of the important things to keep in mind when designing distributed systems. My hope for this talk is that I can use the Eight Fallacies to build a few memes in your brains, things that you too can keep in mind as you right distributed applications, and lets be honest, most applications are distributed in todays mobile and web dominated world.

## The Network Is Reliable

In the early nineties I was teaching developers how to write NEXTSTEP applications. Often travelling around the country. One of the places I taught a few times was a company in Grand Rapids Michigan, name omitted just in case. As we were working on an exercise in one afternoon, the power suddently went off. The emergency power kicked in for minimal lighting and a diesel genearator started up for the mainframes. But the class was definitely done for the day. As we looked around to see what might be going on, one of the students noticed construction outside. It turned out a backhoe had cut the power line.

-------

In April of 2009 someone climbed into a manhole near San Jose and cut a fiber optic cable, leaving many counties without a connectivity. A similar incident occurred in April of 2013.

----------

Google wraps its trans-Pacific fiber cables in Kevlar to prevent against shark attacks. When they lay cables along the ocean floor, there is often a power component to go with the optical one. That power is used to drive repeaters and other support equiptment. Apparently the electic fields created by the power in the cables is confusing the sharks into thinking that a fish is in distress, and like a good shark, they try to eat the fix.

## Latency Is Zero

In 2011 Spread Networks introduced their low latency fiber connection between Chicago and New York, to provide traders with as close as possible to speed of light connections between the two cities. "Spread Networks LLC, backed by former Netscape Chief Executive Jim Barksdale, spent an estimated $300 million or more to bury a fiber-optic cable that shaved three-thousandths of a second off communications between New York and Chicago, launching in 2010." - WSJ

-----

http://www.ibiblio.org/harris/500milemail.html

--------

A few years ago I was working on a product that used an active-passive backup server design. During normal use the active server would receive updates once or twice an hour. But for testing, QA would run multiple updates as fast as possible through the system. Sometimes during this load testing we would get errors where the backup would get ahead of the primary in version. Tracking through the code, I was able to determine that the problem was the way we updated the primary. When a change occurred, the active server would notify the backup. The backup had one minute to reply with success or failure, and in the case where the backup failed or timed out the primary would not update. The problem was that under the recuring changes used in our test the backup would sometimes pause for a garbage collection. When this happened, the backup could timeout, and still succeed. This left the primary thinking the backup failed, while the backup thought everything was ok.

Several people have found a similar issue occuring with ElasticSearch primary/secondary relationships.

Success on one side of a socket doesn't necessarily imply success on the other side of the network.

## Bandwidth Is Infinite

One aspect of bandwidth is the bandwidth of each system. Take, for example, a message broker. It can handle some number of messages. Maybe it is a big number, but it is a limited number. If you are writing the broker, and you don't plan for what happens when too many messages arrive, you could end up with terrible data loss.

Implement backpressure throughout your system

------------------

Scaling works best when the machines are independent of each other

## The Network is Secure

The Heartbleed bug allows anyone on the Internet to read the memory of the systems protected by the vulnerable versions of the OpenSSL software. This compromises the secret keys used to identify the service providers and to encrypt the traffic, the names and passwords of the users and the actual content. This allows attackers to eavesdrop on communications, steal data directly from the services and users and to impersonate services and users. - heartbleed.com

----------------

The theft of the usernames, passwords, phone numbers and physical addresses of around 145 million eBay EBAY -0.59% users back in May remains one of the biggest data breaches in U.S. history. But what were the others? Over the past 10 years, there have been over 300 data breaches involving the theft of 100,000 or more records (that have been disclosed publicly).

- http://www.forbes.com/sites/niallmccarthy/2014/08/26/chart-the-biggest-data-breaches-in-u-s-history/


## Topology Doesn’t Change

There are two ways to think about change, things that are good and things that are bad. Lets start with bad.

Distributed data storage folks like to talk about something called the CAP theorem. CAP is an acronym for Consistency, Availabity, Partition. Basically, the theorem states that you can only design for two out of three of these during exceptional situations. CAP is all about bad topology changes, it is in the acronym as Partitioning.

-----------

In May of 2012, Fog Creek suffered from a "package flood" when several switchs got confused about their spanning-tree and formed a loop.

-----------

Topology can change in a good way. When you upgrade or scale a server, you change the underlying topology. Recently i was deploying some software and the deploy failed. After digging around a bit we realized that one of the administrators had taken a server offline for an upgrade. The software we were using wasn't resilient to that change. Of course a small tweak got us back on line. But the lesson was learned, don't trust anyone else. No seriously, don't trust the topology. It can change.

## There is One Administrator

On May 20, 2013, Edward Snowden a System Administrator that had worked indirectly for the NSA released a number of documents detailing top secret projects at the agency. Snowden was one of many administrators working for the NSA at the time.

-----

When I started on the Kiln team, one of the things I was tasked with was coming up to speed on the deployment process. Kiln runs on a number of types of systems, including a database, IIS and a number of Gunicorn servers. While the deployment process is fairly automated, and getting moreso every day, my initial deployment ran into a few problems because of permissions on the various systems. You can probably imagine my frustration, but that is all water under the bridge.

Thinking of multiple administrators isn't just a question of multiple people. It is also the idea of multiple systems that can have administrators. Single sign-on can help with this, as can technologies like LDAP. But it is up to developers and administrators :-) to keep these tools in mind when they add new elements to a system.

## Transport Cost is Zero

There are two ways to think about transport costs. Time and Money.

In Feburary of 2014, Netflix and Comcast made a deal to improve the performance and reliability of comcast connections for netflix's customers. Shorlty after, Netflix made another deal with Verizon, that we can assume was for similar reasons. More recently I read that Netflix has made a similar deal with AT&T.

-----

I had my own little encounter with the monetary cost of transport a few years ago. My wife was going to help out her parents during the Tour De France. She was planning to take our sprint hotspot, so I told her to make sure she was on 4g. They told me when I signed up that 4g was unlimited, but 3G was capped. So my wife dutifully walked around the apartment lookign for a place to put the hotspot, and made sure it was 4G, before proceeding to watch 9GB of cycling. The next month, i got my bill for $400 bucks. Turns out sprint changed my plan in the fine print on the back one of the bills.

-----

The cost in time can be thought of as latency.
latency can create network unreliability

You can get bandwidth more easily than you can lower your latency.

<something about FTL>

Adding pipes can give you band width, but latency is ultimately limited by the speed of light. So the only way to lower latency, once you hit that lower bound, is to move your computers.


## The Network is Homogeneous

Lets be honest, it would be hard for a software developer in 2014 to think that the network is homogeous. We all have internet connections at home, that can't seem to be homogeneous from day to do. Unless you are one of the lucky people with fiber, and I don't talk to people with fiber to the home. Most, if not all, of us have smart phones. Many of us have had a smart phone for years and have seen the changes in bandwidth.

Facebook bought WhatsApp in Feburary for a gigantic sum of money. Part of the value WhatsApp brings to users is support for 7 different phone platforms. The growing audience for internet traffic is coming from people that may be exclusively on a phone.

There are tools that can help. Google recently announced a network simulator built into Chrome. MacOS contains a similar feature. Companies like appurify are offering device testing, that can highlight issues between various networks.



## Final Thoughts

 * Design for failure
 * Support partial availability - Remeber CAP
 * Measure everything
  * There are people making money on microseconds
  * There are people trying to make money on every byte
 * Try to do work close to the data
 * The next heartbleed is out there
 * The phone is a full-fledged internet device
 * Watch out for backhoes, bulldozers and guys with axes
