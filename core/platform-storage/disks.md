# Disks

As opposed to standard platform storage, disks provide a faster, short-lived, non-partitioned space for jobs. This makes disks perfect for manipulating large amounts of data. 

For example, if there's a large dataset that you need to process, it's better to create a disk, upload all of the required data from the storage to this disk, and only then perform all the necessary operations. In some cases, this can save you about 10%-20% of time depending on the amount of data and the operations you perform with it. After the data is processed, you can download it back to the storage for further use.

### Creating disks

The `neuro disk create` command is used to create disks. When creating a disk, you need to specify its storage capacity by providing a corresponding value as a parameter. You can also specify the disk's name, which provides a more convenient way of accessing it in commands later. For example:

```text
> neuro disk create --name test-disk 40G
```

This will create with a storage capacity of `40 * 2^30` bytes and a name of `test-disk`. Note that some servers can have a predefined granularity \(for example, 1GB\), so the values you provide will be rounded up in such cases.

### Disk lifespan

Disks have a limited lifetime. This lifespan is calculated from the point in time the disk was last used. The default lifespan of a disk is 1 day. You can also specify a custom lifespan by providing the corresponding value in a `--timeout-unused` parameter when creating a disk: 

```text
> neuro disk create --name test-disk --timeout-unused 2d4h 40G
  Id              disk-6d1c44dc-d759-4bc8-8eb2-feafea463898
  Storage         2.0G
  Used
  Uri             disk://neuro-compute/jane-doe/disk-6d1c44dc-d759-4bc8-8eb2-feafea463898
  Name            test-disk
  Status          Pending
  Created at      a second from now
  Last used
  Timeout unused  2d 4h
```

This will set the disk's lifespan to 2 days and 4 hours after last usage.

You can find more information about using the `neuro disk create` command [here](https://neu-ro.gitbook.io/neu-ro-cli-reference/commands/disk).

### Mounting disks

You can use disks when running jobs. The process is very similar to [mounting storage](storage.md#how-do-i-use-storage-folders-in-jobs). 

To mount a disk to a job's container, use the `--volume` parameter in the `run` command. The syntax for this is `--volume disk:<disk-id-or-name>:/mnt/disk`. You need to replace `<disk-id-or-name>` with the disk's actual ID or name. You also need to replace `/mnt/disk/` with the actual path to the disk on the platform storage. The disk's ID, name, and path are shown when the disk is created. 

Here's an example:

```text
> C:\Users\Jane>neuro run --name job303 --volume disk:test-disk:/neuro-compute/jane-doe/disk-eff8095a-7f26-404d-92de-5837cc8ed444 --preset cpu-small ubuntu cat code/train.py
```

