# QuickELK

Quick Code to deploy ELK stack (Elasticsearch, Logstash, Kibana) and supporting services on Kubernetes Controlled Docker platform.

**Prerequisite**
- Kubernets Cluser with minimum 3 workers, to host 3 Elasticsearch nodes.
- Docker Based Container Runtime Environment.
- Helm Installed and Configured to access Kubernetes Cluster Master for deployments.
- Ansible Installed and Configured to run playbook.
- Sufficent resource over kubernetes worker for the entire deployment.
- External Load Balancer(Eg. Metal LB) need to be configured over Kubernetes, to access Kubernetes GUI from LoadBalancer Service.

**Consideration**

Docker images hosted at official "docker.elastic.co" are valid, will be used for ELK deployments. Below are the docker repo used for deployment:
  - **ElasticSearch**: docker.elastic.co/elasticsearch/elasticsearch
  - **Kibana**: docker.elastic.co/kibana/kibana
  - **Filebeat**: docker.elastic.co/beats/filebeat
  - **Logstash**: docker.elastic.co/logstash/logstash
  - **Metricbeat**: docker.elastic.co/beats/metricbeat 

## Tools and Technologies Used
- **Kubernetes**:
    - Kubernetes, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.
- **Docker**:
    - Docker is a set of platform as a service products that use OS-level virtualization to deliver software in packages called containers.
- **Ansible**:
    - Ansible is an open-source software provisioning, configuration management, and application-deployment tool enabling infrastructure as code.
- **Helm**:
    - Helm helps you manage Kubernetes applications — Helm Charts help you define, install, and upgrade even the most complex Kubernetes application.
- **Elasticsearch**: 
    - Elasticsearch provides near real-time search and analytics for all types of data. Whether you have structured or unstructured text, numerical data, or geospatial data, Elasticsearch can efficiently store and index it in a way that supports fast searches.
- **Logstash**:
    - Logstash is a free and open server-side data processing pipeline that ingests data from a multitude of sources, transforms it, and then sends it to Elasticsearch.
- **Kibana**:
    - Kibana is a free and open user interface that lets you visualize your Elasticsearch data and navigate the Elastic Stack.
- **Filebeat**:
    - Lightweight shipper for logs Whether you’re collecting from security devices, cloud, containers, hosts, or OT, Filebeat helps you keep the simple things simple by offering a lightweight way to forward and centralize logs and files. 
- **Metricbeat**:
    - Lightweight shipper for metrics Collect metrics from your systems and services. From CPU to memory, Redis to NGINX, and much more, Metricbeat is a lightweight way to send system and service statistics.
- **Github**:
    - GitHub, Inc. is a provider of Internet hosting for software development and version control using Git. It offers the distributed version control and source code management functionality of Git, plus its own features.

## Installation and Usage

### Target Architecture Diagram
![Deployment Diagram](/ELK_DiaNew.png)

### Installing via Ansible

Clone the "QuickELK" project, ensure that all Prerequisite are in place. Select a kubernetes namespace where this stack is required to be created. Namespace will be automatically created if not exist. Select a helm deployment name for the stack. Run below command from inside the clone directory.

```bash
# ansible-playbook main.yaml -e namespace=<Namespace for Deployment> -e stackname=<Name of Stack> -e chartpath=elk-stack
```

