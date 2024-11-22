# OpenEBS Test

## OpenEBS

OpenEBS turns any storage available to Kubernetes worker nodes into Local or Replicated Kubernetes Persistent Volumes.

## Local Volumes

Local Volumes are accessible only from a single node in the cluster. Pods using local volume have to be scheduled on the node where volume is provisioned.

See [Local Storage](https://openebs.io/docs/concepts/data-engines/localstorage) to learn more.

## Replicated Volumes

Replicated Volumes, as the name suggests, are those that have their data synchronously replicated to multiple nodes. Volumes can sustain node failures. The replication also can be set up across availability zones helping applications move across availability zones.

See [Replicated Storage](https://openebs.io/docs/concepts/data-engines/replicated-storage) to learn more.

## Features

- **Containerized Storage for Containers :** OpenEBS is an example of Container Native Storage (CNS). Volumes provisioned through OpenEBS are always containerized. Each volume has a dedicated storage controller that increases the agility and granularity of persistent storage operations of the stateful applications. 
- **Synchronous Replication :** Synchronous Replication is an optional and popular feature of OpenEBS. When used with the Replicated Storage, OpenEBS can synchronously replicate the data volumes for high availability. The replication happens across Kubernetes zones resulting in high availability for cross AZ setups. This feature is especially useful to build highly available stateful applications using local disks on cloud providers services such as GKE, EKS and AKS.
- **Snapshots and Clones :** Copy-on-write snapshots are another optional and popular feature of OpenEBS. When using the Replicated Storage, snapshots are created instantaneously. The incremental snapshot capability enhances data migration and portability across Kubernetes clusters and different cloud providers or data centers. Operations on snapshots and clones are performed in completely Kubernetes native method using the standard kubectl commands. Common use cases include efficient replication for backups and the use of clones for troubleshooting or development against a read-only copy of data.
- **Backup and Restore :** The backup and restore of OpenEBS volumes works with Kubernetes backup and restore solutions such as Velero via open source OpenEBS Velero-plugins. Data backup to object storage targets such as AWS S3, GCP Object Storage or MinIO are frequently deployed using the OpenEBS incremental snapshot capability. 
- **Prometheus Metrics for Workload Tuning :** OpenEBS volumes are instrumented for granular data metrics such as volume IOPS, throughput, latency and data patterns. As OpenEBS follows the CNS pattern, stateful applications can be tuned for better performance by observing the traffic data patterns on Prometheus and modifying the storage policy parameters without worrying about neighboring workloads that are using OpenEBS thereby minimizing the incidence of "noisy neighbor" issues.

## Data Engines

The data engines are at the core of OpenEBS and are responsible for performing the read and write operations to the underlying persistent storage on behalf of the Stateful workloads they serve.

See [Overview of OpenEBS Data Engines](https://openebs.io/docs/concepts/data-engines/dataengines) to learn more.

## Install

Need to make sure that Kubernetes nodes meet the required prerequisites for the data engine(s) want to use.

```bash
$ helm repo add openebs https://openebs.github.io/openebs
$ helm repo update
$ helm install openebs --namespace openebs openebs/openebs --create-namespace # install both local and replicated data engines
```

## Uninstall 

```bash
$ helm uninstall openebs -n openebs
```

## Resources

- [LVM vs ZFS: A Deep Dive Comparison for System Administrators](https://go.lightnode.com/tech/lvm-vs-zfs)
- [Hugepages](https://wiki.debian.org/Hugepages)
- [Kubernetes Storage Options Can Be Overwhelming — Pick the Right One](https://www.cncf.io/blog/2021/04/05/kubernetes-storage-options-can-be-overwhelming-pick-the-right-one/)