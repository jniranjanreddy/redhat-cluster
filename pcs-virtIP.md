## How to create Virtual IP
```
[root@redclus01 ~]# pcs resource create VirtIP IPAddr ip=192.168.9.112 cidr_netmask=24 op monitor interval=60
Assumed agent name 'ocf:heartbeat:IPaddr' (deduced from 'IPAddr')

[root@redclus01 ~]# pcs resource create Httpd apache Configuration="/etc/httpd/conf/httpd.conf" op monitor interval=60 --force
Assumed agent name 'ocf:heartbeat:apache' (deduced from 'apache')
Warning: invalid resource option 'Configuration', allowed options are: client, configfile, envfiles, httpd, options, port, statusurl, testconffile, testname, testregex, testregex10, testurl, trace_file, trace_ra, use_ipv6

[root@redclus01 ~]# pcs status
Cluster name: my_cluster

WARNINGS:
No stonith devices and stonith-enabled is not false

Stack: corosync
Current DC: redclus01 (version 1.1.23-1.el7_9.1-9acf116022) - partition with quorum
Last updated: Tue Oct 18 08:56:03 2022
Last change: Tue Oct 18 08:55:31 2022 by root via cibadmin on redclus01

2 nodes configured
2 resource instances configured

Online: [ redclus01 redclus02 ]

Full list of resources:

 VirtIP (ocf::heartbeat:IPaddr):        Stopped
 Httpd  (ocf::heartbeat:apache):        Stopped

Daemon Status:
  corosync: active/enabled
  pacemaker: active/enabled
  pcsd: active/enabled

[root@redclus01 ~]# pcs constraint colocation add Httpd with VirtIP INFINITY
[root@redclus01 ~]# pcs property set stonith-enabled=false
[root@redclus01 ~]# pcs property set no-quorum-policy=ignore
[root@redclus01 ~]# pcs resource defaults resource-stickness="INFINITY"
Warning: Defaults do not apply to resources which override them with their own defined values


```
