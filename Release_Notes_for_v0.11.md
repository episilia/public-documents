# **Release notes for Episilia 0.11**

# Features and enhancements:

- Re-Use of cache DB between search server restarts to speed up index warming. To use this feature, attach a    persistant volume to search server.  Refer config changes a) below to enable this feature.

- Labels in messages can be disable now by using the following flag in grafana search query.
  By adding this flag "__label__ =0" (by default labels are enabled) in the query.


# Bug fixes:

-  **Unique Labels:** The logs are added Unique lables so that ALL the logs are visible in grafana. Grafana will no longer show less than the given "limit" number of logs because of duplicate removal.

-  **Change in message:** 
    \- "Error in search server: No search results found for the query please try again later " changed to  "No  search results found for the query". 
    If index warming is still in process message is kept as-is: "No search results found for the query please try again later "

# Config changes:

- **ops.index.cache.resetonstart:**
  The value "true" in this config signifies that the data in cache DB can be discarded/reset on restarts and    "false" signifies that the cache data will be re-used from the dir where pvc is attached and mapped to.\
  \- name: ops.index.cache.resetonstart\
     &ensp;value: "true"

- In the below config please map the dir to which pvc is attached.\
  \- name: ops.cache.dir\
     &ensp;value: "/data"
