## Test on Loopback Device

If you do not have disk, then you can test on the loopback device which is backed by a sparse file.

### LVM

```bash
$ truncate -s 200G /tmp/disk.img # creates an empty file of a specified size
$ sudo losetup -f /tmp/disk.img --show # assigns the `disk.img` file to an available loop device, allowing it to function as if it were a block device (like `/dev/sda` or `/dev/sdb`). `--show` prints the name of the loop device created (e.g., `/dev/loop0`).
$ sudo pvcreate /dev/loop0
$ sudo vgcreate lvmvg /dev/loop0 # here lvmvg is the volume group name to be created
```

### ZFS

```bash
$ truncate -s 200G /tmp/disk.img
$ zpool create zfspv-pool `losetup -f /tmp/disk.img --show`
$ zpool status
```