# Migrating from Kafka to RabbitMQ at SimpleBet: Why and How

This is the content of my talk for RabbitMQ Summit 2021. This presentation is generated using the [Marp Presentation Ecosystem](https://marp.app/).

## Talk Description

At Simplebet, we are striving to make every moment of every sporting event a betting opportunity. In doing so, we initially chose Kafka to deliver market updates. The result was a setup which was difficult and expensive to maintain, and non-trivial for our customers to integrate with. After researching other delivery mechanisms, we migrated to RabbitMQ, as it provided us with a low-cost, low-latency alternative that satisfied our business needs. Best of all, it provided a familiar, standards-based means of integration for our customers with superior flexibility to what Kafka offers.

In this talk, we will cover the technical and business reasons for why RabbitMQ has proven itself to be a great platform for building a B2B SaaS product, how it compares to other tools on the market, and where it excels in flexibility for our customers.

## Links
- [Presentation](#)
- [Notes](notes.md)

## Development

To view this presentation locally, first run `npm install`.

### Build
Outputs HTML

``` sh
npm run html
```

### Watch
Watches for changes, builds html

``` sh
npm run watch
```

### Server
Serves project at `http://localhost:8080`

``` sh
npm run server
```
