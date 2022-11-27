## Sharing data

Notes:
- I'd argue that the data is not the most important part of an application
- we're trying simulate behaviour, data - byproduct
- couples the components, though

+++

### The purpose of sharing is driven by the business

![Untitled](./slides/02-sharing-data/cap-theorem.png) 

Notes:
- data may used for:
  - simple check (permissions)
  - produce derivatives (analytics)
- some parts
  - need strong consistency (free space during file upload)
  - other tolerate eventual consistency, for better availability
  - one or another, as stated by CAP theorem
  - no way to build strongly consistent always available system
  - when strong consistency requirement - must go in single context
- ask domain expert how to handle individual situation (is it ok if user uses enterprise feature right after the plan expiration?)

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Data ownership

Notes:
- when establish boundaries, assign data to context
- more granular separation only if specifics exist (fun, domain requirements)

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Data ownership

![Untitled](./slides/02-sharing-data/shopping-cart-model.png) <!-- .element: class="big-image" -->

Notes:
- do not model data, but behaviour
- in example, we may model shopping cart

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Data ownership

![Untitled](./slides/02-sharing-data/shopping-cart-data-flow.png) <!-- .element: class="big-image" -->

Notes:
- really, just aggregates data from other contexts
- every time we change a context, we also need to change the shopping cart

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Data ownership

![Untitled](./slides/02-sharing-data/shopping-cart-decomposition.png) <!-- .element: class="big-image" -->

Notes:
- instead, shopping card is behaviour
- different contexts are responsible for its functionality
- it's not noun driven development

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" data-auto-animate-restart -->

### Approaches to data sharing

Notes:
- When sorted out the data ownership,
- how to share it
- depends on goal:
  - analytics
  - front end data aggregation (also known BFF)
  - services communication

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Approaches to data sharing <!-- .element: class="orange" -->
## Data aggregation gateway

![Untitled](./slides/02-sharing-data/api-composition-pattern.png)

Notes:
- gateway either composition or decomposition
- get or set data to multiple places (microservices, context)
- essentially, an anti corruption layer - facade
- don't expose inner BC structure
- not suitable for big joins
- joins CQRS is more suitable
- when decompose, split data required by different BC

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Approaches to data sharing <!-- .element: class="orange" -->
## CQRS

![Untitled](./slides/02-sharing-data/cqrs.png)

Notes:
- create optimized read only model
- Warehouses - copy of data optimized for analytics
- goals different

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Approaches to data sharing <!-- .element: class="orange" -->
## Shared DB

Notes:
- notorious
- like with PHP
  - effective language (lots frameworks), even performant
  - Facebook invested in
  - silly features, users made bad reputation
- not handled right, can cause headaches
  - hard to track changes when schema altered
    - high level schema
    - code generation
    - CI CD checks 
  - hard to track origin of an issue
    - only allow single writer (C++ -> Rust)
  - schema may not satisfy all services needs
    - complex queries, writes...

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Approaches to data sharing <!-- .element: class="orange" -->
## Event sourcing

![Untitled](./slides/02-sharing-data/event-sourcing.png)

[Netflix's case](https://netflixtechblog.com/scaling-event-sourcing-for-netflix-downloads-episode-1-6bc1595c5595)

Notes:
- instead of storing the state, we maintain a log of events
- similar to how source control systems work (not Git)
- different from event-driven, when save state and then publish event
- business reasons
  - auditing, compliance, transparency - history
  - data mining, analytics - ETL - takes time, stream faster
  - frequently changed requirements - Netflix's case
- technical
  - single source of truth - different consumer
  - replay (history)
  - CQRS best friend

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Approaches to data sharing <!-- .element: class="orange" -->
## Change data capture

![Untitled](./slides/02-sharing-data/cdc.png)

Notes:
- compared with event sourcing, - event driven
- we update state, event generated
- event is dummy, no meaning, just upsert or delete
- ideal for updating read views (caches) - must not be done in single transaction
- usually, database log is read to create events
