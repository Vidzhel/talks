## Reducing dependency

Notes:
- The process requires conscious understanding of domain and future developments

+++

## Establishing boundaries

![Untitled](./slides/01-reducing-dependency/microservices-api-balance.png) <!-- .element: class="big-image" -->

Notes:
- boundary - service that introduces boundary
- balance between integration API and domain
- isn't one shot process, especially at the beginning
- monolith is crucial at the beginning, quickly iterate

+++

## How to decompose

> There are many useful and revealing heuristics for defining the boundaries of service. Size is one of the least useful.
 
Nick Tune 

Notes:
- cohesion as the guideline, not granularity, size
- DDD may help (Serhii have already told about, let's recap)
- Common understanding of notions (example with Materials and support)
- Important concept bounded context
- If Eric he had been rewriting the Blue Book, he'd have moved chapter to beginning. Building blocks aren't that important, they are the local complexity resolvers, not global

+++

## The notion of Bounded Context

![Untitled](./slides/01-reducing-dependency/bounded-context-services-mapping.png)
![Untitled](./slides/01-reducing-dependency/within-bounded-context-mapping.png)

Notes:
- right granularity - one or many services 
- logical boundary that is bigger than classes...
- don't imply any technology

+++

## BC in Distributed systems

![Untitled](./slides/01-reducing-dependency/three-legged-run.png)

Notes:
- goal is to make the unit
- autonomous, available, independent
- it's easier to locate error, extend
- no strong dependency between context
- a team is responsible for one or a couple of BCs
- removes dependency on other teams

+++

### Identifying the boundary

![Untitled](./slides/01-reducing-dependency/the-boundary-slice.png)
![Untitled](./slides/01-reducing-dependency/amazon-decomposed.png)

Notes:
- split depending on fun relation and behaviour
- span across all layers from data to view 
  - usage of micro frontends
- do not split by tech requirement like:
  - platform (mobile, desktop)
  - type of process (long-running or not)
  - similar to how project is structured 
- to insure absence of collisions between teams

+++

### The three-way relationship

![Untitled](./slides/01-reducing-dependency/cross-context-teams.png)
![Untitled](./slides/01-reducing-dependency/dependencies-adapt-when-context-changes.png)

Notes:
- important to form teams right way
- Conway's law  - orgs design systems mirror their own communication structure
- without getting it in order, can't get rid of chaos in code
- the three-way relationship - domain, architecture, teams
- org structure changes under discoveries, update BCs and team structures
- prevent cross-context teams

+++

### Domain language as a hint

![Untitled](./slides/01-reducing-dependency/domain-language-as-hint.png) <!-- .element: class="big-image" -->

Notes:
- we may have a user entity
- used for diff purposes and have diff data
  - billing info for billing context
  - shipping address for shipping context
  - lead in marketing context
- different rules are present, separation is required
- refer to the example with Materials and support

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### How domain influences BCs

![Untitled](./slides/01-reducing-dependency/subdomains.png) 

Notes:
- domain is important heuristic to defining the boundaries
- domain is the problem org tries to solve
- subdomains, in turn, emphasize the importance of domain

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### How domain influences BCs <!-- .element: class="orange" -->
## Generic subdomain

![Untitled](./slides/01-reducing-dependency/subdomains.png) <!-- .element: class="big-image" -->

Notes:
- the stuff companies do the same way
  - billing, authorization
- Does not give business value
- it's an requirement

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### How domain influences BCs <!-- .element: class="orange" -->
## Core subdomain

![Untitled](./slides/01-reducing-dependency/subdomains.png) <!-- .element: class="big-image" -->

Notes:
- provides competitive advantage
- the complex part we want to work on as much as possible
- involves experimentation
- part of application that you do not want to decompose to quickly
- courses creation process, collaboration, results tracking

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### How domain influences BCs <!-- .element: class="orange" -->
## Supporting subdomain

![Untitled](./slides/01-reducing-dependency/subdomains.png) <!-- .element: class="big-image" -->

Notes:
- support core
- specific to the domain but not crucial functionality
- media library, soon video recording, organization templates

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### How domain influences BCs

![Untitled](./slides/01-reducing-dependency/domain-software-relation.png) <!-- .element: class="big-image" -->

Notes:
- It provides the map for action
  - where to use complex technology or modeling approaches like DDD, Event sourcing
    - have benefits but introduce too much complexity
    - No point in DDD for a CRUD,
    - But complex algorithms, validation rules, business rules or invariants requires the approaches
    - With accidental complexity we only complicate life
  - where to invest time, or it's better to just buy (authentication)

+++

### How data influences bounded context

Notes:
- can't always feature work without some data
- once independent units become chatty
- wrong context - may be a reason
- other heuristics:
  - consistency control - two processes have to run one at a time
  - linearizability - reading the last write which cause synchronous take place
  - tolerance to eventual consistency - may be split

+++

### Interactions between BCs

![Untitled](./slides/01-reducing-dependency/bounded-cotext-relations.png) <!-- .element: class="big-image" -->

Notes:
- interesting things happen not in bounded context but between them
- problem in one context must not cause global issue
- context map used for logical relations between BCs:
  - conform:
    - accept language of other BC. 
    - Change with it. 
    - Can conform to multiple context unless collision in notions (e.g. User)
    - Git visual tools - some embrace git specifics while other, make abstraction
  - partner:
    - both context understand messages, domain language. 
    - Collaboration between teams, which is sometimes useful
  - anti corruption layer into:
    - separate component - translator
    - may be larger that our system
    - to separate subdomains, third party dependencies, decouple legacy monolith

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Interactions between teams

Notes:
- interaction between teams are inevitable
- can halt dev process unless coordinated consciously

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Interactions between teams <!-- .element: class="orange" -->
## Collaboration

![Untitled](./slides/01-reducing-dependency/teams-collaboration.png) 

Notes:
- used when exploring core domains
- boundaries aren't clear
- one team may not have sufficient know-how
- teams work closely together
- domain should be broken into services later

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Interactions between teams <!-- .element: class="orange" -->
## X as a Service

![Untitled](./slides/01-reducing-dependency/x-as-service.png) 

Notes:
- used after research is done, boundaries are clear
- to speed up the dev process
- one team develops service for another

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Interactions between teams <!-- .element: class="orange" -->
## Facilitation

Notes:
- one team supports services other depends upon

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

### Interactions between teams <!-- .element: class="orange" -->
## Evolutionary patterns

![Untitled](./slides/01-reducing-dependency/teams-collaboration-evolution.png) 

Notes:
- teams are more flexible than app architecture
- can be used to create temporary teams:
  - for experimentation
  - more people between teams to get wider ivew

+++

### Modeling techniques and examples

- Domain storytelling - helps in understanding the bigger picture
- Event storming - helps in reiterating boundaries, extensive modeling
- [The Art of Discovering Bounded Contexts by Nick Tune](https://www.youtube.com/watch?v=ez9GWESKG4I)
- [Practical DDD: Bounded Contexts + Events = Microservices](https://www.youtube.com/watch?v=Ab5-ebHja3o)
- [Find Context Boundaries with Domain Storytelling - Stefan Hofer and Henning Schwenter - DDDEU 18](https://www.youtube.com/watch?v=Y1ykXnl6r7s)

+++

## Questions?
