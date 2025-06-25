
---

## 📁 `Lab-2_LVM_Setup/README.md`

```markdown
# 💾 Lab 2: Logical Volume Management (LVM) Setup and Extension

## 🎯 Objective

Learn how to dynamically manage disk storage by:
- Partitioning a new disk
- Creating a volume group and logical volume
- Extending storage without downtime using LVM

---

## 🛠️ Environment
- OS: CentOS Stream 9
- Tools: `fdisk`, `pvcreate`, `vgcreate`, `lvcreate`, `resize2fs`

---

## 📋 Implementation Steps

### 1️⃣ Add a New 6 GB Virtual Disk to the VM

Disk appears as `/dev/sdb`

### 2️⃣ Partition the Disk
```bash
sudo fdisk /dev/sdb
3️⃣ Initialize and Set Up LVM

pvcreate /dev/sdb1
vgcreate my_vg /dev/sdb1
lvcreate -L 1.5G -n my_lv my_vg
mkfs.ext4 /dev/my_vg/my_lv
mkdir /mnt/lvm
mount /dev/my_vg/my_lv /mnt/lv
4️⃣ Extend the Volume Group and Logical Volume

pvcreate /dev/sdb2
vgextend my_vg /dev/sdb2
lvextend -L +2G /dev/my_vg/my_lv
resize2fs /dev/my_vg/my_lv
