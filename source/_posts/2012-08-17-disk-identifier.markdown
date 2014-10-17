---
layout: post
title: "Disk Identifier"
date: 2012-08-17 21:32
comments: true
categories: [work, disk identifier]
---

> http://www.linuxquestions.org/questions/linux-general-1/what-is-disk-identifier-740408/

This is a somewhat confusing area of computers because people don't use consistent terminology. So first I'm going to explain some terms.

A Disk Identifier (or Disk Signature) applies to an entire hard disk drive (not a single partition). A Disk Identifier/Disk Signature is a 4-byte (longword) number that is randomly generated when the Master Boot Record/Partition Table is first created and stored. The Disk Identifier is stored at byte offset 1B8 (hex) through 1BB (hex) in the MBR disk sector. Windows Vista uses the Disk Signature to locate boot devices so changing it can prevent Vista from booting. So far as I know Grub and Linux don't use the Disk Identifier.

A UUID (Universally Unique Identifier) or GUID (Globally Unique Identifier) is a 128-bit number. UUIDs are used to identify many different things including some filesystem partitions. Where the UUID is stored for a filesystem depends on the filesystem. Linux ext2/ext3 and Windows NTFS identify filesystems by UUID. UUIDs are generated randomly using either the current time or a random number generator. The UUID is generated and stored when the filesystem is formatted and then does not usually change.

When you copy a partition or disk as raw binary data (for example, with "dd") the Disk Identifier or UUID is also copied. That can result in two disks or two partitions with the same identifier. There are utilities to change the UUID to a new (random) number. There are also utilities to change the Disk Identifier in the Master Boot Record.

The advantage to a UUID is that no matter where you move a filesystem, an operating system can find that particular filesystem. For filesystems that have no UUID the Disk Identifier can at least be used to locate the disk drive.

Windows identifies all filesystems using UUIDs so UUIDs are kept in the registry if a filesystem does not have a UUID in the partition. Windows uses the Disk Signature and other information to match the registry entry for a partition and find the UUID.

Linux can use device names for partitions when UUIDs are not available.
