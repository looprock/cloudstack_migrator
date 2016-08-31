# cloudstack_migrator
Migrate guest VMs from one host to another, including local data volumes

# Assumptions

This script was written to work with hosts running under KVM with local storage pools on each node. If you're doing something else, your mileage may vary.

# Usage
Unfortunately, this script uses both the [cloudstack API client](https://github.com/vast-engineering/cloudstack-python-client) and cloudmonkey. The only other strange dependency is paramiko.

The reason for this is because I started out using only the client and, due to some issues, eventually reverted to cloudmonkey for most of the heavy lifting. Sorry!

Set up passwordless ssh authentication and populate known_hosts files for all the servers in your cluster so they can transfer the data volume disk images between them.

If you're running the script from a host which isn't part of the cluster, you'll also need to allow passwordless ssh from that server.

Next, configure the script:

```
# cloudstack API config
csapi = 'https://cloudstack.company.com/client/api'
admin_apikey = 'xxxx-xxxx-xxxx-xxxx-xxxx'
admin_secret = 'xxxx-xxxx-xxxx-xxxx-xxxx'

# information for generating the temporary VM used to mount/unmount volumes
zoneid = 'xxxx-xxxx-xxxx-xxxx-xxxx'
networkid = 'xxxx-xxxx-xxxx-xxxx-xxxx'
serviceofferingid = 'xxxx-xxxx-xxxx-xxxx-xxxx'
templateid = 'xxxx-xxxx-xxxx-xxxx-xxxx'

# logfile target: make sure this exists
logdir = "/var/log/migrations"
```

If properly configured you should be provided with a list of hosts to select the source and target destinations from.

# options

```
(cloudstack)[rundeck.oak:~]# migrateguests -h
Usage: migrateguests [options]

Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -s, --skipdata        don't migrate systems with data volumes
  -d, --dry             do a dry run but don't migrate anything
  -e EXCLUDE, --exclude=EXCLUDE
                        Comma separated list of VM ids to exclude
```

Good luck!
