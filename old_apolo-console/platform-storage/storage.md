# Storage

Apolo provides storage to host files that are required to run your jobs. All users on the platform can access the platform storage to manage data, code, and artifacts within their projects. Each storage is specific to its cluster, and you cannot reuse files between clusters. You can manage your storage either from the Apolo Console or the CLI.&#x20;

## The structure of platform storage

Platform storage is a tree-like filesystem, and each cluster and organization within cluster has its own storage space. Under the hood, high-performant NFS is usually used (this depends on the underlying cloud provider).

Home folders of all projects from a cluster are located in the root of this organization's storage space. So if an organization has three projects `projecta`, `projectb`, and `projectc`, the storage root will contain three folders `/projecta`, `/projectb`, and `/projectc.`

Here's the general syntax for addressing files and folders on the storage through commands in CLI:

* `storage://<cluster>/<org>/<project>/<path>` is used for accessing files and folders from a non-current cluster or organization. This path includes cluster, organization and project names.
* `storage:/<other-project>/<path>` is used for accessing files and folders of a specific project on the same cluster and organization.
* `storage:<some>/<path>` is used for accessing files and folders within your current context, which is defined by the cluster, organization and project. See `apolo config show`.

## Checking a cluster's storage usage

You can get information about used storage space on the current cluster by running `apolo storage df`:

```
$ apolo storage df
Disk usage for cluster default:
Total: 3.0T
Used:  2.0T
Free:  975.2G
```

## How do I manage files in the Apolo Console?&#x20;

You can use built-in Files app available in Apolo to manage files in storage. Intuitive drag\&drop, navigation and media files preview is available there.&#x20;

## How do I manage files in the CLI?&#x20;

You can manage your files and folders using the `apolo storage` command. The Apolo CLI lets you perform all operations with your files and folders that are available from the Apolo Console.

### Viewing folders and files&#x20;

To view the folders and files in an image, use the command:&#x20;

`apolo ls [OPTIONS] PATH`&#x20;

