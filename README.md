# linux-raid5-disk-recovery
RAID-5 storage implementation and disk failure recovery using mdadm on Linux
# RAID-5 Configuration & Disk Replacement on Linux using mdadm

## Overview
This project demonstrates the implementation of RAID-5 on a Linux system, along with a real-world disk failure simulation and recovery process.

The initial configuration consisted of 3 disks (/dev/sda, /dev/sdb, /dev/sdc).
After RAID creation, /dev/sda was simulated as faulty and replaced with /dev/sdd.

---

## RAID-5 Creation

lsblk
mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sda /dev/sdb /dev/sdc
cat /proc/mdstat

---

## Create filesystem & mount

mkfs.ext4 /dev/md0
mkdir /raid5
mount /dev/md0 /raid5

---

## Persistent mount using /etc/fstab

UUID from:
blkid /dev/md0

Added to /etc/fstab:
UUID=844aae14-9c4b-43cb-99e5-56fd0978b46f  /raid5  ext4  defaults  0  0

systemctl daemon-reload

---

## Disk Failure Simulation

mdadm --fail /dev/md0 /dev/sda
mdadm --remove /dev/md0 /dev/sda

---

## Add Replacement Disk

mdadm --add /dev/md0 /dev/sdd

---

## Verify RAID Status

cat /proc/mdstat
mdadm --detail /dev/md0

---

## Project Outcome

✔ RAID-5 successfully created  
✔ Disk failure successfully simulated  
✔ Rebuild completed after adding new disk  
✔ System remained available during disk failure  
✔ Demonstrated real-world sysadmin fault-tolerance handling

---

## Tech Stack

- Linux (RHEL/CentOS/Ubuntu compatible)
- mdadm RAID utility
- ext4 filesystem
- UUID & fstab mount management

---

## Summary

This project demonstrates real hands-on Linux system administration ability:
- Creating RAID,
- Managing storage,
- Handling failure events,
- Performing recovery
with no data loss.

