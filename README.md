# QuickELK

Quick Code to deploy ELK stack (Elasticsearch, Logstash, Kibana) and supporting services on Kubernetes Controlled Docker platform.

**Prerequisite**
- Kubernets Cluser with minimum 3 workers, to host 3 Elasticsearch nodes.
- Docker Based Container Runtime Environment.
- Helm Installed and Configured to access Kubernetes Cluster Master for deployments.
- Ansible Installed and Configured to run playbook.
- Sufficent resource over kubernetes worker for the entire deployment.
- External Load Balancer(Metal LB) need to be configured over Kubernetes, to access Kubernetes GUI from LoadBalancer Service.


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
ansible-playbook main.yaml -e namespace=<Namespace for Deployment> -e stackname=<Name of Stack> -e chartpath=elk-stack
```

Example:
```bash
ansible-playbook main.yaml -e namespace=ezsinam1 -e stackname=mystack -e chartpath=elk-stack

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
helm install <Stack Name> elk-stack --namespace <Namespace for Deployment> --create-namespace
```
### Installation Validation via Kubectl
Run below command for deployment validation, provide the namespace provided in previous step.
```bash
kubectl get all -n <Namespace for Deployment>
```

Example:
```bash
kubectl get all -n ezsinam1

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
