# Notes

## Talk Description
At Simplebet, we are striving to make every moment of every sporting event a betting opportunity. In doing so, we initially chose Kafka to deliver market updates. The result was a setup which was difficult and expensive to maintain, and non-trivial for our customers to integrate with. After researching other delivery mechanisms, we migrated to RabbitMQ, as it provided us with a low-cost, low-latency alternative that satisfied our business needs. Best of all, it provided a familiar, standards-based means of integration for our customers with superior flexibility to what Kafka offers.

In this talk, we will cover the technical and business reasons for why RabbitMQ has proven itself to be a great platform for building a B2B SaaS product, how it compares to other tools on the market, and where it excels in flexibility for our customers.

## The Story

At Simplebet, we needed a way to deliver our market data to our enterprise customers with high-availability, low-latency, and flexibility. Kafka was initially chosen as the technology to deliver data to our customers, due to the perception of it being the fastest message queue availabile. As we started building out our Kafka-based integration, we began to realize just how difficult Kafka was to work with. Our pain points started when we began to build out our infrastructure, and had to answer hard questions about data separation between customers, topic-granularity, choosing managed vs self-hosted vs cloud-hosted, data retention, partitioning, Avro-schemas, etc. The best Kafka solutions are proprietary, and thus it necessitated either purchasing licenses for the proprietary pieces or choosing lower-quality OSS solutions. Onboarding a new customer meant that we needed to stand up new infrastructure just so all of the right Kafka peices were in place, and an integration-level topic didn't collide with some live data. The worst part about this, is that much of this complexity was pushed right onto our customers!

As we were in talks with our first integration partner, it quickly became apparent that our customers were going to spend a lot of time working on integrating our product, which was directly against our goals of providing an intuitive development experience that was easy to work with with rapid feedback loops and a short development cycle. This is when we began exploring alternative solutions. 

We took a hard look at two different paths. One was to use GraphQL Subscriptions to stream data over websockets. This seemed appealing due to the flexibility that GraphQL provides at the subscription level. That could have meant an interesting integration experience, but ulimately we didn't choose it because

1. We felt that GraphQL may be too unfamiliar for some of our enterprise customers
2. It meant that we still needed an internal solution to producing durable data

When we originally picked Kafka, it was a hasty decision that didn't have the level of scrutiny that it deserved given the complexity that it introduced to our system and the integration experience. This time around we took a much closer look at what RabbitMQ had to offer, and we noticed many great properties that made it a great fit.

1. RabbitMQ implements the AMQP 0-9-1 protocol, which abstracts away RabbitMQ itself, and provides a standards-based means of integration. 
2. RabbitMQ and AMQP provide robust options for topologies that make it extensible, flexible, and easy to generate
3. Using headers exchanges, customers can choose the level of granularity that they works best for their backend by declaring and binding queues based on a fixed set of headers that we describe in our documentation. Customers are no longer limited by any upfront architectural decisions we make at the time of publish, and instead rely on what works best for them
4. RabbitMQ is relatively easy to deploy and manage (ease on our SRE team)
5. We can easily generate new topologies when adding new customers with the RabbitMQ management APIs
6. We can easily introspect our system and build tools ontop of rabbitmq using the same management APIs

## Talking Points

- Kafka used as a customer interface
    - Hard to choose the topics
        - Sport, League, Match, Market, how do we choose the granularity for which topics get created? This should be a preference decided by our customers. Kafka forces us to bake it into the architecture
    - Difficult to work with
        - Need to setup schema registry
        - Work with Avro
        - Choose a Kafka Library
            - Difficult in Elixir as there are many and none have amazing support, very complex to work with
            - Many that I tried had lots of bugs, that have since been fixed
            - Maybe particular to Elixir ecosystem



## The Story (Inpso)
Good tweet on story telling https://twitter.com/SahilBloom/status/1414202782538678273?s=20

Once upon a time there was a _,
Every day, _,
One day _,
Because of that _,
Because of that _,
Until finaly _.

Once upon a time there was a sports betting company named Simplebet,
Every day, they were working on building their initial produc,
One day they chose Kafka to implement their customer facing interface,
Because of that they had many issues managing the complexity of the system,
Because of that they felt that they needed to re-evaluate,
Until finaly, they found RabbitMQ, and all their problems were solved.
`



## Story arch
1. Explain what Simplebet does as a company
2. Describe the pricing product at a high-level
3. Give context to the environment in which decisions were being made
4. How we chose Kafka
5. Describe the dilemmas we started seeing as we began talking to potential customers and building a deeper integration
6. Re-evaluation: choosing RabbitMQ
7. Show how RabbitMQ solves these dilemmas for us
8. Questions


# Feedback
* Keep it to a single sport
* Simplebet - start by talking about freeroll, walk back from there to describe our data product
* Rabbit solves the problem of distribution very elegantly, Kafka got in the way. Kafka made it complicated, RabbitMQ made it simple.
* Change "Delivering data for integration (Replays)" -> "Delivering replay data for integration"
* Who are our customers?
* Besides Fanduel and data product, describe that we have a free to play product that takes bets
  * Need to highlight the differences between our customers
  * Maybe a visual system
* Possibly mention CQRS/Event Sourcing?
* use a different word than "fanout" on line 11 as it can be confused with the fanout exchange
  * Possibly use "distribute" and "distributing" instead
  * Our problem is distributing and filtering. How do we do it differently with Rabbit?
* A visual representation of our data flow
* It was at the 20 minute mark that i started talking about Rabbit
  * Spend more time talking about Rabbit at the solution
* Slide 13 "How we arrived at Kafka"
  * I said what it can do, but i dont say what it is
* Slide 14 "marcelo said it good"
* Drop "market type" on slide 17 as no one will know what that is
* Give a glossary of terms for simplebet
* Give a glossary of terms of Kafka
* Before moving onto talking about RabbitMQ, give a summary of the problems that we had
* Double check how the e2e bindings work, it might be wrong conceptually. If you publish to a fanout, pretty sure it does not look at headers? maybe?
* Give a visual representation (maybe pull from rabbit)
  * Exchange to exchange links
  * Exchange to queues
  * Show how these enable filtering and distribution
  * Show how this is trivial using RabbitMQ
* Give a visual representation of what a V-host does and how easy it is to create

* Drawn to the bright shiny object and how we were drawn to it, and then moved to the workhorse
  * Typically brought up first in the conversation when talking about Message buses
* Possibly consolidate the dilemma/solution to bring rabbitmq into the picture earlier
* RabbitMQ doesn't have a schema registry, which is a good and bad thing. Maybe simpler!
* Kafka is really expensive! Talk about that!
* Put more emphasis on the consumers
* RabbitMQ is the main entrypoint to our product, keep it as simple as possible

* Kafka interface / libraries
* Websockets/ SSE (Chunked HTTP) still need to build intermediate
* AMQP was killer feature
