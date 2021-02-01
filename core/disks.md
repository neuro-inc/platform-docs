# Disks

As opposed to standard platform storage, disks provide a faster, short-lived, non-partitioned space for jobs. This makes disks perfect for manipulating large amounts of data. 

For example, if there's a large dataset that you need to process, it's better to create a disk, upload all of the required data from the storage to this disk, and only then perform all the necessary operations. In some cases, this can save you about 10%-20% of time depending on the amount of data and the operations you perform with it. After the data is processed, you can download it back to the storage for further use.

### Creating disks

The `neuro disk create` command is used to create disks. When creating a disk, you need to specify its storage capacity by providing a corresponding value as a parameter. For example:

```text
> neuro disk create 40G
```

This will create with a storage capacity of `40 * 2^30` bytes. Note that some servers can have a predefined granularity \(for example, 1GB\), so the values you provide will be rounded up in such cases.

### Disk lifespan

Disks have a limited lifetime. This lifespan is calculated from the point in time the disk was last used. The default lifespan of a disk is 1 day. You can also specify a custom lifespan by providing the corresponding value in a `--life-span` parameter when creating a disk: 

```text
> neuro disk create --life-span 2d4h 40G
```

This will set the disk's lifespan to 2 days and 4 hours after last usage.

You can find more information about using the `neuro disk create` command [here](https://neu-ro.gitbook.io/neu-ro-cli-reference/commands/disk).

### Mounting disks

You can use disks when running jobs. The process is very similar to [mounting storage](storage.md#how-do-i-use-storage-folders-in-jobs). 

To mount a disk to a job's container, use the `--volume` parameter in the `run` command:

```text
neuro run --name job303 --volume disk:disk-id:/mnt/disk --preset cpu-small ubuntu cat code/train.py
```

