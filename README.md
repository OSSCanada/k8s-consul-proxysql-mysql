# Kubernetes Consul ProxySQL and MySQL setup

The goal of this project is to setup an application deployed to Kubernetes that can discover via Consul/Consul Connect a MySQL Group Replication Cluster running on a VM cluster outside of Kubernetes.  The MySQL cluster will be fronted by a ProxySQL cluster, and will be discoverable via service discovery from Consul.

## Preliminary thoughts

1. Use packer to create a base image
    - ensure:
      - MySQL is installed
      - Consul agent is installed
    - run cloudinit scripts to initialize the MySQL Group Replication cluster
      - check to see if current node is ```<node-name>-#``` e.g. ```mysql-node-1```
        - run initial MySQL group replication setup for first node
        - output/save the config string for all other nodes
      - check to see if current node 
        - use output from first node to join the cluster
1. Use packer to create a base image
    - ensure:
      - ProxySQL is installed
      - Consul agent is installed
    - run init scripts to stand up the ProxySQL cluster

### Kubernetes
1. Deploy AKS cluster
1. Install Consul via Helm
1. Deploy base app that will connect to the MySQL backend
1. Ensure consul agent/proxy is deployed as sidecar along side app
    - enforce mTLS communication
    - enforce service mesh policies (App can communicate with MySQL backend)

## Future
1. Add Vault integration to dynamically get MySQL user credentials
    - reduces the blast radius should a node and it's credentials get compromised
    - allows for auditing/logging for who has tried to gain access