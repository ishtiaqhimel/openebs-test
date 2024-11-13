## Replicated PV Mayastor

OpenEBS Replicated Persistent Volumes (PV) backed by Mayastor Storage.

## Prerequisites

All worker nodes must satisfy the following requirements:

- `x86-64` CPU cores with `SSE4.2` instruction support

```bash
$ cat /proc/cpuinfo | grep sse4_2 # check if `SSE4.2` is supported or not
```

- Linux kernel `5.13` or higher (Recommended)

```bash
$ uname -r
```

- The kernel should have the following modules loaded: `nvme-tcp`, `ext4` and optionally `xfs`, `Helm` version must be `v3.7` or later.

```bash
$ sudo apt update
$ sudo apt install linux-modules-extra-$(uname -r) linux-headers-$(uname -r)
$ modinfo nvme_tcp
$ sudo vi /etc/modules-load.d/storage-modules.conf
# ---add following values
# nvme-tcp
# ext4
# ---now exit
$ sudo reboot

$ lsmod | grep -E 'nvme_tcp|ext4'

$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
$ rm ./get_helm.sh
```

- Each worker node which will host an instance of an io-engine pod must have the following resources free and available for exclusive use by that pod: `Two` CPU cores, `1GiB` RAM, HugePage support (A minimum of `2GiB` of `2MiB`-sized pages).

```bash
$ grep HugePages /proc/meminfo
$ echo vm.nr_hugepages = 1024 | sudo tee -a /etc/sysctl.conf
$ sudo reboot
$ grep HugePages /proc/meminfo
```

- Ensure that the following ports are not in use on the node: `10124: Mayastor gRPC server will use this port`, `8420 / 4421: NVMf targets will use these ports`

```bash
$ sudo ss -tuln | grep -E ':10124|:8420|:4421'
```

- The firewall settings should not restrict connection to the node.

```bash
$ sudo ufw status
```

- Disks must be unpartitioned, unformatted, and used exclusively by the DiskPool. The minimum capacity of the disks should be `10 GB`.

```bash
$ lsblk -o NAME,SIZE,TYPE
```

- The minimum supported worker node count is `three` nodes.

- All worker nodes which will have IO engine pods running on them must be labeled with the OpenEBS storage type "Replicated PV Mayastor". This label will be used as a node selector by the IO engine Daemonset, which is deployed as a part of the Replicated PV Mayastor data plane components installation. To add this label to a node, execute:

```bash
kubectl label node <node_name> openebs.io/engine=mayastor
```

## Create DiskPool 

When a node allocates storage capacity for a replica of a Persistent Volume (PV) it does so from a DiskPool. Each node may create and manage zero, one, or more such pools. The ownership of a pool by a node is exclusive. A pool can manage only one block device, which constitutes the entire data persistence layer for that pool and thus defines its maximum storage capacity.

```yaml
apiVersion: "openebs.io/v1beta2"
kind: DiskPool
metadata:
  name: INSERT_POOL_NAME_HERE
  namespace: openebs
spec:
  node: INSERT_WORKERNODE_HOSTNAME_HERE
  disks: ["INSERT_DEVICE_URI_HERE"]
```

## Create StorageClass

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: mayastor-3
parameters:
  protocol: nvmf
  repl: "3"
provisioner: io.openebs.csi-mayastor
```

---

See [StorageClass Parameters](https://openebs.io/docs/user-guides/replicated-storage-user-guide/replicated-pv-mayastor/rs-configuration#storage-class-parameters).

See [Mayastor kubectl plugin](https://openebs.io/docs/user-guides/replicated-storage-user-guide/replicated-pv-mayastor/advanced-operations/kubectl-plugin).

See [Volume Snapshots](https://openebs.io/docs/user-guides/replicated-storage-user-guide/replicated-pv-mayastor/advanced-operations/volume-snapshots).

