## **EPISILIA**

Helm installation guide for episilia.  Refer to [prerequisites](#prerequisites) before installation.

### **Step1: Adding the helm repo to local Machine**
 Repo URL is Gitlab Page URL
 ```bash
helm repo add [NAME] [URL]
$ helm repo add episilia https://episilia.gitlab.io/episilia-helm/release
```
#### **Listing chart repositories:**
```
$ helm repo list or helm repo ls
  NAME        URL 
  episilia   https://episilia.gitlab.io/episilia-helm/release
```
#### **Searching for charts in the repository:**
```
$ helm search repo episilia
```
### **Step2: episilia/episilia-cpanel is the master chart. In this master chart's values.yaml file update required global values as described in the [values section](#values-documentation)**
Inspect the values before installing application use below:
```
$ helm inspect values episilia/episilia-cpanel > episilia_values.yaml

$ helm install episilia episilia/episilia-cpanel --set global.client.name=episilia-client --set global.client.env=dev
```
### **Step3: Just do the dry run and cross check all the values and see override or updates values reflected.**
```
$ helm install  episilia episilia/episilia-cpanel -f episilia_values.yaml --dry-run
```
```
$ helm install episilia episilia/episilia-cpanel --set global.client.name=episilia-client --set global.client.env=dev --dry-run
```
### **Step4: Install helm repo**
```
helm install [RELEASE NAME] [CHART]
$ helm install episilia episilia/episilia-cpanel  -f episilia_values.yaml
                              or      
$ helm install episilia episilia/episilia-cpanel --set global.client.name=episilia-client --set global.client.env=dev
```
List the installed helm chart
 ```
 $ helm ls
 ```
 Listing all the pods
 ```
 $ kubectl get pods 
 ```
 List the services:
 ```
 $ kubectl get services
```
### **Step5: Access grafana Dashboard in browser**
We can access the grafana dashboard using grafana External IP i.e IP of the kubernetes node and the portno

Default USERNAME and PASSWORD is admin

Click on Explore Right Hand Side.
And Select the source on the top right side of the page.
Now you can browse the logs.
Done...!

### Values documentation


#### Image tag

Docker image for Episilia which will be updated by the Episilia team.

<pre><code class="language-yaml">
imageTag: &release "1.0.0.RC3-20210922"
</code></pre>

#### Enabling server nodes

To enable the required servers.

Use these variable enables to  enable optimizer and historic search if needed.


  ```
  📝**Note**: episilia-log-indexer, episilia-search, episilia-gateway should be enabled for Episilia to work.
  ```


<pre><code class="language-yaml">
episilia-log-indexer:
  enabled: true
episilia-search:
  enabled: true
episilia-search-fixed:
  enabled: false
episilia-gateway:
  enabled: true
episilia-log-indexer-opt:
  enabled: false
grafana:
  enabled: true

</code></pre>

#### Installation specific configuration

<!--Every installation should proivide an unique client name and env as below-->
Each consumer will have unique client name and env provided, the same is to be used here.

<pre><code class="language-yaml">global:
  client:
    name: episilia-helm
    env: test-helm

</code></pre>

####  All common ops

<pre><code class="language-yaml">
ops:
    log:
      debug: on # Enable to get debug logs in all the servers
    cpanel:
      data:
        publish:
          interval:
            seconds: 300 # Time interval in which cpanel will be pushing metrics to console

</code></pre>

#### Kafka config

<pre><code class="language-yaml">
kafka:
    metadata:
      broker:
        list: localhost:9092 # The kafka broker
    group:      
      search: episilia-search-group   # Kafka consumer-group for search
      cpanel: episilia-cpanel-group   # Kafka consumer-group for cpanel
    topic:
      index:
        live: stagefiles # Topic for internal publish indexed files - stage.topic
        optimized: optfiles # Topic for internal optimize.topic:  publish file names post optimization
      optimize:
        request: stagefolder #optimize.request.topic send folders to optimize
      cpanel:
        in: cpaneld # Internal topic cpanel.data.topic
        out: cpaneld # Internal topic cpanel.data.topic
    indexer:
      logs:
        topics: logs # Topic from where logs are loaded.
      group: episilia-indexer-group  # Kafka consumer-group for indexer

</code></pre>

#### Datastore

Where indexed data is stored

<pre><code class="language-yaml">
datastore:
    s3:
      accesskey: ""  # filestorage access key (eg: AWS S3 access key)
      secretkey: ""  # filestorage secret key (eg: AWS S3 secret key)
      region: ""     # filestorage region (eg: AWS S3 region)
      endpoint:
        url: ""     # filestorage endpoint URL (eg: AWS S3 endpoint URL)
      sign:
        payload: true
      bucket: episilia-bucket  # filestorage bucket (eg: AWS S3 bucket)
      folder: episilia-folder  # filestorage folder URL (eg: AWS S3 folder)
      work:
        folder: work-folder   # Folder name to store s3 limit/write results
      url:
        prefix: s3://   # filestorage prefix URL (default to AWS S3)

</code></pre>

#### Indexer

Config for indexer and optimizer

<pre><code class="language-yaml">
indexer:
    image:
      repository: episilia/episilia-log-indexer    # docker image of episilia-log-indexer
      tag: *release
    replicaCount: "1"        # kubernetes pod replicas of episilia-log-indexer
    resources:
      limits:
        cpu: 800m            # cpu limit on episilia-log-indexer 
        memory: 1024Mi       # memory limit on episilia-log-indexer
      requests:
        cpu: 400m            # cpu request on episilia-log-indexer
        memory: 300Mi        # memory request on episilia-log-indexer


    logs:
      source: kafka         # source: kafka  # s3 or kafka
    schema:
      appid:
        fixed: "defaultApp"      # If appid is a fixed string
        keys: "project.app_id"   # label(s) for app identifier
      tenantid:        
        fixed: "defaultTenant"   # If tenantid is a fixed string  
        keys: ""                 # label(s) for tenant identifier        
      message:
        key: "log"               # actual log message key
      timestamp:
        key: "time"              # timestamp key
        formats: "%Y-%m-%dT%H:%M:%S"  #to specify timestamp format (ex: %Y-%m-%dT%H:%M:%S )
      exclude: "time"            # labels to be excluded from the list

  optimizer:
    replicaCount: "1"           # kubernetes pod replicas of episilia-optimizer  
    resources:
      limits:
        cpu: 800m              # cpu limit on episilia-optimizer
        memory: 1024Mi         # memory limit on episilia-optimizer
      requests:
        cpu: 500m              # cpu request on episilia-optimizer        
        memory: 300Mi          # memory request on episilia-optimizer

</code></pre>

#### Search

Config for search engine

<pre><code class="language-yaml">
search:
    image:
      repository: episilia/episilia-search   # docker image of episilia-search
      tag: *release
    replicaCount: "1"                        # kubernetes pod replicas of episilia-search 
    resources:
      limits:
        cpu: "1"                             # cpu limit on episilia-search
        memory: 2048Mi                       # memory limit on episilia-search
      requests:
        cpu: 500m                            # cpu request on episilia-search
        memory: 600Mi                        # memory request on episilia-search

    api:
      timeout:
        seconds: 40                         # timeout for search while querying
    
    live:
      from:
        hours: 48                           # hours from when the required index blocks should be loaded
      to:
        hours: 0                            # hours till when the required index blocks should be loaded, Note: value to be "0" to get instant logs
    labels:
      exclude: "@timestamp,log" # Lables excluded from grafana dropdown GUI. 

</code></pre>

Config for search engine

<pre><code class="language-yaml">
fixedSearch:
    bucket: ""             # s3 bucket for historic search to run parallelly, Note: if the value is empty it takes datastore.s3.bucket value as default
    folder: ""             # s3 folder for historic search to run parallelly, Note: if the value is empty it takes datastore.s3.folder value as default
    replicaCount: "1"      # kubernetes pod replicas of historic episilia-search 
    resources:
      limits:
        cpu: "1"          # cpu limit on historic episilia-search
        memory: 1024Mi    # memory limit on historic episilia-search
      requests:
        cpu: 500m         # cpu request on historic episilia-search
        memory: 600Mi     # memory request on historic episilia-search
    fixed:
      from:
        yyyymmddhh: "2021092100"    # the date from when the required index blocks should be loaded
      to:
        yyyymmddhh: "2021092202"    # the date till when the required index blocks should be loaded

    api:
      timeout:
        seconds: 40       # timeout for search while querying

    labels:
      exclude: "@timestamp,log" # Lables excluded from grafana dropdown GUI. 

</code></pre>

####  Gateway

<pre><code class="language-yaml">
gateway:
    image:
      repository: episilia/episilia-gateway
      tag: *release
    service:
      type: ClusterIP 
    replicaCount: "1"
    resources:
      limits:
        cpu: 500m
        memory: 600Mi
      requests:
        cpu: 300m
        memory: 200Mi 

    search:
      timeout:
        seconds: 40
 
</code></pre>

#### Control Panel

<pre><code class="language-yaml">
cpanel:
    ops:
      healthchecks:
        interval:
          mins: "5"
        exclude:
          list: ""
    api:
      access:
        key: token
        token: random
      post:
        server: "https://console.episilia.com/publish_cpanel_data"
      get:
        server: ""
        
</code></pre>

#### Persistance Volume 

<pre><code class="language-yaml">
persistence:
    enabled: false  
    mountPath: "/data"
    storageClassName: do-block-storage
    accessModes:
    - ReadWriteOnce
    size: "40Gi"
    historicSize: "20Gi"
    # annotations: {}
    finalizers:
     - kubernetes.io/pvc-protection
    # selectorLabels: {}
    # subPath: ""
    # existingClaim:

</code></pre>

### **Prerequisites**:
The following prerequisites are required to install Episilia.
```
1. Installing and configuring Helm (Helm CLI)
2. Kubectl CLI
3. A Kubernetes Cluster Up and Running
4. Setup Kafka (kafka IP address should be mentioned in Episilia)
      Topics:
         - Episilia-logs(kafka topic where logs are pushed)
         - Episilia_index_1(internal live topic)
         - Episilia_opt_1(internal optimized topic)
         - episilia-metrics
         - Episilia_opt_req	(internal optimizer request topic)
      The Logs to be pushed to this topic “episilia-logs” and only this topic can be overridden in the charts and others are   default internal topics.
5. Docker login (login into DockerHub)
6. Create Secret (To pull the images using secret, use regcred as default secret name) 
   kubectl create secret generic regcred --from-file=.dockerconfigjson=/home/Deskstop/.docker/config.json --type=kubernetes.io/  dockerconfigjson
7. S3 credentials where the Index and data files to be stored (aws accesskey, secret key, bucket name, folder name, region)
```

#### **System-Requisites:**
1. Check AVX support in the local Machine
   On linux (or unix machines) the information about your cpu is in /proc/cpuinfo. You can extract information from there by     hand, or with a grep command (grep flags /proc/cpuinfo).
Also most compilers will automatically define __AVX2__ so you can check for that too.
2. vCPUs - 2,Mem - 8gb (Preferably t2-large in AWS)

### **References**


**Kubectl CLI**
[kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

**Helm3 CLI**
[helm3](https://helm.sh/docs/intro/install/)

**Basic commands**
To upload a chart to Kubernetes, you can use the following command:
```
helm install [RELEASE NAME] [CHART]

helm install [CHART] —generate-name 

helm install [NAME] [CHART] —dry-run --debug

```
You can remove a chart repository:
```
helm repo remove | rm [NAME]
helm repo remove episilia
```
Before installing chart, if you want to see template of that chart 
```
helm  template [chart]
helm template episilia/episilia-cpanel
```
While Installing application we can pass keywords at runtime like below:
```
helm install episilia episilia/episilia-log-indexer --set image.repository=<imagename> --set image.tag=<tagname>
```
If you want to upgrade your chart, use the following command and choose the release you want:
```
helm upgrade [RELEASE] [CHART] 
helm upgrade episilia episilia/episilia-cpanel
```
You can add the flag -i or --install if you want to run an install before if a release by this name doesn’t already exist
Otherwise, you can do a rollback. If you do not specify the revision, it will roll back to the previous version. 
```
helm rollback [RELEASE] [REVISION]
helm rollback episilia --revision 1
```
You can see with this command all the historical revisions for a given release.
```
helm history [RELEASE]
```
If you want to uninstall a release, here is the command:
```
helm uninstall [RELEASE]
helm uninstall episilia
```




