Example:
```bash
# ansible-playbook main.yaml -e namespace=ezsinam1 -e stackname=mystack -e chartpath=elk-stack

Output: 
 [WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'


PLAY [localhost] *****************************************************************************************************************************************************

TASK [Check the Namespace Exist.] ************************************************************************************************************************************
fatal: [localhost]: FAILED! => {"changed": true, "cmd": "kubectl get ns ezsinam1", "delta": "0:00:00.136403", "end": "2021-10-27 14:11:27.742760", "msg": "non-zero return code", "rc": 1, "start": "2021-10-27 14:11:27.606357", "stderr": "Error from server (NotFound): namespaces \"ezsinam1\" not found", "stderr_lines": ["Error from server (NotFound): namespaces \"ezsinam1\" not found"], "stdout": "", "stdout_lines": []}

TASK [Create Namespace if NOT exists.] *******************************************************************************************************************************
changed: [localhost]

TASK [debug] *********************************************************************************************************************************************************
ok: [localhost] => {
    "ns.stdout_lines": [
        "namespace/ezsinam1 created"
    ]
}

TASK [Deploy the ELK Stack.] *****************************************************************************************************************************************
changed: [localhost]

TASK [debug] *********************************************************************************************************************************************************
ok: [localhost] => {
    "chart.stdout_lines": [
        "NAME: mystack",
        "LAST DEPLOYED: Wed Oct 27 14:11:28 2021",
        "NAMESPACE: ezsinam1",
        "STATUS: deployed",
        "REVISION: 1",
        "NOTES:",
        "1. Get the application URL by running these commands:"
    ]
}

PLAY RECAP ***********************************************************************************************************************************************************
localhost                  : ok=4    changed=2    unreachable=0    failed=1
```
### Installing via Helm
Clone the "QuickELK" project, ensure that all Prerequisite are in place. Select a kubernetes namespace where this stack is required to be created. Namespace will be automatically created if not exist. Select a helm deployment name for the stack. Run below command from inside the clone directory.

```bash
# helm install <Stack Name> elk-stack --namespace <Namespace for Deployment> --create-namespace
```
### Installation Validation via Kubectl
Run below command for deployment validation, provide the namespace provided in previous step.
```bash
# kubectl get all -n <Namespace for Deployment>
```

Example:
```bash
# kubectl get all -n ezsinam1

Output:
NAME                                       READY   STATUS    RESTARTS   AGE
pod/elasticsearch-logging-0                1/1     Running   0          4m7s
pod/elasticsearch-logging-1                1/1     Running   0          4m7s
pod/elasticsearch-logging-2                1/1     Running   0          4m7s
pod/filebeat-4kc6r                         1/1     Running   0          4m7s
pod/filebeat-5b9pz                         1/1     Running   0          4m7s
pod/filebeat-cxwpj                         1/1     Running   0          4m7s
pod/filebeat-fqth7                         1/1     Running   0          4m7s
pod/filebeat-mgtts                         1/1     Running   0          4m7s
pod/filebeat-rdgx8                         1/1     Running   0          4m7s
pod/filebeat-rgxmc                         1/1     Running   0          4m7s
pod/kibana-logging-58db4c758f-gbb6k        1/1     Running   2          4m7s
pod/logstash-deployment-5b4879b9f7-4zwnr   1/1     Running   0          4m7s
pod/metricbeat-6kp4k                       1/1     Running   0          4m7s
pod/metricbeat-dqf7c                       1/1     Running   0          4m7s
pod/metricbeat-gj78v                       1/1     Running   0          4m7s
pod/metricbeat-kgbfg                       1/1     Running   0          4m7s
pod/metricbeat-ngb9q                       1/1     Running   0          4m7s
pod/metricbeat-tw7zb                       1/1     Running   0          4m7s
pod/metricbeat-x4tv4                       1/1     Running   0          4m7s

NAME                            TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)          AGE
service/elasticsearch-logging   ClusterIP      10.108.33.4    <none>          9200/TCP         4m7s
service/kibana-logging          LoadBalancer   10.108.8.123   10.64.109.113   5601:30316/TCP   4m7s
service/logstash-service        ClusterIP      10.105.7.173   <none>          5044/TCP         4m7s

NAME                        DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
daemonset.apps/filebeat     7         7         7       7            7           <none>          4m7s
daemonset.apps/metricbeat   7         7         7       7            7           <none>          4m7s

NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/kibana-logging        1/1     1            1           4m7s
deployment.apps/logstash-deployment   1/1     1            1           4m7s

NAME                                             DESIRED   CURRENT   READY   AGE
replicaset.apps/kibana-logging-58db4c758f        1         1         1       4m7s
replicaset.apps/logstash-deployment-5b4879b9f7   1         1         1       4m7s

NAME                                     READY   AGE
statefulset.apps/elasticsearch-logging   3/3     4m7s
```

