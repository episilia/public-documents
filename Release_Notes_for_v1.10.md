# **Release notes for Episilia 1.1.0**

# Features and Enhancements:

- Alert - log based alert server is been added.Alert metrics will be pushed to cpanel and cpanel will be publishing the alert metrics to Push-gateway.

- Additional search metrics on index warming  will be added to server metrics.

- Text Tokenization Changes to enhance the search queries. 


# Bug fixes:

- Optimizer OOM fixes


# Config changes:

# Added few additional configs w.r.t Alert:

- **kafka.group.logwatcher:** Kafka consumer group for logwatcher.

- **kafka.group.logwatcher.tail:** Kafka consumer group for logwatcher-tail.

- **kafka.group.logwatcher.alert:** Kafka consumer group for logwatcher-alert.

- **kafka.topic.alert.request.in:** Kafka topic to publish results for alerts.

- **alert.rules.file.url:** s3 path for alert rules files.

- **alert.prometheus.gateway:** Push-gateway url to be added here.
