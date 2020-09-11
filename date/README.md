# DATE

Two shards using DATE Sharding method.

As a sample parametrizacion, following values has been selected:

* SHARD_KEY: "cm:created" (date field to get a Sharding Date)
* SHARD_DATE_GROUPING: "2" (half year in one shard and half year in the other)
