title: Erlang B Visual Basic implementation for use in Excel
date: 2013-08-10 13:43:00
tags:
- programming
---
Erlang B is a commonly used algorithm used to estimate required capacity for a given load. Originating in telephony for estimating required trunks/channels for an expected load, this same algorithm can be applied to any problem where you have a known arrival rate and required Grade of Service - e.g. servers required for given web traffic, lanes required for given car traffic, staff required for given customers  in a retail shop, etc. I'm by no means an expert on the algorithm (it involves poisson distributions and clever things mathematicians get exited about). I do however use it so need it available in a format that doesn't require a maths degree or regularly going to websites, hence Excel. Note also that this does not account for queuing (Erlang C for that) so may not be suitable where you expect and accept a queue, e.g. customers in your store can wait a few minutes and would not typically mind.
<!-- more -->
For the layman, the algorithm boils down to 3 components:

**Grade of Service** = the probability that the service is "busy". For telephony, this is the % of time that customers get a busy message due to all trunks being consumed. In a web environment it would be how often your users hit a "busy" message from your website due to all capacity being consumed. You set this to a desired level based on your business needs, .5% is quite conservative and works for me. A higher value means you will install less circuits/servers etc which is cheaper, at the cost of increased probability that your service is unavailable to your customers.

**Erlangs** = the <a href="http://en.wikipedia.org/wiki/Erlang_(unit)">definition is on Wikipedia</a>, but this is basically a dimensionless measure of arriving volume, aka load. Don't confuse with the <a href="http://en.wikipedia.org/wiki/Erlang_(programming_language)">programming language</a>. Put simply it is the number of calls/web requests/customers arriving per second/minute/hour/day multiplied by the average time taken to "complete" each request. If each customer in your store takes 6 minutes to handle, and you expect 100 customers per hour, you would have (100 * 60) * 5 = 10 Erlangs. Generally you would measure this over a long period of time and pick your busiest hour to scale capacity.

**Circuits** (or servers, or counter staff, lanes on the road, etc) = these are basically the consumers of the "load". This is usually what you are trying to figure out so you can scale appropriately.

Below is a Visual Basic implementation of Erlang B based on the [pseudo-code algorithms in the paper by Sanzheng Qiao](http://www.cas.mcmaster.ca/~qiao/publications/erlang.pdf). To use it, create a new file called "erlangb.bas" and import it as a module into the Excel spreadsheet you wish to use it with. You will then have access to 2 User Defined Functions (UDF):
```
ErlangB_GoS(erlangs, capacity)

ErlangB_Circuits(erlangs, GoS)
```
The main one of use is `ErlangB_Circuits`. Once you have your Erlangs (perhaps from historic data or forecast) and an agreed target GoS, you can then figure out how many circuits/servers/people you need to service the given load.

There is more in the article that I did not implement (e.g. ErlangC) as I didn't need it for the job at hand, but I leave that as a reader exercise to complete and leave in the comments :)

```vb
Attribute VB_Name = "ErlangB"
Option Explicit
 
' Erlang-B module based on algorithm at http://www.cas.mcmaster.ca/~qiao/publications/erlang.pdf
' Jeff Ferrari
 
' note - make sure the iteration limit is set in Excel (see calculations preferences) to avoid circular reference errors
 
 
 
' given the busy hour traffic (erlangs) and capacity (circuits)
' returns the Grade of Service (GoS)
' assert erlangs and capacity are non-negative
Function ErlangB_GoS(erlangs As Double, capacity As Integer) As Double
   Dim s As Double
   Dim i As Integer
 
   If erlangs = 0 Then
       ErlangB_GoS = 0
       Exit Function
   End If
 
   s = 0
   For i = 1 To capacity
       s = (1 + s) * i / erlangs
   Next
   ErlangB_GoS = 1 / (1 + s)
End Function
 
' given the busy hour traffic (erlangs) and Grade of Service (GoS)
' returns the capacity(circuits) consumed by above traffic while still meeting GoS
Function ErlangB_Circuits(erlangs As Double, GoS As Double) As Double
   Dim rightendpointCapacity As Integer
   Dim leftendpointCapacity As Integer
   Dim midCapacity As Integer
   Dim currentGoS As Double
 
   ' find initial rough interval in which our target resides
   currentGoS = ErlangB_GoS(erlangs, rightendpointCapacity)
   Do While currentGoS > GoS
       leftendpointCapacity = rightendpointCapacity
       rightendpointCapacity = rightendpointCapacity + 32
       currentGoS = ErlangB_GoS(erlangs, rightendpointCapacity)
   Loop
 
 
   ' find capacity using bisection
   Do While (rightendpointCapacity - leftendpointCapacity) > 1
       midCapacity = (leftendpointCapacity + rightendpointCapacity) / 2
       currentGoS = ErlangB_GoS(erlangs, midCapacity)
       If currentGoS > GoS Then
           leftendpointCapacity = midCapacity
       Else
           rightendpointCapacity = midCapacity
       End If
 
   Loop
 
   ErlangB_Circuits = rightendpointCapacity
End Function
```