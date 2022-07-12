# 

# NFS Mounting via Execution Node and Execution Environment

## Execution Node

### Requirements:

Install NFS Clients on EE Nodes
- `yum install -y nfs-utils rpcbind`

Run mount command: 

`mount -t nfs 192.168.56.12:/mnt/nfs_shares/docs -o rw,context="system_u:object_r:container_file_t:s0" /ansible --make-shared`

Change AAP 'Job Settings' `isolated directories`:

Paths to expose to isolated jobs
```json
	[	
	  "/ansible:/ansible:rw",
	  "/etc/pki/ca-trust:/etc/pki/ca-trust:O",
	  "/usr/share/pki:/usr/share/pki:O"
	]		
```

Directory Permissions and Owner for mountpoint
EE Node - 
- (unverified)- But these are changes I made

	`chown <recursive> awx:awx /ansible`
	`chmod 775 /ansible`


Container -
- Auto assigns root..
