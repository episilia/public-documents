# **Release notes for Episilia 0.10**

## Features and enhancements:

- Search optimization done to improve query TAT for data intensive seaches. 
  For queries with wider time ranges producing large(1000+) results, query TAT is significantly reduced.

# Bug fixes:

-  Index in-memory size compaction leading to lower index warming memory. (Bug fix in 0.9.1)

-  Use ops.index.memory.maxmb for search cache sizing. Reduces memory usage by queries. (fix in 0.9.2)

- Cpanel restarts are significantly reduced.

- Search hanging on trailing wildcard regexp expressions fixed.

# Config Changes:

  No config overrides changes for the deployments.
