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
* Husband, :dog: :dog: dad, :baby: Dad
* :heart: Elixir

---

![bg w:70%](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/dogs.jpg)

![bg w:70%](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/owen.jpg)

---
![bg w:400px left](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/simplebet_green.png)
Our mission is to power the future of fan engagement

---
![bg w:400px left](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/simplebet_green.png)

Make every moment a of a sporting event a betting opportunity

---
![bg w:400px left](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/simplebet_green.png)

Micro Markets :football: :baseball: :basketball:


---
## Odds as a service
![bg right w:300px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/selections.png)

Powered by Machine Learning :robot:

<!-- 
Explain who our customers our (Enterprise)
Sport Coverage
Real-time nature

Describe how we use machine learning
1. Years of historical data
2. Build features based of deep understanding of the game
3. Refer to the ML panel
-->

---
Craft branded end-user experiences

![bg left w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/drive_result.png)

<!-- 
Explain what our free to play product is
-->

---
## Data Pipeline
1. :satellite: Receive incident data
2. :chart_with_upwards_trend: Create markets
3. :1234: Produce odds :robot:
4. :rocket: Publish markets

<!-- The last step, publish markets, we were not focusing on -->

---
## In the beginning...

![bg right](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/beginning.jpg)

<!--
* Story starts in the early days
* We were building our initial archicture of the odds feed
* Many open questions
-->

---
# The Problem

![w:600px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/problem.jpg)

<!--
* Needed a means to publish our data
* Focused on building the core element of our product
  * Integration with Machine learning
  * Building a Rust ML framework
-->
---

# Serving odds

![w:400px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/diagrams/data_flow_2.png)
<!-- 
TODO  a picture of our feed at a high level
TODO highlight filtering an distribution
-->

---
## Market Publishing Requirements
* :white_check_mark: Customizeable per customer
* :family_man_girl_girl: Distributeable
* :racing_car: Minimal latency
* :hammer_and_wrench: Durable

---
# Kafka
![bg right w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/kafka_logo_white2.png)

<!--
Apache Kafka is an  distributed event streaming platform for high-performance data pipelines, streaming analytics, data integration, and mission-critical applications.
-->

---
![left w:550px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/kafka_features.png)

![bg right w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/kafka_logo_white2.png)


<!---
These are the values of Kafka that attracted us to Kafka initially
-->

---
### Choosing Kafka
![bg right w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/kafka_logo_white2.png)

* Reputation for being fast
* Nascent customer integration story

<!--
Highlight that we picked Kafka because it seemed like a good enough 
fit, but we really did not understand the needs of our customers yet
-->

---
### Diagram
![w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/diagrams/kafka_diagram.png)

![bg right w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/kafka_logo_white2.png)

---
![bg right w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/kafka_logo_white2.png)

### :warning: Dilemmas :warning: 

---
![bg right w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/kafka_logo_white2.png)

### Moving Parts
1. Kafka Broker
2. Kafka Rest Proxy
3. Schema registry
4. Kafka UI
5. Zookeeper 

<!--
You can use a managed solution

BUT! you still need to deal with complexity locally
Deploying per customer
* Deployed a broker per customer
* Terraformed out a topic per customer

-->
---
![bg right w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/kafka_logo_white2.png)

### Partioning / Topic Strategy

* Topic per customer?
* Topic per match?
* Topic per league?

---

![bg right w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/kafka_logo_white2.png)

### Delivering data a la carte

<!--
* Customer packages (nfl \ mlb \ different markets)
-->

---
![bg right w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/kafka_logo_white2.png)

### Data Retention

<!--
* Once a message is written, it is readable for a long time after
* This means that stale messages remain until they are dropped from the log by the broker
-->

---
![bg right w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/kafka_logo_white2.png)

### Authentication and Authorization

<!--
RBAC control is offered for Kafka via the Confluent Metadata service
-->

---
![bg right w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/kafka_logo_white2.png)

### Customer Complexity
- Avro / Schema Registry
- Managing offsets
- Separation of integration data
---

![bg right w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/kafka_logo_white2.png)

### Summary
---

### Re-evaluating our Integration

![bg left w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/rabbit-logo.png)


---
![bg left w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/rabbit-logo.png)
* Integrations should be familiar
* Customizeable
* Easy to provision

---

### Diagram

![bg left w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/rabbit-logo.png)

---
![bg left w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/rabbit-logo.png)

### Serialization
* JSON

---
![bg left w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/rabbit-logo.png)

### Authentication and Authorization
* RabbitMQ Management Interface
* V-Host per consumer

---
![bg left w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/rabbit-logo.png)

### Data Retention
* Durable / Quorum queues
* Reads are destructive

---
![bg left w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/rabbit-logo.png)

### Data a la carte
* Publisher produce to a fanout exchange
* E2E bindings
* Headers dictate the flow of data

---
![bg left w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/rabbit-logo.png)

### Partioning / Topic Strategy
* Let the customer decide!
* Headers exchange bindings
* Headers dictate the flow of data

---
# Summary

![bg left w:200px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/rabbit-logo.png)

---
![bg w:400px](https://raw.githubusercontent.com/davydog187/migrating_from_kafka/main/images/rabbit-logo.png)


---
# Why not kafka
* Hard to use
* Many features you don't get for free
* Have to pay for features

---

# Why RabbitMQ 
* Very easy to get started with
* Batteries included
* Whole package is free


---
# Questions?
---



---
<!---->## Disclaimer:
### Kafka has its use cases -->

<!-- Show a picture of Franz -->

<!-- 
* Plenty of valid usecases for Kafka for large data volume
* Useful for linearity and many consumers
* RabbitMQ is getting streams!
-->
