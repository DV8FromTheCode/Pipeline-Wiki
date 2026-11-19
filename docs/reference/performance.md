# Performance Monitoring

Pipeline profiles every event dispatch and action execution. It records invocation and failure counts, total and maximum execution time, recent tick duration and estimated TPS, JVM memory usage, and the online-player count.

Operators can run `/pipeline profile` for a compact live summary. Metrics are kept in memory in a bounded tick window and reset when the server restarts; profiling does not write a separate metrics database.

HTTP actions are synchronous and appear in action timings. A slow endpoint therefore affects the server thread until the request completes or reaches its 10-second timeout. Use webhooks sparingly in tick-sensitive handlers.
