# How to install Redhat Cluster

```
DNS
Snapshot
yum install pcs pacemaker fence-agents-all
firewall-cmd  --add-service=high-availability --permanent
passwd hacluster 
systemctl start pcsd.service    
systemctl enable pcsd.service
pcs cluster auth bing1 bing2
pcs cluster setup --start --name my_cluster bing1 bing2
pcs cluster enable --all
```
