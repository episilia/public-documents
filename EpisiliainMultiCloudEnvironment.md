## **Google Cloud (GCP):**
Step 1:  Create K8s cluster GKE with 3 nodes each node of 2vcpu and 8gb ram\
Step 2:  Create cloud storage(bucket) and a folder.\
Step 3:  Get the access key, secret key, and end point URL from -cloud storage---> settings-->Interoperability---> end point URL; access key and secret key and set your project name as default. (If you have admin access)\
📝 **Note**: Create a service account and get access key and secret key and give permissions as k8s engine admin and cloud storage admin.\
Step 4:   Add helm repository and update cpanel values file with access and sec key, bucket name, folder name, kafka server ip, region, end point URL and topic name.\
Step 5: Install/deploy episilia and test.\

## **Alibaba Cloud:**
Step 1: Create VPC by choosing Virtual Private Cloud service.\
Step 2: Create K8s cluster ACK (container service for Kubernetes) with 3 nodes each node of 4vcpu and 8gb ram and choose the vpc created before while creating cluster.\
Step 3:  Create bucket by getting into OSS (Object Storage Service) and create a folder.\
Step 4:  Give permissions for the bucket to read/write to the bucket.\
📝 **Note**:  For permissions visit Object Storage Service/Buckets/select-your-bucket/Bucket Policy/configure/Authorize/select Anonymous accounts/select operation.\
Step 5:  Get the end point URL, bucket name and folder name.\
Step 6: Add helm repository and update cpanel values file with the above values and install/deploy episilia and test.
