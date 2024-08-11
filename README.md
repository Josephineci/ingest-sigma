# ingest-sigma
# Ingest-rs - Data Platform Ingestion Services

[![deploy ingest-polygon](https://github.com/team-sigma-ai/ingest-rs/actions/workflows/ingest-polygon.yml/badge.svg?branch=main)](https://github.com/team-sigma-ai/ingest-rs/actions/workflows/ingest-polygon.yml)
[![deploy ingest-cboe](https://github.com/team-sigma-ai/ingest-rs/actions/workflows/ingest-cboe.yml/badge.svg)](https://github.com/team-sigma-ai/ingest-rs/actions/workflows/ingest-cboe.yml)
[![deploy ingest-tradingtech](https://github.com/team-sigma-ai/ingest-rs/actions/workflows/ingest-trading-tech.yml/badge.svg)](https://github.com/team-sigma-ai/ingest-rs/actions/workflows/ingest-trading-tech.yml)
[![Security Audit](https://github.com/team-sigma-ai/ingest-rs/actions/workflows/rustsec_audit.yml/badge.svg)](https://github.com/team-sigma-ai/ingest-rs/actions/workflows/rustsec_audit.yml)

Ingest-rs contains a set of Data Ingestion Services for [Polygon](https://polygon.io/), [Kaiko](https://www.kaiko.com/), [CBOE](https://www.cboe.com) and [TradingTech](https://www.tradingtechnologies.com/).

Each ingestion service is its own separate binary that can be run and deployed independently.

## Ingestion Services

- [Polygon](src/engines/polygon)
- [Kaiko](src/engines/kaiko.rs)
- [CBOE](src/engines/cboe.rs)
- [TradingTech](src/engines/tradingtech.rs)

## Local Development

This will require a set of running instances of [Redis, RedPanda and Postgres](https://gist.github.com/dandxy89/aa4ac45440860b93e152984731586e04).

Data ingestion is controlled via registry - to run this service locally, you will need to populate the Database instructions to do this are located in the project. Once populated, you can run the service locally and on registration, the service will commence ingestion for the given set of instruments.

Example Config:

```yaml
logging:
  level: "info"
kafka_producer:
  group_id: "dpn-ingest-local"
  bootstrap_node: "localhost:19092"
  ssl: false
  queue_size: 1000
registry:
  uri: http://localhost:49051
engine: "Polygon"
polygon:
  uri: wss://socket.polygon.io/
  key: "API_KEY"
  kafka_topic_prefix: "candlestick"
```

Note: Scaffolding of the Database (for registry) requires running the [deploy_PG_datastore](https://github.com/team-sigma-ai/sigma-pg-database/blob/main/sql_scripts/deploy_PG_datastore.sql) sql script.

