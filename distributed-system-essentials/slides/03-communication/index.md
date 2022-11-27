## Communication

Notes:
- last part discusses communication patterns
- vital part of development of a distributed system
- depends on number of factors

+++

### Communication strategy

![Untitled](./slides/03-communication/communication-strategy.png) <!-- .element: class="big-image" -->

Notes:
- synchronous or asynchronous
- different from protocol (HTTP, AMQP)
- whether we call other components within the initial request
- events or other means to postpone the action
- HTTP is fine, unless you create chain of calls
- decreases performance, increases coupling and failure propagation

+++

### Communication purpose

Notes:
- ask: need to query, mutate and why
- can condition change after requested? 
  - no? then reserve
  - atomicity is required our action, reservation 
- same goes for multiple mutations within a use case
  - we usually need them to succeed all or none

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Atomicity in distributed system

Notes:
- driven by domain
  - do we need reverting changes if one from multiple calls fails
  - do we care about inconsistencies when update multiple docs NoSQL
  - always assume that failure will happen
  - will the business tolerate it?
- transaction boundary for different resources:
  - ACID systems like SQL DBs - 
    - everything within transaction is atomic
  - in BASE systems like NoSQL DBs
    - single document wright is atomic
    - transaction is an exception
    - it's possible to update multiple docs - different approach
  - Nothing atomic in between, like MQ and DB
    - if we update DB and publish event in the same transaction - problems will occur

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Atomicity in distributed system <!-- .element: class="orange" -->
## two-phase commit

![Untitled](./slides/03-communication/2pc.png)

Notes:
- easiest approach that comes to mind
- firstly prepare, then commit
- synchronous communication style:
  - participants up
  - problems may collapse everything

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Atomicity in distributed system <!-- .element: class="orange" -->
## saga

![Untitled](./slides/03-communication/sagas.png)

Notes:
- differs from the previous - async
- break process into stages with
- create step revert
- on failure, issue compensating event to revert
- problems - easy:
  - observability
  - dirty reads - depends on context - clear account balance

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Atomicity in distributed system <!-- .element: class="orange" -->
### Orchestration ~~or~~ and Choreography

![Untitled](./slides/03-communication/orchestration-and-choreography.png)

Notes:
- orchestration - single component responsible
  - central component dependency 
- choreography - components play roles
  - harder to track and change
- one isn't better than another - common misconception
- difference between them = diff commands and events (request type)

+++

### Service location

Notes:
- different approaches to communication
- internal
  - most tools
- external
  - cant use message busses...
  - webhooks - synchronous calls - special care, timeouts
  - RSS

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Request type

Notes:
- not all requests alike
- get data
- mutate
- notify

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Request type <!-- .element: class="orange" -->
## Queries

Notes:
- intention to get some data
- destination service known

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Request type <!-- .element: class="orange" -->
## Commands

Notes:
- request change, 
- destination is known

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Request type <!-- .element: class="orange" -->
## Events

Notes:
- indicate something that happened
- publisher doesn't care of consequences
- no defined receiver
- asynchronous, but do not take long to complete
  - monitor for problem

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Request type <!-- .element: class="orange" -->
### Not just Events

![Untitled](./slides/03-communication/coupling-with-events.png) 

Notes:
- more sophisticated
  - event content and granularity is unknown upfront
- couples:
  - event per action
  - excess data that is put in events - gigantic

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Not just Events <!-- .element: class="orange" -->
### Public/Private Events

![Untitled](./slides/03-communication/public-private-events.png) 

Notes:
- do not expose implementation details outside
- we have to create anti-corruption layers
- translate private to public events
- domain events - private and require understand
- state changed events - public, dummy

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Not just Events <!-- .element: class="orange" -->
### Explicit events

![Untitled](./slides/03-communication/explicit-events.png)

Notes:
- events may be ambiguous
- not clear what they mean
- example: should we replace or add?

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Not just Events <!-- .element: class="orange" -->
### Types of events

![Untitled](./slides/03-communication/event-types.png)

Notes:
- 2 types:
  - notification event (shallow)
    - only ids
    - publisher will be queried for more info
  - Event-carried state transfer
    - contains payload in different forms
      - only updated fields
      - delta
    - increases availability - self-contained events
    - ensure compatibility
    - better suited in between BC
      - translate to have specific data and granularity
- depending on usage
  - domain-specific - fewer data, more meaning
  - integration (stage-changed) - synchronization purposes; no meaning

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Not just Events <!-- .element: class="orange" -->
### Order of events

![Untitled](./slides/03-communication/events-wrong-ordering.png)

Notes:
- some event require specific order
- out-of-order messages aren't rare
  - either multiple producers or consumer
- solved with partitions
  - Kafka has out of the box
  - Rabbit MQ - hard to achieve
- better not to depend on order
  - assume such situation and handle properly 
  - order cancelled before accepted
  - generative testing may help 
    - generate events in random order

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" data-auto-animate-restart -->

### Relation between events and commands in terms of dependency

![Untitled](./slides/03-communication/commands-events-dependency.png)

Notes:
- both introduce dependency
- when command is send - knows destination, schema
  - has to be changed when schema does
- events have schema as well
  - publisher knows nothing
  - consumer depends
- used similarly to Dependency Inversion

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Relation between events and commands in terms of dependency <!-- .element: class="orange" -->
## Wrong usage of events 

![Untitled](./slides/03-communication/commands-events-dependency.png)

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Relation between events and commands in terms of dependency <!-- .element: class="orange" -->
## Wrong usage of events 

![Untitled](./slides/03-communication/right-commands-usage.png)

+++

### Idempotency

Notes:
- despite events or commands are used
- identical request are not rare
  - retries in synchronous protocols
  - at least once delivery in asynchronous protocols
- depends on the message intent
  - upsert - always idempotent
  - if it's just state, or domain event like user added - usually not idempotent
- can be solved by saving list of handled request ids for some period

+++

### Example

Notes:
- as per examples
- good: media library service uses the concept of reserving
  - reserves space before file upload, receives link to bucket
- bad: 
  - notification and mailer service dependency

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Example <!-- .element: class="orange" -->
### Notifications service dependency problem

![Untitled](./slides/03-communication/notification-service-queues.png)

Notes:
- both, notification and mailer are generic subdomains
- depend on many other service

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Example <!-- .element: class="orange" -->
### Notifications service dependency problem

![Untitled](./slides/03-communication/notification-service-dependencies.png)

Notes:
- depend on the events
- events change - those change as well

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Example <!-- .element: class="orange" -->
### Notifications service dependency problem

![Untitled](./slides/03-communication/notification-service-di.png)

Notes:
- I'd argue that we need to reverse the dependency
- notification service has to provide endpoint to send the email
- introduction of anti-corruption layer, handles events to send email request
- each context now responsible for email templates

+++

## Questions?
