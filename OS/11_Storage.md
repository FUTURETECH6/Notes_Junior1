Review
• Disk: platter, cylinder, sector, track
• Time: positioning time = seek time + rotational latency
• SSD: read/write in page, but cannot overwrite in place
• Flash translation layer, gc, over-provisioning area
• Disk structure, Disk attachment: host- ,network-, storage area network
• Disk scheduling: CFS, SSTF, SCAN, C-SCAN, C-LOOK
• Disk management: physical formatting, partition disk, logical formatting
• RAID: redundant array of inexpensive disk
• RAID 0: split, RAID 1: mirror, RAID4/RAID 5/RAID 6: block with parity
• ZFS

## Disk Scheduling

合理安排访问顺序，减少用在寻道上的时间

* FCFS: First came, first served
* SSTF: Shortest seek time first
    * may cause starvation: 远端的可能一直无法被调度到
    * 响应时间波动大
* SCAN: Elevator Alg，一次只向一个方向移动，到无地址可读再掉头
* C-SCAN: 1, 2, ..., end, 1, 2, ...
* **LOOK/C-LOOK**
    * 常用

# RAID