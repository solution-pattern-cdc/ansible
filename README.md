### Using Change Data Capture for Stack Modernization: Ansible playbook

This repo contains an Ansible playbook to install the demo described in the Solution Pattern [Using Change Data Capture for Stack Modernization](https://redhat-solution-patterns.github.io/solution-pattern-modernization-cdc).

The playbook leverages OpenShift Pipelines to deploy the solution pattern as a ArgoCD application.

**Playbook roles**

The playbook executes the foollowing roles:

* Installation of the Red Hat AMQ Streams operator
* Installation of the ElasticSearch (ECK) certified operator
* Installation of the Red Hat Gitops operator, and a cluster-scoped ArgoCD instance
* Deployment of the Solution pattern as a ArgoCD application. This includes:
  * The retail database which stores customers, products and orders.
  * An AMQ Streams Kafka cluster.
  * Kafka Connect with the Debezium Connector.
  * An ElasticSearch instance.
  * Integration microservices.
  * The Cashback service application.

**Prerequisites**

* Access to an OpenShift cluster (version >= 4.12) with _cluster-admin_ privileges
* `oc` OpenShift CLI installed
* Logged in into the OpenShift cluster with `oc` as _cluster-admin_
* Ansible - the playbook was tested with version 2.9
* Ansible `kubernetes.core` module.
  This module can be installed with `ansible-galaxy`:
  ```
  $ ansible-galaxy collection install kubernetes.core
  ```

**Running the playbook**

1. Clone this repository to your workstation
1. Run the Ansible playbook.
    ```
    $ ansible-playbook playbooks/install.yml
    ```
    The different components of the demo are installed in the `retail` namespace on your OpenShift cluster.
    The Ansible playbook is idempotent, so in case of an issue, fix the issue and run the playbook again.
