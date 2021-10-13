# **Release notes for Episilia 1.00.RC1**

# Features and enhancements:

- This release supports helm based installation . For details refer link:[Click-Here](https://docs.google.com/document/d/181NdijoVr0FHV_BN33h-a8fDWo_pVCQv9hKkUw27D4c/edit?usp=sharing)

- The release also supoort episilia-in-a-box product, where all of episilia cluster including grafana can be installed in a single installation.

- All configurations for episilia is now a centralized YAML, instead of server-wise configurations. Use helm for installation and configuration: Refer to the document above.

-	Server wise Prometheus is removed and all metrics data is shared by cpanel.

-	Optimizer trigger is now enabled. Optimizer will be run automatically on the staging folder.

-	Data file caching now respects the caching limit via data.cache.disk.max.gb

-	work.dir has all episilia working data including index and data cache files.

-	Empty s3 folders are skipped during index warming.

-	**Allowed sever modes:** search, gateway, optimizer, indexer, log_watcher.

-	**Search now has a timeout config:** search.api.timeout.seconds

# Bug fixes:
NA

# Config changes:

- Configuration is revamped as per #3 above.
