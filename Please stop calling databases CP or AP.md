https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html

# Please stop calling databases CP or AP



> Consistency in CAP actually means linearizability, which is a very specific (and very strong) notion of consistency. In particular it has got nothing to do with the C in ACID, even though that C also stands for “consistency”. I explain the meaning of linearizability below.

    Availability in CAP is defined as “every request received by a non-failing [database] node in the system must result in a [non-error] response”. It’s not sufficient for some node to be able to handle the request: any non-failing node needs to be able to handle it. Many so-called “highly available” (i.e. low downtime) systems actually do not meet this definition of availability.

    Partition Tolerance (terribly mis-named) basically means that you’re communicating over an asynchronous network that may delay or drop messages. The internet and all our datacenters have this property, so you don’t really have any choice in this matter.
