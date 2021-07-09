---
marp: true
title: Migrating from Kafka to RabbitMQ at Simplebet: Why and How
description: Hosting Marp slide deck on the web
footer: Kafka :point_right: RabbitMQ at Simplebet — @davydog187
backgroundImage: url(https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/bg.png)
#color: #ff00ff
color: white
theme: uncover
paginate: true
_paginate: false
---

# Migrating from Kafka to RabbitMQ at Simplebet

### Why and How

<!-- My Notes -->

---
![bg](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/bg.png)
# Who am I - Dave Lucia

<!-- color: white -->
* VP, Engineering at Simplebet :basketball: :baseball: :football:
* :dog: :dog: dad :baby: Dad
* :heart: Elixir

---
## What is Simplebet?

<!-- TODO show Simplebet logo and or website-->

<!--
Our mission is to power the future of fan engagement
-->
---
## We turn every moment of a sports event into a betting opportunity
<!-- TODO show image of the FanDuel PlayAction -->

---
## Odds as a service
<!-- TODO show an image of our data -->

<!-- 
Explain who our customers our (Enterprise)
Sport Coverage
Real-time nature
-->
---
## Powered by Machine-Learning

<!-- 
Possibly reference a slide from my Rust talk

Describe how we use machine learning
1. Years of historical data
2. Build features based of deep understanding of the game
3. Refer to the ML panel
-->
---
## Craft branded end-user experiences

![bg left w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/drive_result.png)

---
## In the beginning...

![bg right](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/beginning.jpg)

<!--
* Early days of the company
* Architecture 
* Many open questions
-->

---
## Simplebet Data Pipeline
* :satellite: Receive incident data
* :chart_with_upwards_trend: Create markets (New Batter up to the plate!)
* :1234: Produce odds (Run through ML models :robot:)
* :rocket: Publish markets

<!-- The last step, publish markets, we were not focusing on -->
---
# The Problem

![w:600px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/problem.jpg)

<!--
* Needed a means to publish our data
* Focused on building the core element of our product -->
  * Integration with Machine learning
  * Building a Rust ML framework
-->

---
## Market Publishing Requirements
* :white_check_mark: Customizeable per customer
* :family_man_girl_girl: Fanout
* :racing_car: Minimal latency
* :hammer_and_wrench: Durable

---
# Kafka
![w:600px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/kafka.jpg)

<!---
These are the values of Kafka that attracted us to Kafka initially
-->

---
# How we arrived at Kafka

* Reputation for being fast
* Nascent customer integration story

---
# Deploying Kafka

## Moving Parts
1. Kafka Broker
2. Kafka Rest Proxy
3. Schema registry
4. Kafka UI
5. Zookeeper 

---
# Deploying per customer
TODO talk with Max on these
* Deployed a broker per customer (or was it just a topic?)
* Terraformed out new infra for every customer to isolate

---
# Kafka Dilemmas

:warning: So what problems did we face? :warning:

---
# Kafka Dilemma
## Partioning / Topic Strategy

<!--
One topic to rule them all?
A topic per sports match?
A topic per league?
A topic per market type?
-->

---
# Kafka Dilemma
## Delivering data a la carte

---
# Kafka Dilemma
## Data Retention



---
# Kafka Dilemma
## Authentication and Authorization


---
# Kafka Dilemma
## Simplifying Integration

- Avro
- Broker discovery
- Delivering data for integration (Replays)
---

## Disclaimer:
### Kafka has its use cases

<!-- Show a picture of Franz -->

<!-- 
* Plenty of valid usecases for Kafka for large data volume
* Useful for linearity and many consumers
* RabbitMQ is getting streams!
-->

