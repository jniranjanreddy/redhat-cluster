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
## Here is the output
```
[root@redclus01 ~]# yum install pcs pacemaker fence-agents-all
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.aktkn.sg
 * extras: mirror.aktkn.sg
 * updates: mirror.aktkn.sg
 
[root@redclus01 ~]# systemctl disable firewalld
Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.

[root@redclus01 ~]# systemctl stop firewalld

[root@redclus01 ~]# passwd hacluster
Changing password for user hacluster.
New password:
BAD PASSWORD: The password fails the dictionary check - it is based on a diction                                                                                        ary word
Retype new password:
passwd: all authentication tokens updated successfully.

[root@redclus01 ~]# systemctl start pcsd.service
[root@redclus01 ~]# systemctl enable pcsd.service
Created symlink from /etc/systemd/system/multi-user.target.wants/pcsd.service to                                                                                         /usr/lib/systemd/system/pcsd.service.
[root@redclus01 ~]# pcs cluster auth redclus01 redclus02
Username: hacluster
Password:
redclus01: Authorized
redclus02: Authorized
[root@redclus01 ~]# pcs cluster setup --start --name my_cluster redclus01 redclu                                                                                        s02
Destroying cluster on nodes: redclus01, redclus02...
redclus01: Stopping Cluster (pacemaker)...
redclus02: Stopping Cluster (pacemaker)...
redclus01: Successfully destroyed cluster
redclus02: Successfully destroyed cluster

Sending 'pacemaker_remote authkey' to 'redclus01', 'redclus02'
redclus01: successful distribution of the file 'pacemaker_remote authkey'
redclus02: successful distribution of the file 'pacemaker_remote authkey'
Sending cluster config files to the nodes...
redclus01: Succeeded
redclus02: Succeeded

Starting cluster on nodes: redclus01, redclus02...
redclus01: Starting Cluster (corosync)...
redclus02: Starting Cluster (corosync)...
redclus01: Starting Cluster (pacemaker)...
redclus02: Starting Cluster (pacemaker)...

Synchronizing pcsd certificates on nodes redclus01, redclus02...
redclus01: Success
redclus02: Success
Restarting pcsd on the nodes in order to reload the certificates...
redclus01: Success
redclus02: Success

[root@redclus01 ~]# pcs status
Cluster name: my_cluster

WARNINGS:
No stonith devices and stonith-enabled is not false

Stack: corosync
Current DC: redclus01 (version 1.1.23-1.el7_9.1-9acf116022) - partition with quorum
Last updated: Mon Oct 17 06:12:44 2022
Last change: Mon Oct 17 06:11:57 2022 by hacluster via crmd on redclus01

2 nodes configured
0 resource instances configured

Online: [ redclus01 redclus02 ]

No resources


Daemon Status:
  corosync: active/disabled
  pacemaker: active/disabled
  pcsd: active/enabled

[root@redclus01 ~]# pcs cluster enable --all
redclus01: Cluster Enabled
redclus02: Cluster Enabled
[root@redclus01 ~]# pcs status
Cluster name: my_cluster

WARNINGS:
No stonith devices and stonith-enabled is not false

Stack: corosync
Current DC: redclus01 (version 1.1.23-1.el7_9.1-9acf116022) - partition with quorum
Last updated: Mon Oct 17 06:14:18 2022
Last change: Mon Oct 17 06:11:57 2022 by hacluster via crmd on redclus01

2 nodes configured
0 resource instances configured

Online: [ redclus01 redclus02 ]

No resources

Daemon Status:
  corosync: active/enabled
  pacemaker: active/enabled
  pcsd: active/enabled


```
