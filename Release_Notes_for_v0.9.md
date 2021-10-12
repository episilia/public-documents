# **Release notes for Episilia 0.9**

## Features and enhancements:

- **Episilia Control Panel:**     
    \- Monitors health/metrics of episilia cluster\
    \- Collects server tools health indicators/metrics and also runs additional health checks on the cluster

- **Search Pod Memory improvment:**\
    \- Index in-memory size compaction leading to lower index warming memory.\
    \- Use ops.index.memory.maxmb for search cache sizing. Reduces memory usage by queries.

## Bug fixes:

- Multi Json Parsers in episilia-log-indexer for better performance i.e Multi threads are enabled now.


## Config Changes:

- All the servers now have to pass the clientname and env for the cpanel has the data stored under their unique env 
  \* client.name: epic\
  \* client.env: prod

- All the existing servers have got these new config through which the required info for cpanel is handled
  \* cpanel.data.topic: cpaneld\
  \* cpanel.data.interval.seconds: 300\
  \* out.kafka.metadata.broker.list: localhost:9092

- \* ops.server.mode:  gateway\
   Values based on below:\
[    SERVER_MODE_INDEXER "indexer"\
     SERVER_MODE_OPTIMIZER "optimizer"\
     SERVER_MODE_SEARCH_REALTIME "search_realtime"\
     SERVER_MODE_SEARCH_DATE_RANGE "search_daterange" (Historic search)\
     SERVER_MODE_GATEWAY "gateway" ]

- Some new config changes only for episilia-log-indexer
  \* out.kafka.metadata.broker.list: localhost:9092\
  \* out.kafka.stage.topic: stagefiles #publish indexed files\
  \* out.kafka.stage.group.topic: stagegroup #publish indexed files\
  \* out.kafka.optimize.topic: optfiles # publish file names post optimization\
  \* out.kafka.optimize.group.id: optimizer-group # kafka group used by optimizer for the above topics\
  \* out.kafka.sign.payload: true
