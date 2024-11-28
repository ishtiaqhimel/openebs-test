## LVM (Logical Volume Manager)

A layer of abstraction between physical storage devices and the storage presented to the operating system, allowing for flexible and dynamic storage management.

## Key Concepts in LVM

- **Physical Volumes (PV):** actual storage devices or partitions (e.g., `/dev/sda1`, `/dev/sdb`) used as building blocks for LVM.
- **Volume Groups (VG):** collection of physical volumes combined into a single storage pool.
- **Logical Volumes (LV):** virtual storage units created from volume groups. These are similar to partitions but can be resized or moved.

## Commands
- initialize PV: `pvcreate /dev/sda1 /dev/sdb1`
- create a VG: `vgcreate my_vg /dev/sda1 /dev/sdb1`
- Show LV(s): `lvs`
- Show VG(s): `vgs`
- Show PV(s): `pvs`
