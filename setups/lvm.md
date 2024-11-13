## Local PV LVM

OpenEBS Local Persistent Volumes (PV) backed by LVM Storage.

## Prerequisites

- All the nodes must have `lvm2` utils installed and the `dm-snapshot` kernel module loaded.

```bash
$ sudo apt install lvm2
$ sudo tee /etc/modules-load.d/dm-snapshot.conf <<< "dm-snapshot"
$ sudo reboot

$ lsmod | grep dm_snapshot
```


- Find the disk that you want to use for LVM. Create the Volume group on all the nodes, which will be used by the LVM Driver for provisioning the volumes.

```bash
$ lsblk # to list all block devices
$ sudo vgcreate lvmvg /dev/vdb # here lvmvg is the volume group name to be created...replace /dev/vdb according to your preference 
```

## Create StorageClass

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: openebs-lvmpv
parameters:
  storage: "lvm"
  volgroup: "lvmvg"
provisioner: local.csi.openebs.io
```

## Create VolumeSnapshotClass

```yaml
kind: VolumeSnapshotClass
apiVersion: snapshot.storage.k8s.io/v1
metadata:
  name: lvmpv-snapclass
  annotations:
    snapshot.storage.kubernetes.io/is-default-class: "true"
driver: local.csi.openebs.io
deletionPolicy: Delete
```

---

See [StorageClass Parameters Conformance Matrix](https://openebs.io/docs/user-guides/local-storage-user-guide/local-pv-lvm/lvm-configuration#storageclass-parameters-conformance-matrix).

See [PersistentVolumeClaim Conformance Matrix](https://openebs.io/docs/user-guides/local-storage-user-guide/local-pv-lvm/lvm-deployment#persistentvolumeclaim-conformance-matrix).

See [Snapshot](https://openebs.io/docs/user-guides/local-storage-user-guide/local-pv-lvm/advanced-operations/lvm-snapshot).

