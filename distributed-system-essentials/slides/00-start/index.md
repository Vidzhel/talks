### If (domain logic) then (solution) <!-- .element: class="orange" -->
## Distributed systems essentials

+++

## What are *microservices*

![Untitled](./slides/00-start/microservices-are-dead.png)

Notes:
- microservices is a buzzword
- falling victim to common cognitive bias

+++

### Distributed system
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

*Distributed computing** is a field of [computer science](https://en.wikipedia.org/wiki/Computer_science "Computer science") that studies distributed systems. A _distributed system_ is a system whose components are located on different [networked computers](https://en.wikipedia.org/wiki/Computer_network "Computer network"), which communicate and coordinate their actions by [passing messages](https://en.wikipedia.org/wiki/Message_passing "Message passing") to one another from any system.[[1]](https://en.wikipedia.org/wiki/Distributed_computing#cite_note-tanenbaum-1)[[2]](https://en.wikipedia.org/wiki/Distributed_computing#cite_note-Distributed_Programs_2010_pp._373–406-2) The components interact with one another in order to achieve a common goal. Three significant challenges of distributed systems are: maintaining concurrency of components, overcoming the [lack of a global clock](https://en.wikipedia.org/wiki/Clock_synchronization "Clock synchronization"), and managing the independent failure of components.[[1]](https://en.wikipedia.org/wiki/Distributed_computing#cite_note-tanenbaum-1) When a component of one system fails, the entire system does not fail.[[3]](https://en.wikipedia.org/wiki/Distributed_computing#cite_note-FOOTNOTEDusseauDusseau20161-2-3) Examples of distributed systems vary from [SOA-based systems](https://en.wikipedia.org/wiki/Service-oriented_architecture "Service-oriented architecture") to [massively multiplayer online games](https://en.wikipedia.org/wiki/Massively_multiplayer_online_game "Massively multiplayer online game") to [peer-to-peer applications](https://en.wikipedia.org/wiki/Peer-to-peer "Peer-to-peer").

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Distributed system

Notes:
- components that aren't running in a single process
- most systems are distributed
- frontend, db

+++

### *Micro* ? services

![Untitled](./slides/00-start/micro-services.png) <!-- .element: class="big-image" -->

Notes:
- notion of microservice misleading
- micro part refers to size
- number of endpoints small
- small subjective
- if too small - dozens of integrations

+++

### Big ball of mud

![Untitled](./slides/00-start/big-ball-of-mud.png) <!-- .element: class="big-image" -->

Notes:
- the most useful architecture
- the largest part of internet written in
- many dependencies
- hard to extend

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### What microservice isn't

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### What microservice isn't <!-- .element: class="orange" -->
## Easiness of delivery

Notes:
- easier to deploy monolith than 300 microservices

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### What microservice isn't <!-- .element: class="orange" -->
## Not about size, easiness of comprehension

Notes:
- we do not break monolith to better understand, it's byproduct

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### What microservice isn't <!-- .element: class="orange" -->
## About using a tech zoo

Notes:
- many languages, shiny technologies, absence of XML, presence REST
- increases complexity
- k8s won't solve domain problems

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### What microservice isn't <!-- .element: class="orange" -->
## For performance reasons

Notes:
- Neither it is
- can have monolith with scaled data layer
- Example - Signal

+++

## It isn't to solve monolith <!-- .element: class="red" -->

![Untitled](./slides/00-start/bbm-decomposition.png)

Notes:
- don't create microservices to fix monolith

+++

## The *great* MONOLITH

![Untitled](./slides/00-start/great-monolith.png) <!-- .element: class="big-image" -->

Notes:
- monolith are great
- do not decompose without a reason
- isn't the same as big ball of mud

+++

## Start with monolith

![Untitled](./slides/00-start/start-with-monolith.png) <!-- .element: class="big-image" -->

Notes:
- decompose application without premature problems

+++

## The single reason
![Untitled](./slides/00-start/who-is-that-pockemon.png) <!-- .element: class="big-image" -->

Notes:
- deliver solutions to business problem
- ASK: if you could name a single reason what would it be?

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

## Agility


![Untitled](./slides/00-start/silicon-valley.png) <!-- .element: class="big-image" -->

Notes:
I mean the Agility

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

## Agility

![Untitled](./slides/00-start/team-work.png) <!-- .element: class="big-image" -->

Notes:
- tremendous on agility
- simplify when many teams
- reduce complexity of change
- dependency between teams
- resilience, easiness of comprehension are secondary

+++

![Untitled](./slides/00-start/many-branches.png) 

Notes:
- no need advantage? no need to decompose
- consider when many PRs

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Additional concerns

![Untitled](./slides/00-start/great-power.png) <!-- .element: class="big-image" -->

Notes:
- great power, great responsibility
- The phrase of Yevhenii when he granted access to staging
- because of complexity, additional concerns

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Additional concerns <!-- .element: class="orange" -->
## Loose coupling

Notes:
- without which will turn into distributed mess

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Additional concerns <!-- .element: class="orange" -->
## Easy problem locality

Notes:
- identify source of problem
- which service is responsible for behaviour
- refers to autonomy and ownership
- hard without separation of concerns and monitoring

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Additional concerns <!-- .element: class="orange" -->
## Easiness of extension

Notes:
- see what other parts are dependent and have to be changed

+++

## The lurking problem

### One only can achieve agility when the coupling is minimal <!-- .element: class="red" -->

Notes:
- the most important part of the talk
- ...

+++

## What is *coupling* after all

![Untitled](./slides/00-start/coupling-degrees.png) 

Notes:
- measure of dependence
- hard to extend
- not sure what will break
- parts have to be deployed together
- can't remove it, useless
- coordination, storage sharing - higher

+++

## Nature of coupling

![Untitled](./slides/00-start/local-global-complexity.png) 

Notes:
- tough to understand
- local - individual microservices
- global - system on the whole
- reduce local complexity - small talkative services
- reduce global complexity - Big Ball Of Mud

+++

## Coupling in distributed system makes it useless

Notes:
- systems becomes number of functions and classes separated by network
- never assume that inter-process calls to a function are the same thing as a network call

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Network fallacies

![Untitled](./slides/00-start/network-fallacies.png) 

Notes:
- We must never assume that a network is reliable.

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Network fallacies <!-- .element: class="orange" -->
## Network is not reliable

![Untitled](./slides/00-start/network-is-not-reliable.png) 

Notes:
- hide the call - services, DI
- timeouts, retries gush
- another - in cloud we are safe
- cloud reliable on the whole
- servers may restart, patch applied

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Network fallacies <!-- .element: class="orange" -->
## Latency is an issue

![Untitled](./slides/00-start/latency-is-an-issue.png) 

Notes:
- network call not instantaneous
- scale to see the problem
- don't distribute your objects
- EF Core, as other ORMs, would lazy load

+++

<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Network fallacies <!-- .element: class="orange" -->
## Bandwidth is limited

Notes:
- sizes, speed CPU grow faster
- to reduce latency we prefetch
- to reduce bandwidth - only needed data
- makes us create tailor made models
- not bites-counting, isn't just about performance

+++

### How to build a *reliable system* over *unreliable network*

Notes:
- turning a blind eye
- we introduce ourselves to

+++

## Vicious circle of coupling

![Untitled](./slides/00-start/calling-calling-calling.png) 

Notes:
- starting from smaller concepts
- trying to simplify change
- failing when coupling slips through
- GOTO -> procedural code -> OOP (encapsulation would introduce too much coupling in a big system leading to stiffness, inability to change) -> Components (encapsulation in components, not between) -> SOA (Service Bus is too smart) -> Microservices

+++

![Untitled](./slides/00-start/mess-without-constraints.png)

Notes:
- coupling crosses all the concepts, not only microservices
- without constraints, coupling introduced easily
- physical boundary in Dist sys. - less prone

+++

