# Deploying with Ansible
* .yaml files are ansible, .yml files are k8s manifests
* envs.yaml contains most key settings used in the deployment


# Quick Deploy
```ansible-playbook ansible-bitbucket.yaml -e @envs.yaml```

# Customizing Options
This guide relies on NFS volumes and static node-port type load balancers. These can be changed. The Bitbucket docker container accepts several ENV variables to configure JVM or Tomcat HTTP options. Listed Below:


##### Namespace
* Target K8s Namespace

```devops-bitbucket-ns```

##### Bambooservicename
* Service label used to gather objects 

```devops-bitbucket-service```

##### Appname
* Name of the application 

```bitbucket```

##### jvmlow
* Xms value. Recommended to set the name. Not too big!

```1536m```

##### jvmhigh
* Xmx. Set as Above

##### bitbucketpv
* Bamboo Persistent Volume used in deployment

```bamboopv```
Git & NFS can be a bad mix. This is an example only. A recommended deployment would be to have NFS mounted as hostDirs on each k8s worker node, and using local paths to mount this.
This will ensure the proper NFS mount settings can be applied such as noatime,sync,posix and W/R sizes. This is a sample only!

##### bitbucketvolumeclaim
* K8s PVC object to claim the volume

```bitbucketnnfspvc```

##### bitbucketvolumename
* Used in Deployment

```bitbucket-vol```

##### Storagesize
* Capacity in GB for the Bamboo Home 

```10Gi```

##### nfsserverip
* IP or FQDN for NFS Host

```10.1.1.22```

##### nfspath
* NFS Mount Path ( nfs export )

```/data/bitbucket-prod/```

##### bambook8servicename
* used with k8s service and ingress controllers

```devops-bitbucket-svc```

##### nodeip1,2
* Used with local load balancers or node ports to expose services outside without ingress controllers
* typically takes the node-port of each node in the k8s cluster

```10.1.1.25```

##### Topcat properties
* tomcat properties are set with the following values in the envs.yaml

```PROXYNAME```


```PROXYSCHEME```


```PROXYSECURE```


* Proxy Name is your FQDN in which your reverse proxy/load balancer listens for the service, such as bamboo.company.com
* Proxy Scheme tells tomcat if this is HTTP or HTTPS
* Proxy Secure informs tomcat if SSL is in use and allows for better re-writing and security. This value is hard coded into the deployment.yml due to bugs between ansible and go templates not reading this value correctly. If HTTPS, set this to "true", if HTTP, set this to "false"
