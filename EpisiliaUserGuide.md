# **User guide**
This document provides details on usage of episilia for the end users, primarily focused on the search of logs using grafana GUI.

## Search Using Grafana:
Log searches in Episilia is done via the Grafana/Loki data source. Grafana should be configured to connect to the episilia-gateway node of the cluster as a Loki data source.

The Epislia query syntax is aligned with the Grafana/Loki query syntax: [https://grafana.com/docs/loki/latest/logql/](https://grafana.com/docs/loki/latest/logql/)

User can run the query as-is they would run for a grafana/loki system. User can type the query in the query window or use the drop-down menu available.

### Structure of query is:

  <b>{LabelSelector1, LabelSelector2, LabelSelector3,.. } SeacrhFilter1 SeacrhFilter2 SeacrhFilter3 </b>

The query comprises of two parts:

- Label Selector Part 
- Search Filter Part

#### Label Selector Part:

There can be one or more label selectors.

<b>{LabelSelector1, LabelSelector2, LabelSelector3,.. } </b>

Each label selector would choose one available label (as per the label name in the input log) and select any of the four operators below:

- **( EQUAL ) Label = “Val”** 
- **(NOT EQ) Label != “Val”** 
- **(REGEX ) Label =~ “Val”** 
- **(NOTREG) Label !~ “Val”**

### Search Filter Part:

There can be multiple search filters applied on the label selectors.

*** SeacrhFilter1 SeacrhFilter2 SeacrhFilter3 …

Search filter can have any of the following operators as below:

- <b>(Exactly contain) |= “Filter”</b> 
- **(Doesn’t contain) != “Filter”** 
- <b>(Regex match) |~ “Filter”</b> 
- **(Regex not match) !~ “Filter”**


For detailed regexp syntax refer to: [https://www.pcre.org/original/doc/html/pcrepattern.html](https://www.pcre.org/original/doc/html/pcrepattern.html)

### Note on App ID and Tenant ID:

Episilia uses/creates a unique app id & tenant id label (as part of the deployment). These two unique labels are available to be used for query as:
 
   - **\_\_app\_\_**  
   - **\_\_tenant\_\_**  

The App ID label (\_\_app\_\_) is provided by log indexer configuration. It can be a fixed value or a combination of multiple labels from the input log.

Similarly, the Tennat ID label (\_\_tenant\_\_) is provided by log indexer configuration. It can be a fixed value or a combination of multiple labels from the input log.

### Sample queries:

Query all the logs for a given app-id:

**\{\_\_app\_\_="Fuduntu12.04"\}**

Query all the logs for a given label:

__{release="14.04LTS"}__

Query by selecting multiple labels:

**{distr="Fuduntu",release="12.04"}**

Query for a given app-id with simple match filter:

<b>{\_\_app\_\_="Fuduntu12.04"} |= "signal"</b>

Query for a given app-id with multiple match filter:

<b>{\_\_app\_\_="Fuduntu12.04"} |= "signal" |= "SIGTERM"</b>

Query all the logs based on regexp match of labels:

<b>{release=~".*LTS"} |= "host"</b>

Query for a given app-id with regexp match filter:

<b>{\_\_app\_\_="openSUSE12.04"} |~ "TID \d3\d\d"</b>

### Running Multiple Queries:

Multiple queries can be selected using “Add Query” in Grafana, which will enable a summing (OR) of log outputs. This is same as running more than one query serially and concatenating the results.

### Running for multiple values of a label:

Multiple values of a label can be passed using regexp as below:

<b>{release=~"(20.10|16.04LTS)"} |= "Error"</b>

### To search for double-quoted strings in the input log, escape the double quotes as below:

<b>{\_\_app\_\_="Fuduntu12.04"} |= "\\"pnmf4.py\\""</b>

### Highlighting context of logs inline:

To enable selecting contextual logs, add the following pre defined label selector

**\_\_ctx\_\_=\<number\>**

Example:

<b>{\_\_app\_\_="fedora14.04LTS",\_\_ctx\_\_=2} |= "shutdown"</b>


### S3 writing of logs inline:

To enable writing results of the search to S3, add the following pre defined label selector
 
**\_\_s3\_\_= 1**

Example:

**{\_\_app\_\_="fedora14.04LTS",\_\_s3\_\_=1}**

To limit the number of lines written to S3, add the following pre defined label selector

**\_\_s3limit\_\_=5000**

Example:

 **{\_\_app\_\_="fedora14.04LTS",\_\_s3limit\_\_=5000}**


### Reference:
- Refer to the Episilia installation guide.
- Grafana loki query syntax:[https://grafana.com/docs/loki/latest/logql/](https://grafana.com/docs/loki/latest/logql/)
- Regexp filter patterns:[https://www.pcre.org/original/doc/html/pcrepattern.html](https://www.pcre.org/original/doc/html/pcrepattern.html)

