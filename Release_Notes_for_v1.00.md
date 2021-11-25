# **Release notes for Episilia 1.0.0**

1.0.0 release for production usage and it coveres all enhancements/fixes from prior 1.0.0rc* releases.
Use the release helm charts for installation.

# Features and enhancements:

- This release has public docker images.

- Health check metrics from cpanel will be publised in prometheus.

- Episilia release version will also be added to server metrics.


# Bug fixes:

- Docker image layers have been reduced to minimal.

- Cpanel docker image size is been reduced from 920MB to 162MB.


# Config changes:

- client.name is replaced by client.id because client.id is auto-generated and will be mapped with the given licence key.

- client.licence.email is replaced by client.licence.key.
