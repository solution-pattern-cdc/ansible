### Using Change Data Capture for Stack Modernization: Ansible playbook

This repo contains an Ansible playbook to install the demo described in the Solution Pattern [Using Change Data Capture for Stack Modernization](https://redhat-solution-patterns.github.io/solution-pattern-modernization-cdc).

**Prerequisites**

* Access to an OpenShift cluster (version >= 4.9) with _cluster-admin_ privileges
* `oc` OpenShift CLI installed
* Logged in into the OpenShift cluster with `oc` as _cluster-admin_
* Ansible - the playbook was tested with version 2.9
* Ansible `kubernetes.core` module.
  This module can be installed with `ansible-galaxy`:
  ```
  $ ansible-galaxy collection install kubernetes.core
  ```

**Preparation**

This demo uses the OpenShift Streams for Apache Kafka managed Kafka service. Before running the playbook you need to provision and configure a Kafka instance.

Detailed instructions for provisioning and configuring a managed Kafka instance can be found [here](https://redhat-scholars.github.io/managed-kafka-workshop/managed-kafka-workshop/main/index.html).

Short instructions:
1. Navigate to [console.redhat.com](https://console.redhat.com) and log in with your Red Hat Account ID.
1. On the [console.redhat.com](https://console.redhat.com) landing page, select *Application Services -> Streams for Apache Kafka -> Kafka instances*
1. Create a Kafka instance
1. Create a Service Account to connect to your Kafka instance
1. Configure the ACL for your Service Account. The Service Account should have the following permissions:
    * `read`, `write`, `create` permissions for all topics
    * `read` permissions for all consumer groups
1. Create the following topics:
    * `retail.sale-aggregated`, with 1 partition
    * `retail.updates.public.customer`, with 1 partition
    * `retail.updates.public.product`, with 1 partition
    * `retail.updates.public.sale`, with 1 partition
    * `retail.updates.public.line_item`, with 1 partition
    * `retail.expense-event`, with 1 partition
    > **Note:** The `retail.updates.*` topics contain the change events captured by Debezium. Normally these topics are auto-created by Debezium. For this demo they are created upfront to ensure that all components of the application start up correctly without data in the database.

**Running the playbook**

1. Clone this repository to your workstation
1. Copy the `inventories/inventory.template` file to `inventories/inventory`.
1. In the `inventories/inventory` file, provide the connection details for your Kafka instance:
    * **rhosak_bootstrap_server**: Bootstrap server of your managed Kafka instance
    * **rhosak_service_account_client_id**: Client ID of your Service Account
    * **rhosak_service_account_client_secret**: Client Secret of your Service Account
1. Run the Ansible playbook.
    ```
    $ ansible-playbook -i inventories/inventory playbooks/install.yml
    ```
    The different components of the demo are installed in the `retail` namespace on your OpenShift cluster.  
    The Ansible playbook is idempotent, so in case of an issue, fix the issue and run the playbook again.

