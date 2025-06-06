<!--- Hugo front matter used to generate the website version of this page:
linkTitle: Redis
--->

# Semantic conventions for Redis client operations

**Status**: [Development][DocumentStatus]

The Semantic Conventions for [Redis](https://redis.com/) extend and override the [Database Semantic Conventions](database-spans.md).

## Spans
<!-- semconv span.db.redis.client -->
<!-- NOTE: THIS TEXT IS AUTOGENERATED. DO NOT EDIT BY HAND. -->
<!-- see templates/registry/markdown/snippet.md.j2 -->
<!-- prettier-ignore-start -->
<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

**Status:** ![Development](https://img.shields.io/badge/-development-blue)

Spans representing calls to Redis adhere to the general [Semantic Conventions for Database Client Spans](/docs/database/database-spans.md).

`db.system.name` MUST be set to `"redis"` and SHOULD be provided **at span creation time**.

**Span name** SHOULD follow the general [database span name convention](/docs/database/database-spans.md#name)
except that `db.namespace` SHOULD NOT be used in the span name since it is a numeric value that ends up
looking confusing.

**Span kind** SHOULD be `CLIENT`.

**Span status** SHOULD follow the [Recording Errors](/docs/general/recording-errors.md) document.

| Attribute  | Type | Description  | Examples  | [Requirement Level](https://opentelemetry.io/docs/specs/semconv/general/attribute-requirement-level/) | Stability |
|---|---|---|---|---|---|
| [`db.operation.name`](/docs/registry/attributes/db.md) | string | The Redis command name. [1] | `HMSET`; `GET`; `SET` | `Required` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`db.namespace`](/docs/registry/attributes/db.md) | string | The [database index] associated with the connection, represented as a string. [2] | `0`; `1`; `15` | `Conditionally Required` If and only if it can be captured reliably. | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`db.response.status_code`](/docs/registry/attributes/db.md) | string | The Redis [simple error](https://redis.io/docs/latest/develop/reference/protocol-spec/#simple-errors) prefix. [3] | `ERR`; `WRONGTYPE`; `CLUSTERDOWN` | `Conditionally Required` [4] | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`error.type`](/docs/registry/attributes/error.md) | string | Describes a class of error the operation ended with. [5] | `timeout`; `java.net.UnknownHostException`; `server_certificate_invalid`; `500` | `Conditionally Required` If and only if the operation failed. | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`server.port`](/docs/registry/attributes/server.md) | int | Server port number. [6] | `80`; `8080`; `443` | `Conditionally Required` [7] | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`db.operation.batch.size`](/docs/registry/attributes/db.md) | int | The number of queries included in a batch operation. [8] | `2`; `3`; `4` | `Recommended` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`db.query.text`](/docs/registry/attributes/db.md) | string | The full syntax of the Redis CLI command. [9] | `HMSET myhash field1 ? field2 ?` | `Recommended` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`db.stored_procedure.name`](/docs/registry/attributes/db.md) | string | The name or sha1 digest of a Lua script in the database. [10] | `GetCustomer` | `Recommended` If operation applies to a specific Lua script. | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`network.peer.address`](/docs/registry/attributes/network.md) | string | Peer address of the database node where the operation was performed. [11] | `10.1.2.80`; `/tmp/my.sock` | `Recommended` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`network.peer.port`](/docs/registry/attributes/network.md) | int | Peer port number of the network connection. | `65123` | `Recommended` if and only if `network.peer.address` is set. | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| [`server.address`](/docs/registry/attributes/server.md) | string | Name of the database host. [12] | `example.com`; `10.1.2.80`; `/tmp/my.sock` | `Recommended` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |

**[1] `db.operation.name`:** It is RECOMMENDED to capture the value as provided by the application without attempting to do any case normalization.
For [transactions and pipelined calls](https://redis.io/docs/latest/develop/clients/redis-py/transpipe/), if the individual operations are known to have the same command then that command SHOULD be used prepended by `MULTI ` or `PIPELINE `. Otherwise `db.operation.name` SHOULD be `MULTI` or `PIPELINE`.

**[2] `db.namespace`:** A connection's currently associated database index may change during its lifetime, e.g. from executing `SELECT <index>`.

If instrumentation is unable to capture the connection's currently associated database index on each query
without triggering an additional query to be executed,
then it is RECOMMENDED to fallback and use the database index provided when the connection was established.

Instrumentation SHOULD document if `db.namespace` reflects the database index provided when the connection was established.

**[3] `db.response.status_code`:** All Redis error prefixes SHOULD be considered errors.

**[4] `db.response.status_code`:** If the operation failed and status code is available.

**[5] `error.type`:** The `error.type` SHOULD match the `db.response.status_code` returned by the database or the client library, or the canonical name of exception that occurred.
When using canonical exception type name, instrumentation SHOULD do the best effort to report the most relevant type. For example, if the original exception is wrapped into a generic one, the original exception SHOULD be preferred.
Instrumentations SHOULD document how `error.type` is populated.

**[6] `server.port`:** When observed from the client side, and when communicating through an intermediary, `server.port` SHOULD represent the server port behind any intermediaries, for example proxies, if it's available.

**[7] `server.port`:** If using a port other than the default port for this DBMS and if `server.address` is set.

**[8] `db.operation.batch.size`:** Operations are only considered batches when they contain two or more operations, and so `db.operation.batch.size` SHOULD never be `1`.

**[9] `db.query.text`:** Query text SHOULD NOT be collected by default unless there is sanitization that excludes sensitive data, e.g. by redacting all literal values present in the query text.
See [Sanitization of `db.query.text`](/docs/database/database-spans.md#sanitization-of-dbquerytext).
The value provided for `db.query.text` SHOULD correspond to the syntax of the Redis CLI. If, for example, the [`HMSET` command](https://redis.io/docs/latest/commands/hmset) is invoked, `"HMSET myhash field1 ? field2 ?"` would be a suitable value for `db.query.text`.

**[10] `db.stored_procedure.name`:** See [FCALL](https://redis.io/docs/latest/commands/fcall/) and [EVALSHA](https://redis.io/docs/latest/commands/evalsha/).

**[11] `network.peer.address`:** If a database operation involved multiple network calls (for example retries), the address of the last contacted node SHOULD be used.

**[12] `server.address`:** When observed from the client side, and when communicating through an intermediary, `server.address` SHOULD represent the server address behind any intermediaries, for example proxies, if it's available.

The following attributes can be important for making sampling decisions
and SHOULD be provided **at span creation time** (if provided at all):

* [`db.namespace`](/docs/registry/attributes/db.md)
* [`db.operation.name`](/docs/registry/attributes/db.md)
* [`db.query.text`](/docs/registry/attributes/db.md)
* [`server.address`](/docs/registry/attributes/server.md)
* [`server.port`](/docs/registry/attributes/server.md)

---

`error.type` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `_OTHER` | A fallback error value to be used when the instrumentation doesn't define a custom value. | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->
<!-- END AUTOGENERATED TEXT -->
<!-- endsemconv -->

## Example

In this example, Redis is connected using a unix domain socket and therefore the connection string is left out.

| Key                       | Value |
|:--------------------------| :-------------------------------------------- |
| Span name                 | `"HMSET"` |
| `db.system.name`          | `"redis"` |
| `network.peer.address`    | `"/tmp/redis.sock"` |
| `network.transport`       | `"unix"` |
| `db.namespace`            | `"15"` |
| `db.query.text`           | `"HMSET myhash field1 'Hello' field2 'World"` |
| `db.operation.name`       | `"HMSET"` |

[DocumentStatus]: https://opentelemetry.io/docs/specs/otel/document-status