For more information, see [storage](https://app.gitbook.com/s/-MOkWy7dB5MDbkSII8iF/commands/storage "mention") section of Apolo CLI.

**Sample command:**

```
$ apolo storage ls storage:nero-assistant
ModelCode  apt.txt  config  data  modules  
notebooks  requirements.txt  results  setup.cfg
```

You could use `apolo storage tree` to view the contents in a tree format. This is might be handy when you have multiple sub-folders.

**Sample command:**

```
$ apolo storage tree storage:nero-assistant
'storage:nero-assistant'
+-- 'ModelCode'
|   +-- 'VoiceExperiment'
+-- 'apt.txt'
+-- 'config'
+-- 'data'
+-- 'modules'
+-- 'notebooks'
+-- 'requirements.txt'
+-- 'results'
+-- 'setup.cfg'

7 directories, 3 files
```

### Creating folders from the CLI&#x20;

You can create folders in the storage from the CLI by using the command:&#x20;

`apolo storage mkdir [OPTIONS] PATHS`

This works the same way as in the Apolo Console, but you can also create folders in any path without having to navigate to that path.

**Sample command:**

```
$ apolo storage mkdir -p storage:nero-assistant/ModelCode/VoiceExperiment
```

The folder is created and you can verify this by running the `apolo storage ls` command or in the Apolo Console.

### Moving files between the storage and my local file system&#x20;

You can use the `apolo cp SOURCE DESTINATION` command to move files between the storage and the host machine.&#x20;

To make sure that the folder's contents are also moved during the copying process, you need to use the recursive copy command: `apolo cp -r SOURCE DESTINATION`.

Remember the following key points before moving files or folders to and from storage:&#x20;

* The command assumes that the provided path is relative.&#x20;
* File and folder names are case-sensitive.

Examples:&#x20;

* To move files from local machine to storage, use the command `apolo cp parameters.txt storage:nero-assistant`\
  This command copies the file `parameters.txt` from the local host machine to the storage folder `nero-assistant`.

```
$ apolo cp parameters.txt storage:nero-assistant
Copy 'file:///C:/Projects/parameters.txt' => 'storage://default/.../parameters.txt' 'parameters.txt' 952B
```

* To move files from storage to the local machine, use the command `apolo cp storage:nero-assistant/TestResults.txt file:///Projects`\
  This command copies the `TestResults.txt` file from the storage folder `nero-assistant` to the local host machine.

```
$ apolo cp storage:nero-assistant/TestResults.txt file:///Projects 
Copy 'storage://default/.../TestResults.txt' => 'file:///C:/Projects/TestResults.txt' 'TestResults.txt' 1.5K
```

* To move a folder from the local host system to storage, use the command `apolo cp -r samplecode storage:nero-assistant`\
  This command copies the folder `samplecode` and all its files to the storage folder `nero-assistant`.

```
$ apolo cp -r samplecode storage:nero-assistant 
Copy 'file:///C:/Projects/samplecode' => 'storage://default/.../nero-assistant/samplecode' 'file:///C:/Projects/samplecode
```

* To move folders from storage to the local host, use the command `apolo cp -r storage:nero-assistant/testlogs file:///Projects`\
  This command copies the storage folder `testlogs` and all its files to the local host machine.

```
$ apolo cp -r storage:nero-assistant/testlogs file:///Projects 
Copy 'storage://default.../nero-assistant/testlogs' => 'file:///C:/Projects/testlogs' 'storage://default/clarytyllc/nero-assistant/testlogs' ...
```

### Removing files from storage&#x20;

You can remove files or folders from storage using the command `apolo rm [Files/Folder]`

To make sure that the folder's contents are also removed, you need to use the recursive deletion command: `apolo rm -r [Files/Folder]`

Examples:&#x20;

* To remove files from storage, use the command `apolo rm storage:nero-assistant/parameters.txt`
* To remove folders from storage, use the command `apolo rm -r storage:nero-assistant/samplecode`
* To remove files with a certain pattern, e.g., all `.log` files, use the command `apolo rm -r storage:nero-assistant/*.log`

### Moving files and folders within storage&#x20;

The process of moving files and folders is slightly different from the process of copying them. When you move files or folders, they are first copied to the destination and then removed from the source. To move files or folders, use the command `apolo storage mv`. Optionally, you can also rename a file after it 's moved to the destination directory.

Examples:&#x20;

* To move a file from a storage folder to another storage folder and rename the file, use the following command: `apolo mv storage:nero-assistant/code.txt storage:neuro-tutorial/samplecode.txt`\
  This command moves the file `code.txt` from the `nero-assistant` storage folder to the `neuro-tutorial` storage folder and renames this file to `samplecode.txt`.
* To move a storage folder and its contents to a new location, use the command: `apolo mv -T storage:nero-assistant/ModelCode storage:neuro-tutorial/SampleCode`\
  This command moves the folder `ModelCode` and all its contents from the `nero-assistant` storage folder to the `neuro-tutorial` storage folder and renames it to `SampleCode`.

## How do I use storage folders in jobs?&#x20;

You can use storage folders in jobs. To do this, you can mount a storage folder to the container running a job.&#x20;

### **Notes**:&#x20;

* It is highly unrecommended to pack/unpack archives (ZIP, TAR, etc) on a storage folder, since it performs a lot of IO operations. The process might take much longer compared to copying folder from storage to ephemeral job storage, processing it there and copying result back.

There are a few key moments you should keep in mind while working with storage:&#x20;

* When you run a job, you can modify files inside its ephemeral file system. However, when you terminate a job, all changes are lost (hence, ephemeral). To avoid this, you must mount storage volumes, disks or use platform buckets.
* Storage is a great way to deliver data into a job, fetch data from a job and share data between jobs.

### Mounting volumes

To use a volume when running a job, you must mount the data from the storage to the container. It's also possible to mount multiple volumes to the container.

The general syntax for mounting a volume is this:

```
--volume storage:path/on/storage:/mount/path:[ro|rw]
```

The `storage:path/on/storage` part specifies a relative path to the root folder of your project storage. You can also specify a full URI like this: `storage://cluster-name/org-name/other-project/path/on/storage`.&#x20;

`:/mount/path` specifies a path in the job container to which the volume should be mounted.

The last part specifies the mode in which to mount the volume - either `:ro` (read-only) or `:rw` (read-write). If this parameter is not specified, read-write mode is used by default.

{% hint style="info" %}
Read-write mount mode requires at least writer access level to the storage path.
{% endhint %}

{% hint style="info" %}
You can only mount volumes into volumes if the target root volumes is in read-write mode.
{% endhint %}

### Example

To mount a volume, use the `--volume` parameter in the `run` command. For example:&#x20;

```
$ apolo run --name job303 --volume storage:nero-assistant/ModelCode:/code:rw --preset cpu-small ubuntu -- cat code/train.py
```

This command mounts the datasets from the `ModelCode` folder to the container for use during job execution. The folder `/data` is used to access the data from within the job.

When in the container, you can update the data in the volume. For example, you can update a file in a container by running the following command from within the job:&#x20;

```
echo “New info” >/code/info.txt
```

## How do I share files with others?&#x20;

The easiest way to share storage paths with other users is to invite them into your project.

However, if a more coarse-grained control is needed, you can use the `apolo share` command.

**Sample command:**&#x20;

```
$ apolo share storage:nero-assistant/ModelCode alice manage
```

This shares the folder `storage:nero-assistant/ModelCode` with the user `alice`.

You can view the resources that are shared with you using the `apolo acl list` command:

```
$ apolo acl list
...
storage://default/johndoe/bone-age manage
storage://default/neuromation/midi-generator read
storage://default/neuromation/public read
```

## Storage operations with other clusters

All operations that were described earlier can be performed with storage from other clusters.&#x20;

For example, to copy a local `parameters.txt` file to the platform storage on the `<cluster-name>` cluster, run:

```
$ neuro cp parameters.txt storage://<cluster>/<org>/<project>/nero-assistant
```

Inter-cluster operations like archival / copying are available via `apolo-extras data` commands.
