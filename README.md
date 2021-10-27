# QuickELK

Quick Code to deploy ELK stack (Elasticsearch, Logstash, Kibana) and supporting services on Kubernetes Controlled Docker platform.

**Prerequisite**
- Kubernets Cluser with minimum 3 workers, to host 3 Elasticsearch nodes.
- Docker Based Container Runtime Environment.
- Helm Installed and Configured to access Kubernetes Cluster Master for deployments.
- Ansible Installed and Configured to run playbook.
- Sufficent resource over kubernetes worker for the entire deployment.


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
![Deployment Diagram](/ELK_Dia.png)

### Installing via Ansible
