<!-- NOTE: THIS FILE IS AUTOGENERATED. DO NOT EDIT BY HAND. -->
<!-- see templates/registry/markdown/attribute_namespace.md.j2 -->

# DB

- [General Database Attributes](#general-database-attributes)
- [Deprecated Database Attributes](#deprecated-database-attributes)
- [Deprecated Database Metrics](#deprecated-database-metrics)

## General Database Attributes

This group defines the attributes used to describe telemetry in the context of databases.

| Attribute | Type | Description | Examples | Stability |
|---|---|---|---|---|
| <a id="db-client-connection-pool-name" href="#db-client-connection-pool-name">`db.client.connection.pool.name`</a> | string | The name of the connection pool; unique within the instrumented application. In case the connection pool implementation doesn't provide a name, instrumentation SHOULD use a combination of parameters that would make the name unique, for example, combining attributes `server.address`, `server.port`, and `db.namespace`, formatted as `server.address:server.port/db.namespace`. Instrumentations that generate connection pool name following different patterns SHOULD document it. | `myDataSource` | ![Development](https://img.shields.io/badge/-development-blue) |
| <a id="db-client-connection-state" href="#db-client-connection-state">`db.client.connection.state`</a> | string | The state of a connection in the pool | `idle` | ![Development](https://img.shields.io/badge/-development-blue) |
| <a id="db-collection-name" href="#db-collection-name">`db.collection.name`</a> | string | The name of a collection (table, container) within the database. [1] | `public.users`; `customers` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| <a id="db-namespace" href="#db-namespace">`db.namespace`</a> | string | The name of the database, fully qualified within the server address and port. [2] | `customers`; `test.users` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| <a id="db-operation-batch-size" href="#db-operation-batch-size">`db.operation.batch.size`</a> | int | The number of queries included in a batch operation. [3] | `2`; `3`; `4` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| <a id="db-operation-name" href="#db-operation-name">`db.operation.name`</a> | string | The name of the operation or command being executed. [4] | `findAndModify`; `HMSET`; `SELECT` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| <a id="db-operation-parameter" href="#db-operation-parameter">`db.operation.parameter.<key>`</a> | string | A database operation parameter, with `<key>` being the parameter name, and the attribute value being a string representation of the parameter value. [5] | `someval`; `55` | ![Development](https://img.shields.io/badge/-development-blue) |
| <a id="db-query-parameter" href="#db-query-parameter">`db.query.parameter.<key>`</a> | string | A database query parameter, with `<key>` being the parameter name, and the attribute value being a string representation of the parameter value. [6] | `someval`; `55` | ![Development](https://img.shields.io/badge/-development-blue) |
| <a id="db-query-summary" href="#db-query-summary">`db.query.summary`</a> | string | Low cardinality summary of a database query. [7] | `SELECT wuser_table`; `INSERT shipping_details SELECT orders`; `get user by id` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| <a id="db-query-text" href="#db-query-text">`db.query.text`</a> | string | The database query being executed. [8] | `SELECT * FROM wuser_table where username = ?`; `SET mykey ?` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| <a id="db-response-returned-rows" href="#db-response-returned-rows">`db.response.returned_rows`</a> | int | Number of rows returned by the operation. | `10`; `30`; `1000` | ![Development](https://img.shields.io/badge/-development-blue) |
| <a id="db-response-status-code" href="#db-response-status-code">`db.response.status_code`</a> | string | Database response status code. [9] | `102`; `ORA-17002`; `08P01`; `404` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| <a id="db-stored-procedure-name" href="#db-stored-procedure-name">`db.stored_procedure.name`</a> | string | The name of a stored procedure within the database. [10] | `GetCustomer` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| <a id="db-system-name" href="#db-system-name">`db.system.name`</a> | string | The database management system (DBMS) product as identified by the client instrumentation. [11] | `other_sql`; `softwareag.adabas`; `actian.ingres` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |

**[1] `db.collection.name`:** It is RECOMMENDED to capture the value as provided by the application
without attempting to do any case normalization.

The collection name SHOULD NOT be extracted from `db.query.text`,
when the database system supports query text with multiple collections
in non-batch operations.

For batch operations, if the individual operations are known to have the same
collection name then that collection name SHOULD be used.

**[2] `db.namespace`:** If a database system has multiple namespace components, they SHOULD be concatenated from the most general to the most specific namespace component, using `|` as a separator between the components. Any missing components (and their associated separators) SHOULD be omitted.
Semantic conventions for individual database systems SHOULD document what `db.namespace` means in the context of that system.
It is RECOMMENDED to capture the value as provided by the application without attempting to do any case normalization.

**[3] `db.operation.batch.size`:** Operations are only considered batches when they contain two or more operations, and so `db.operation.batch.size` SHOULD never be `1`.

**[4] `db.operation.name`:** It is RECOMMENDED to capture the value as provided by the application
without attempting to do any case normalization.

The operation name SHOULD NOT be extracted from `db.query.text`,
when the database system supports query text with multiple operations
in non-batch operations.

If spaces can occur in the operation name, multiple consecutive spaces
SHOULD be normalized to a single space.

For batch operations, if the individual operations are known to have the same operation name
then that operation name SHOULD be used prepended by `BATCH `,
otherwise `db.operation.name` SHOULD be `BATCH` or some other database
system specific term if more applicable.

**[5] `db.operation.parameter.<key>`:** For example, a client-side maximum number of rows to read from the database
MAY be recorded as the `db.operation.parameter.max_rows` attribute.

`db.query.text` parameters SHOULD be captured using `db.query.parameter.<key>`
instead of `db.operation.parameter.<key>`.

**[6] `db.query.parameter.<key>`:** If a query parameter has no name and instead is referenced only by index,
then `<key>` SHOULD be the 0-based index.

`db.query.parameter.<key>` SHOULD match
up with the parameterized placeholders present in `db.query.text`.

`db.query.parameter.<key>` SHOULD NOT be captured on batch operations.

Examples:

- For a query `SELECT * FROM users where username =  %s` with the parameter `"jdoe"`,
  the attribute `db.query.parameter.0` SHOULD be set to `"jdoe"`.

- For a query `"SELECT * FROM users WHERE username = %(username)s;` with parameter
  `username = "jdoe"`, the attribute `db.query.parameter.username` SHOULD be set to `"jdoe"`.

**[7] `db.query.summary`:** The query summary describes a class of database queries and is useful
as a grouping key, especially when analyzing telemetry for database
calls involving complex queries.

Summary may be available to the instrumentation through
instrumentation hooks or other means. If it is not available, instrumentations
that support query parsing SHOULD generate a summary following
[Generating query summary](/docs/database/database-spans.md#generating-a-summary-of-the-query)
section.

**[8] `db.query.text`:** For sanitization see [Sanitization of `db.query.text`](/docs/database/database-spans.md#sanitization-of-dbquerytext).
For batch operations, if the individual operations are known to have the same query text then that query text SHOULD be used, otherwise all of the individual query texts SHOULD be concatenated with separator `; ` or some other database system specific separator if more applicable.
Parameterized query text SHOULD NOT be sanitized. Even though parameterized query text can potentially have sensitive data, by using a parameterized query the user is giving a strong signal that any sensitive data will be passed as parameter values, and the benefit to observability of capturing the static part of the query text by default outweighs the risk.

**[9] `db.response.status_code`:** The status code returned by the database. Usually it represents an error code, but may also represent partial success, warning, or differentiate between various types of successful outcomes.
Semantic conventions for individual database systems SHOULD document what `db.response.status_code` means in the context of that system.

**[10] `db.stored_procedure.name`:** It is RECOMMENDED to capture the value as provided by the application
without attempting to do any case normalization.

For batch operations, if the individual operations are known to have the same
stored procedure name then that stored procedure name SHOULD be used.

**[11] `db.system.name`:** The actual DBMS may differ from the one identified by the client. For example, when using PostgreSQL client libraries to connect to a CockroachDB, the `db.system.name` is set to `postgresql` based on the instrumentation's best knowledge.

---

`db.client.connection.state` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `idle` | idle | ![Development](https://img.shields.io/badge/-development-blue) |
| `used` | used | ![Development](https://img.shields.io/badge/-development-blue) |

---

`db.system.name` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `actian.ingres` | [Actian Ingres](https://www.actian.com/databases/ingres/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `aws.dynamodb` | [Amazon DynamoDB](https://aws.amazon.com/pm/dynamodb/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `aws.redshift` | [Amazon Redshift](https://aws.amazon.com/redshift/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `azure.cosmosdb` | [Azure Cosmos DB](https://learn.microsoft.com/azure/cosmos-db) | ![Development](https://img.shields.io/badge/-development-blue) |
| `cassandra` | [Apache Cassandra](https://cassandra.apache.org/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `clickhouse` | [ClickHouse](https://clickhouse.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `cockroachdb` | [CockroachDB](https://www.cockroachlabs.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `couchbase` | [Couchbase](https://www.couchbase.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `couchdb` | [Apache CouchDB](https://couchdb.apache.org/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `derby` | [Apache Derby](https://db.apache.org/derby/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `elasticsearch` | [Elasticsearch](https://www.elastic.co/elasticsearch) | ![Development](https://img.shields.io/badge/-development-blue) |
| `firebirdsql` | [Firebird](https://www.firebirdsql.org/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `gcp.spanner` | [Google Cloud Spanner](https://cloud.google.com/spanner) | ![Development](https://img.shields.io/badge/-development-blue) |
| `geode` | [Apache Geode](https://geode.apache.org/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `h2database` | [H2 Database](https://h2database.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `hbase` | [Apache HBase](https://hbase.apache.org/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `hive` | [Apache Hive](https://hive.apache.org/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `hsqldb` | [HyperSQL Database](https://hsqldb.org/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `ibm.db2` | [IBM Db2](https://www.ibm.com/db2) | ![Development](https://img.shields.io/badge/-development-blue) |
| `ibm.informix` | [IBM Informix](https://www.ibm.com/products/informix) | ![Development](https://img.shields.io/badge/-development-blue) |
| `ibm.netezza` | [IBM Netezza](https://www.ibm.com/products/netezza) | ![Development](https://img.shields.io/badge/-development-blue) |
| `influxdb` | [InfluxDB](https://www.influxdata.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `instantdb` | [Instant](https://www.instantdb.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `intersystems.cache` | [InterSystems Caché](https://www.intersystems.com/products/cache/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `mariadb` | [MariaDB](https://mariadb.org/) | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `memcached` | [Memcached](https://memcached.org/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `microsoft.sql_server` | [Microsoft SQL Server](https://www.microsoft.com/sql-server) | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `mongodb` | [MongoDB](https://www.mongodb.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `mysql` | [MySQL](https://www.mysql.com/) | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `neo4j` | [Neo4j](https://neo4j.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `opensearch` | [OpenSearch](https://opensearch.org/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `oracle.db` | [Oracle Database](https://www.oracle.com/database/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `other_sql` | Some other SQL database. Fallback only. | ![Development](https://img.shields.io/badge/-development-blue) |
| `postgresql` | [PostgreSQL](https://www.postgresql.org/) | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `redis` | [Redis](https://redis.io/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `sap.hana` | [SAP HANA](https://www.sap.com/products/technology-platform/hana/what-is-sap-hana.html) | ![Development](https://img.shields.io/badge/-development-blue) |
| `sap.maxdb` | [SAP MaxDB](https://maxdb.sap.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `softwareag.adabas` | [Adabas (Adaptable Database System)](https://documentation.softwareag.com/?pf=adabas) | ![Development](https://img.shields.io/badge/-development-blue) |
| `sqlite` | [SQLite](https://www.sqlite.org/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `teradata` | [Teradata](https://www.teradata.com/) | ![Development](https://img.shields.io/badge/-development-blue) |
| `trino` | [Trino](https://trino.io/) | ![Development](https://img.shields.io/badge/-development-blue) |

## Deprecated Database Attributes

Describes deprecated database attributes.

| Attribute | Type | Description | Examples | Stability |
|---|---|---|---|---|
| <a id="db-cassandra-consistency-level" href="#db-cassandra-consistency-level">`db.cassandra.consistency_level`</a> | string | Deprecated, use `cassandra.consistency.level` instead. | `all`; `each_quorum`; `quorum` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `cassandra.consistency.level`. |
| <a id="db-cassandra-coordinator-dc" href="#db-cassandra-coordinator-dc">`db.cassandra.coordinator.dc`</a> | string | Deprecated, use `cassandra.coordinator.dc` instead. | `us-west-2` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `cassandra.coordinator.dc`. |
| <a id="db-cassandra-coordinator-id" href="#db-cassandra-coordinator-id">`db.cassandra.coordinator.id`</a> | string | Deprecated, use `cassandra.coordinator.id` instead. | `be13faa2-8574-4d71-926d-27f16cf8a7af` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `cassandra.coordinator.id`. |
| <a id="db-cassandra-idempotence" href="#db-cassandra-idempotence">`db.cassandra.idempotence`</a> | boolean | Deprecated, use `cassandra.query.idempotent` instead. |  | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `cassandra.query.idempotent`. |
| <a id="db-cassandra-page-size" href="#db-cassandra-page-size">`db.cassandra.page_size`</a> | int | Deprecated, use `cassandra.page.size` instead. | `5000` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `cassandra.page.size`. |
| <a id="db-cassandra-speculative-execution-count" href="#db-cassandra-speculative-execution-count">`db.cassandra.speculative_execution_count`</a> | int | Deprecated, use `cassandra.speculative_execution.count` instead. | `0`; `2` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `cassandra.speculative_execution.count`. |
| <a id="db-cassandra-table" href="#db-cassandra-table">`db.cassandra.table`</a> | string | Deprecated, use `db.collection.name` instead. | `mytable` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.collection.name`. |
| <a id="db-connection-string" href="#db-connection-string">`db.connection_string`</a> | string | Deprecated, use `server.address`, `server.port` attributes instead. | `Server=(localdb)\v11.0;Integrated Security=true;` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `server.address` and `server.port`. |
| <a id="db-cosmosdb-client-id" href="#db-cosmosdb-client-id">`db.cosmosdb.client_id`</a> | string | Deprecated, use `azure.client.id` instead. | `3ba4827d-4422-483f-b59f-85b74211c11d` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `azure.client.id`. |
| <a id="db-cosmosdb-connection-mode" href="#db-cosmosdb-connection-mode">`db.cosmosdb.connection_mode`</a> | string | Deprecated, use `azure.cosmosdb.connection.mode` instead. | `gateway`; `direct` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `azure.cosmosdb.connection.mode`. |
| <a id="db-cosmosdb-consistency-level" href="#db-cosmosdb-consistency-level">`db.cosmosdb.consistency_level`</a> | string | Deprecated, use `cosmosdb.consistency.level` instead. | `Eventual`; `ConsistentPrefix`; `BoundedStaleness`; `Strong`; `Session` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `azure.cosmosdb.consistency.level`. |
| <a id="db-cosmosdb-container" href="#db-cosmosdb-container">`db.cosmosdb.container`</a> | string | Deprecated, use `db.collection.name` instead. | `mytable` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.collection.name`. |
| <a id="db-cosmosdb-operation-type" href="#db-cosmosdb-operation-type">`db.cosmosdb.operation_type`</a> | string | Deprecated, no replacement at this time. | `batch`; `create`; `delete` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Removed, no replacement at this time. |
| <a id="db-cosmosdb-regions-contacted" href="#db-cosmosdb-regions-contacted">`db.cosmosdb.regions_contacted`</a> | string[] | Deprecated, use `azure.cosmosdb.operation.contacted_regions` instead. | `["North Central US", "Australia East", "Australia Southeast"]` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `azure.cosmosdb.operation.contacted_regions`. |
| <a id="db-cosmosdb-request-charge" href="#db-cosmosdb-request-charge">`db.cosmosdb.request_charge`</a> | double | Deprecated, use `azure.cosmosdb.operation.request_charge` instead. | `46.18`; `1.0` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `azure.cosmosdb.operation.request_charge`. |
| <a id="db-cosmosdb-request-content-length" href="#db-cosmosdb-request-content-length">`db.cosmosdb.request_content_length`</a> | int | Deprecated, use `azure.cosmosdb.request.body.size` instead. |  | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `azure.cosmosdb.request.body.size`. |
| <a id="db-cosmosdb-status-code" href="#db-cosmosdb-status-code">`db.cosmosdb.status_code`</a> | int | Deprecated, use `db.response.status_code` instead. | `200`; `201` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.response.status_code`. |
| <a id="db-cosmosdb-sub-status-code" href="#db-cosmosdb-sub-status-code">`db.cosmosdb.sub_status_code`</a> | int | Deprecated, use `azure.cosmosdb.response.sub_status_code` instead. | `1000`; `1002` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `azure.cosmosdb.response.sub_status_code`. |
| <a id="db-elasticsearch-cluster-name" href="#db-elasticsearch-cluster-name">`db.elasticsearch.cluster.name`</a> | string | Deprecated, use `db.namespace` instead. | `e9106fc68e3044f0b1475b04bf4ffd5f` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.namespace`. |
| <a id="db-elasticsearch-node-name" href="#db-elasticsearch-node-name">`db.elasticsearch.node.name`</a> | string | Deprecated, use `elasticsearch.node.name` instead. | `instance-0000000001` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `elasticsearch.node.name`. |
| <a id="db-elasticsearch-path-parts" href="#db-elasticsearch-path-parts">`db.elasticsearch.path_parts.<key>`</a> | string | Deprecated, use `db.operation.parameter` instead. | `test-index`; `123` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.operation.parameter`. |
| <a id="db-instance-id" href="#db-instance-id">`db.instance.id`</a> | string | Deprecated, no general replacement at this time. For Elasticsearch, use `db.elasticsearch.node.name` instead. | `mysql-e26b99z.example.com` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Removed, no general replacement at this time. For Elasticsearch, use `db.elasticsearch.node.name` instead. |
| <a id="db-jdbc-driver-classname" href="#db-jdbc-driver-classname">`db.jdbc.driver_classname`</a> | string | Removed, no replacement at this time. | `org.postgresql.Driver`; `com.microsoft.sqlserver.jdbc.SQLServerDriver` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Removed, no replacement at this time. |
| <a id="db-mongodb-collection" href="#db-mongodb-collection">`db.mongodb.collection`</a> | string | Deprecated, use `db.collection.name` instead. | `mytable` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.collection.name`. |
| <a id="db-mssql-instance-name" href="#db-mssql-instance-name">`db.mssql.instance_name`</a> | string | Deprecated, SQL Server instance is now populated as a part of `db.namespace` attribute. | `MSSQLSERVER` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Removed, no replacement at this time. |
| <a id="db-name" href="#db-name">`db.name`</a> | string | Deprecated, use `db.namespace` instead. | `customers`; `main` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.namespace`. |
| <a id="db-operation" href="#db-operation">`db.operation`</a> | string | Deprecated, use `db.operation.name` instead. | `findAndModify`; `HMSET`; `SELECT` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.operation.name`. |
| <a id="db-redis-database-index" href="#db-redis-database-index">`db.redis.database_index`</a> | int | Deprecated, use `db.namespace` instead. | `0`; `1`; `15` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.namespace`. |
| <a id="db-sql-table" href="#db-sql-table">`db.sql.table`</a> | string | Deprecated, use `db.collection.name` instead. | `mytable` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.collection.name`, but only if not extracting the value from `db.query.text`. |
| <a id="db-statement" href="#db-statement">`db.statement`</a> | string | The database statement being executed. | `SELECT * FROM wuser_table`; `SET mykey "WuValue"` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.query.text`. |
| <a id="db-system" href="#db-system">`db.system`</a> | string | Deprecated, use `db.system.name` instead. | `other_sql`; `adabas`; `cache` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.system.name`. |
| <a id="db-user" href="#db-user">`db.user`</a> | string | Deprecated, no replacement at this time. | `readonly_user`; `reporting_user` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Removed, no replacement at this time. |

---

`db.cassandra.consistency_level` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `all` | all | ![Development](https://img.shields.io/badge/-development-blue) |
| `any` | any | ![Development](https://img.shields.io/badge/-development-blue) |
| `each_quorum` | each_quorum | ![Development](https://img.shields.io/badge/-development-blue) |
| `local_one` | local_one | ![Development](https://img.shields.io/badge/-development-blue) |
| `local_quorum` | local_quorum | ![Development](https://img.shields.io/badge/-development-blue) |
| `local_serial` | local_serial | ![Development](https://img.shields.io/badge/-development-blue) |
| `one` | one | ![Development](https://img.shields.io/badge/-development-blue) |
| `quorum` | quorum | ![Development](https://img.shields.io/badge/-development-blue) |
| `serial` | serial | ![Development](https://img.shields.io/badge/-development-blue) |
| `three` | three | ![Development](https://img.shields.io/badge/-development-blue) |
| `two` | two | ![Development](https://img.shields.io/badge/-development-blue) |

---

`db.cosmosdb.connection_mode` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `direct` | Direct connection. | ![Development](https://img.shields.io/badge/-development-blue) |
| `gateway` | Gateway (HTTP) connection. | ![Development](https://img.shields.io/badge/-development-blue) |

---

`db.cosmosdb.consistency_level` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `BoundedStaleness` | bounded_staleness | ![Development](https://img.shields.io/badge/-development-blue) |
| `ConsistentPrefix` | consistent_prefix | ![Development](https://img.shields.io/badge/-development-blue) |
| `Eventual` | eventual | ![Development](https://img.shields.io/badge/-development-blue) |
| `Session` | session | ![Development](https://img.shields.io/badge/-development-blue) |
| `Strong` | strong | ![Development](https://img.shields.io/badge/-development-blue) |

---

`db.cosmosdb.operation_type` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `batch` | batch | ![Development](https://img.shields.io/badge/-development-blue) |
| `create` | create | ![Development](https://img.shields.io/badge/-development-blue) |
| `delete` | delete | ![Development](https://img.shields.io/badge/-development-blue) |
| `execute` | execute | ![Development](https://img.shields.io/badge/-development-blue) |
| `execute_javascript` | execute_javascript | ![Development](https://img.shields.io/badge/-development-blue) |
| `head` | head | ![Development](https://img.shields.io/badge/-development-blue) |
| `head_feed` | head_feed | ![Development](https://img.shields.io/badge/-development-blue) |
| `invalid` | invalid | ![Development](https://img.shields.io/badge/-development-blue) |
| `patch` | patch | ![Development](https://img.shields.io/badge/-development-blue) |
| `query` | query | ![Development](https://img.shields.io/badge/-development-blue) |
| `query_plan` | query_plan | ![Development](https://img.shields.io/badge/-development-blue) |
| `read` | read | ![Development](https://img.shields.io/badge/-development-blue) |
| `read_feed` | read_feed | ![Development](https://img.shields.io/badge/-development-blue) |
| `replace` | replace | ![Development](https://img.shields.io/badge/-development-blue) |
| `upsert` | upsert | ![Development](https://img.shields.io/badge/-development-blue) |

---

`db.system` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `adabas` | Adabas (Adaptable Database System) | ![Development](https://img.shields.io/badge/-development-blue) |
| `cassandra` | Apache Cassandra | ![Development](https://img.shields.io/badge/-development-blue) |
| `clickhouse` | ClickHouse | ![Development](https://img.shields.io/badge/-development-blue) |
| `cockroachdb` | CockroachDB | ![Development](https://img.shields.io/badge/-development-blue) |
| `cosmosdb` | Microsoft Azure Cosmos DB | ![Development](https://img.shields.io/badge/-development-blue) |
| `couchbase` | Couchbase | ![Development](https://img.shields.io/badge/-development-blue) |
| `couchdb` | CouchDB | ![Development](https://img.shields.io/badge/-development-blue) |
| `db2` | IBM Db2 | ![Development](https://img.shields.io/badge/-development-blue) |
| `derby` | Apache Derby | ![Development](https://img.shields.io/badge/-development-blue) |
| `dynamodb` | Amazon DynamoDB | ![Development](https://img.shields.io/badge/-development-blue) |
| `edb` | EnterpriseDB | ![Development](https://img.shields.io/badge/-development-blue) |
| `elasticsearch` | Elasticsearch | ![Development](https://img.shields.io/badge/-development-blue) |
| `filemaker` | FileMaker | ![Development](https://img.shields.io/badge/-development-blue) |
| `firebird` | Firebird | ![Development](https://img.shields.io/badge/-development-blue) |
| `geode` | Apache Geode | ![Development](https://img.shields.io/badge/-development-blue) |
| `h2` | H2 | ![Development](https://img.shields.io/badge/-development-blue) |
| `hanadb` | SAP HANA | ![Development](https://img.shields.io/badge/-development-blue) |
| `hbase` | Apache HBase | ![Development](https://img.shields.io/badge/-development-blue) |
| `hive` | Apache Hive | ![Development](https://img.shields.io/badge/-development-blue) |
| `hsqldb` | HyperSQL DataBase | ![Development](https://img.shields.io/badge/-development-blue) |
| `influxdb` | InfluxDB | ![Development](https://img.shields.io/badge/-development-blue) |
| `informix` | Informix | ![Development](https://img.shields.io/badge/-development-blue) |
| `ingres` | Ingres | ![Development](https://img.shields.io/badge/-development-blue) |
| `instantdb` | InstantDB | ![Development](https://img.shields.io/badge/-development-blue) |
| `interbase` | InterBase | ![Development](https://img.shields.io/badge/-development-blue) |
| `intersystems_cache` | InterSystems Caché | ![Development](https://img.shields.io/badge/-development-blue) |
| `mariadb` | MariaDB | ![Development](https://img.shields.io/badge/-development-blue) |
| `maxdb` | SAP MaxDB | ![Development](https://img.shields.io/badge/-development-blue) |
| `memcached` | Memcached | ![Development](https://img.shields.io/badge/-development-blue) |
| `mongodb` | MongoDB | ![Development](https://img.shields.io/badge/-development-blue) |
| `mssql` | Microsoft SQL Server | ![Development](https://img.shields.io/badge/-development-blue) |
| `mysql` | MySQL | ![Development](https://img.shields.io/badge/-development-blue) |
| `neo4j` | Neo4j | ![Development](https://img.shields.io/badge/-development-blue) |
| `netezza` | Netezza | ![Development](https://img.shields.io/badge/-development-blue) |
| `opensearch` | OpenSearch | ![Development](https://img.shields.io/badge/-development-blue) |
| `oracle` | Oracle Database | ![Development](https://img.shields.io/badge/-development-blue) |
| `other_sql` | Some other SQL database. Fallback only. See notes. | ![Development](https://img.shields.io/badge/-development-blue) |
| `pervasive` | Pervasive PSQL | ![Development](https://img.shields.io/badge/-development-blue) |
| `pointbase` | PointBase | ![Development](https://img.shields.io/badge/-development-blue) |
| `postgresql` | PostgreSQL | ![Development](https://img.shields.io/badge/-development-blue) |
| `progress` | Progress Database | ![Development](https://img.shields.io/badge/-development-blue) |
| `redis` | Redis | ![Development](https://img.shields.io/badge/-development-blue) |
| `redshift` | Amazon Redshift | ![Development](https://img.shields.io/badge/-development-blue) |
| `spanner` | Cloud Spanner | ![Development](https://img.shields.io/badge/-development-blue) |
| `sqlite` | SQLite | ![Development](https://img.shields.io/badge/-development-blue) |
| `sybase` | Sybase | ![Development](https://img.shields.io/badge/-development-blue) |
| `teradata` | Teradata | ![Development](https://img.shields.io/badge/-development-blue) |
| `trino` | Trino | ![Development](https://img.shields.io/badge/-development-blue) |
| `vertica` | Vertica | ![Development](https://img.shields.io/badge/-development-blue) |

## Deprecated Database Metrics

"Describes deprecated db metrics attributes."

| Attribute | Type | Description | Examples | Stability |
|---|---|---|---|---|
| <a id="db-client-connections-pool-name" href="#db-client-connections-pool-name">`db.client.connections.pool.name`</a> | string | Deprecated, use `db.client.connection.pool.name` instead. | `myDataSource` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.client.connection.pool.name`. |
| <a id="db-client-connections-state" href="#db-client-connections-state">`db.client.connections.state`</a> | string | Deprecated, use `db.client.connection.state` instead. | `idle` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.client.connection.state`. |
| <a id="pool-name" href="#pool-name">`pool.name`</a> | string | Deprecated, use `db.client.connection.pool.name` instead. | `myDataSource` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.client.connection.pool.name`. |
| <a id="state" href="#state">`state`</a> | string | Deprecated, use `db.client.connection.state` instead. | `idle` | ![Deprecated](https://img.shields.io/badge/-deprecated-red)<br>Replaced by `db.client.connection.state`. |

---

`db.client.connections.state` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `idle` | idle | ![Development](https://img.shields.io/badge/-development-blue) |
| `used` | used | ![Development](https://img.shields.io/badge/-development-blue) |

---

`state` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `idle` | idle | ![Development](https://img.shields.io/badge/-development-blue) |
| `used` | used | ![Development](https://img.shields.io/badge/-development-blue) |