### Integration Validation
To validate indeces for ElasticSearch to validate Integration with filebeat, logstash and metricbeat.
```bash
# curl <elasticsearch-logging clsuetrIP Service IP>:9200/_cat/indices
```

Example:
```bash
# curl 10.108.33.4:9200/_cat/indices
yellow open logstash-2021.07.08          sxXdBTWZSt6dIaDB4s_QLQ 5 1   218 0 388.2kb 388.2kb
yellow open logstash-2021.08.05          H37edbWUT4enUQfF---t_A 5 1     6 0  62.7kb  62.7kb
yellow open logstash-2021.09.05          vuQnUkGMTLWy85c9LSr-SA 5 1   681 0 458.8kb 458.8kb
yellow open logstash-2021.07.05          Im1jancJQlWfwUiC8h0vKw 5 1   717 0 773.6kb 773.6kb
yellow open logstash-2021.07.02          b9AFqpcTTkW5nmqoqaCsig 5 1 27535 0   6.6mb   6.6mb
yellow open metricbeat-6.8.20-2021.10.27 iiVkm-lMQwKT6ElVwpukGw 5 1 17020 0  12.9mb  12.9mb
...
...
```

MetricBeat and LogStash indices should be visible. Also you can select any index and check data using below command.

Example:
```bash
# curl 10.108.33.4:9200/logstash-2021.07.08/_search

```
### Installation Cleanup
To delete entire deployment provide kubernetes namespace where this stack is created. Namespace will be automatically deleted. Also provide helm deployment name of the created stack. Run below command.
```bash
# ansible-playbook  clean.yaml -e namespace=<Namespace of Deployment> -e stackname=<Name of Deployed Stack>
```
Example:
```bash
# ansible-playbook  clean.yaml -e namespace=ezsinam1 -e stackname=mystack

Output:
 [WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'


PLAY [localhost] *****************************************************************************************************************************************************

TASK [Remove the ELK Stack.] *****************************************************************************************************************************************
changed: [localhost]

TASK [debug] *********************************************************************************************************************************************************
ok: [localhost] => {
    "chart.stdout_lines": [
        "release \"mystack\" uninstalled"
    ]
}

TASK [Delete the Namespace.] *****************************************************************************************************************************************
ansible-playbook  clean.yaml -e namespace=ezsinam1 -e stackname=mystackchanged: [localhost]

TASK [debug] *********************************************************************************************************************************************************
ok: [localhost] => {
    "ns.stdout_lines": [
        "namespace \"ezsinam1\" deleted"
    ]
}

PLAY RECAP ***********************************************************************************************************************************************************
localhost                  : ok=4    changed=2    unreachable=0    failed=0

```
## Backup Strategy 
- **Elasticsearch**: Snapshot and Restore
    - A snapshot is a backup taken from a running Elasticsearch cluster. You can take snapshots of an entire cluster, including all its data streams and indices. You can also take snapshots of only specific data streams or indices in the cluster. You must register a snapshot repository before you can create snapshots. Snapshots can be stored in either local or remote repositories. Remote repositories can reside on Amazon S3, HDFS, Microsoft Azure, Google Cloud Storage, and other platforms supported by a repository plugin.
- **Kibana**: 
    - Dashboard and configuration are important for Kibana, Dashboards could be exported and source control managed. Configuration could backed up externally on PV or NFS. Rest of the instance could be treated as Cow.
- **Logstash**:
    - Logstash will act as middle layer in between Logging source and Elastic Search, no specific backup measures required. Keep the backup of its configuration instead. Could be treated as a Cow.
- **Metricbeat**:
    - Metricbeat will act as middle layer in between Metric source and Elastic Search, no specific backup measures required. Keep the backup of its configuration instead. Could be treated as a Cow.
- **Filebeat**:
    - Filebeat will act as middle layer in between Logging source and Elastic Search, no specific backup measures required. Keep the backup of its configuration instead. Could be treated as a Cow.




