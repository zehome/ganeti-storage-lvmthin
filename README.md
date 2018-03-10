INSTALLATION
============

Create an LVM thin pool:
```
lvcreate -L 10G rootvg -T thinpool
```

Login to master node over ssh.
Enable "ext" disk template in the ganeti cluster.

```
git clone git@github.com:zehome/ganeti-storage-lvmthin
ln -s $(realpath ganeti-storage-lvmthin) /usr/share/ganeti/extstorage/lvmthin
```

Do this on every other node in the cluster too.
When the installation is finished check the installation from master:

```
gnt-storage diagnose
```

When everything is fine this should show something like this:

```
Provider: lvm
---
Node: 1b5aa798-1c07-4fda-ba11-5f6ad66b857b, status: valid (path: /usr/share/ganeti/extstorage/lvmthin) [parameters: vgname, thinpool]
Node: 4cc09080-46d1-4756-b9e7-d4ce90887553, status: valid (path: /usr/share/ganeti/extstorage/lvmthin) [parameters: vgname, thinpool]
Node: 27a0c840-d08e-4ea4-8748-08f9a274c479, status: valid (path: /usr/share/ganeti/extstorage/lvmthin) [parameters: vgname, thinpool]
Node: d700d1da-2b1a-4038-ad55-643795680906, status: valid (path: /usr/share/ganeti/extstorage/lvmthin) [parameters: vgname, thinpool]
--
For nodegroup: default --> valid
```


CREATE AN INSTANCE
------------------

Make sure all nodes in the cluster have access to the same volume group (check with vgs).
Now create your first shared lvm instance...

```
gnt-instance add -t ext --disk 0:size=20G,provider=lvm,vgname=VOLUMEGROUPNAME,thinpool=THINPOOLNAME ... INSTANCE
```

