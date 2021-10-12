# **Release notes for Episilia 0.8**


## Features and enhancements:

- **S3limit**: This feature will define dynamically how many lines will be added to s3, when \_\_s3limit\_\_ flag is used \_\_s3\_\_ flag is not needed 
               also.
               Default s3 limit, if above flag not used in 100k lines(that can be configured in config in future). The following is an example. 
               {\_\_app\_\_="kubernetes",\_\_s3limit\_\_="1"}

- __SIMD Jsonparser__: Episilia was using Rapid Jsonparser and now we have implemented SIMD Jsonparser for better performance to get improved    throughput from indexer.Also as of now it is configurable in config of Episilia-log-indexer, Default is SIMD Jsonparser but the user can choose to run in Rapid Jsonparser also by passing this property.\
    \- name: ops.use.simdjson.parser     
      &ensp;value: "true"\
      &ensp;(true indicates SIMD and false will be Rapid)

- __Help Message__: In grafana the user by default will see the Help link as the first message, the below message will on top.
2021-04-01 13:57:43 Episilia User Guide: http://help.episilia.com

- __S3 Directory Restructuring__: This will do the Directory structuring in Stage folder datewise directories will be created for more accessibility and will help historic search without final also.

## Bug fixes:

- Memory leak Fixes in Episilia-search and Episilia-log-indexer.

- Unescape quotes before passing to search. i.e {\_\_app\_\_="kubernetes"} |= \"\\"episilia\\\" will work .

- Print search error logs into log stream than a pop-up error.

- Changed the syntax \_\_ctxlimit\_\_ to \_\_ctx\_\_ 

- Fixes on empty search issue.

- Fixes for Log-indexer freezing the log consumption from kafka
