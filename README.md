## pg_async.rs
Asynchronous, HA (master-master) PostgreSQL driver on top of libpq.

[![crate](https://img.shields.io/crates/v/pg_async.svg)](https://crates.io/crates/pg_async)
[![docs](https://docs.rs/pg_async/badge.svg)](https://docs.rs/pg_async/)
![](https://tokei.rs/b1/github/ArtemGr/pg_async.rs)
[![patreon](https://img.shields.io/badge/patreon-donate-green.svg)](https://www.patreon.com/user?u=4695668)

## Vision
- [x] Designed to work with a master-master replicated database, such as BDR (but standalone databases are supported too).
- [x] Uses `libpq`, leveraging it's asynchronous support.
- [x] Maintains an asynchronous connection to every node of the replicated cluster.
- [ ] Pings the nodes (with `SELECT 1`) to see who's closer/faster.
- [x] Every operation is a separate transaction.
- [x] If a node fails, the operation is transparently retried on another node.
- [x] Operations are exposed as `futures`.
- [x] `futures` are backed by a thread or two and can be used without a `tokio` reactor (because KISS).
- [ ] Fast mode: send the operation to every node and return the first answer.
- [x] Pin mode: send the operation to one of the nodes only (useful to avoid some of the master-master conflicts).
- [x] There is a JSON helper converting table rows to serde_json objects.
- [ ] If the operation wasn't pipelined already, dropping a `Future` allows the driver to cancel it.
- [x] Database-level timeouts can be used to terminate slow operations early.

## Notes

The combination of libpq and OpenSSL doesn't work very well during PostgreSQL server restarts. I've seen libpq SEGV-crash reliably when the OpenSSL layer is enabled. For the driver to properly work around PostgreSQL server unavailability and restarts I recommend adding the "sslmode=disable" disabler to the connection strings.
