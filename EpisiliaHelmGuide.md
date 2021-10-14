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



#### Enabling server nodes

Use these variable enables to  enable optimizer and historic search if needed.

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

Every installation should proivide an unique client name and env as below

<pre><code class="language-yaml">global:
  client:
    name: episilia-helm
    env: test-helm

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




















