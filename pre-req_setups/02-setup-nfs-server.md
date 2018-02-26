Setup a NFS Server/Client on RaspberryPi 3

## Setup the Server Side - Disks and Directories

Prepare the directories:

```bash
$ sudo mkdir -p /opt/nfs
$ sudo chown nobody:nogroup /opt/nfs
$ sudo chmod 755 /opt/nfs
```

## Setup the Server Side - Installing NFS Server

Install the NFS Server packages:

```bash
$ sudo apt install nfs-kernel-server nfs-common rpcbind -y
```

Configure the paths in `/etc/exports`, setup our path that we would like to be accessible via NFS:

```bash
/opt/nfs *(rw,sync,no_subtree_check)
# or
/opt/nfs <cidr>(rw,sync,no_subtree_check)
```

Export the config, enable the services on boot and start NFS:

```bash
$ sudo exportfs -ra
$ sudo systemctl enable rpcbind
$ sudo systemctl enable nfs-kernel-server
$ sudo systemctl enable nfs-common
$ sudo systemctl start rpcbind
$ sudo systemctl start nfs-kernel-server
$ sudo systemctl start nfs-common
```

NFS will be mounted on each node, so that is why we need the nfs-common package on the NFS server as well.

## Setup the NFS Client

On the client install the NFS Client packages:

```bash
$ sudo apt install nfs-common -y
```

Create the mountpoint of choice and change the ownership:

```bash
$ sudo chown pi:pi /mnt
```

Mount the NFS Share to your local mount point:

```bash
$ sudo mount 192.168.1.2:/opt/nfs /mnt
```

Enable mount on boot via `/etc/fstab`:

```bash
192.168.1.2:/opt/nfs /mnt nfs rw 0 0
```

Now we have a central server for our data persistence services. As at this moment, its a single point of failure, 
you can also setup GlusterFS.
