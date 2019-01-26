# sahara-cdh-5.11.0
Templates for deploying Cloudera CDH v5.11.0 using OpenStack Data Processing (Sahara)


NOTE: You need to either obtain or build a CDH v5.11.0 image before hand. [RHOSP 13 Doc](https://access.redhat.com/documentation/en-us/red_hat_openstack_platform/13/html-single/openstack_data_processing/)

Register and Tag your CDH image
```bash
$ openstack dataprocessing image register --username cloud-user cdh5110
$ openstack dataprocessing image register --username cloud-user cdh5110
```

NOTE: You must change any UUID references and Flavor IDs in the templates to match your environment

Create Node Group Templates
```bash
$ openstack dataprocessing node group template create --json manager.json
$ openstack dataprocessing node group template create --json master-core.json
$ openstack dataprocessing node group template create --json master-additional.json
$ openstack dataprocessing node group template create --json worker-nm-dn.json
```

Create Cluster Template
```bash
$ openstack dataprocessing cluster template create --json cluster.json
```

You can now lauch your clusters using the cluster template via CLI, Horizon, or API


Once the CDH cluster is up, I found that clock offset health check was failing even though NTP was configured correctly.
I just restarted the cloudera agent on each node to correct. Replace the numbers in the for loop with the last octet of your node IPs`

```bash
$ for i in {89,72,92,74,75,88}; do ssh cloud-user@192.168.1.$i 'sudo systemctl restart cloudera-scm-agent' ; done
```
