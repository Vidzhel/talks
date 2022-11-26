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

“There are many useful and revealing heuristics for defining the boundaries of service. Size is one of the least useful.” - Nick Tune 

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
    - No point in DDD for a CRUD
    - With accidental complexity we only complicate life
  - where to invest time or it's better to just buy (authentication)

+++

### Identify subdomain
