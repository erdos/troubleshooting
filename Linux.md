# File System

## File system error (LVM)

System won't boot after power failure: Likely a file system error, the following worked for me with LVM partitions:

1. Boot a live system from an USB drive.
2. `vgchange --ignoreblockingfailure -ay`
3. `lvscan --ignoreblockingfailure`
4. `fsck -y /dev/GROUP/PARTITION`
