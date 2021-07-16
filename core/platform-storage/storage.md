# Storage

Neu.ro provides storage to host files that are required to run your jobs. All users on the platform can access the platform storage to manage data, code, and artifacts. Each storage is specific to its cluster, and you cannot reuse files between clusters. You can manage your storage either from the Web UI or the CLI. 

The following image shows an overview of data flows between your local machine, the storage, and the job file system: 

![](../../.gitbook/assets/storage-2.png)

## The structure of platform storage

Platform storage is a tree-like filesystem, and each cluster has its own storage space.

Home folders of all users from a cluster are located in the root of this cluster's storage space. So if a cluster has three users `usera`, `userb`, and `userc`, the storage root will contain three folders `/usera`, `/userb`, and `/userc` :

![](https://neuro-platform.myjetbrains.com/youtrack/api/files/8-21?sign=MTYyNjY1MjgwMDAwMHwxLTZ8OC0yMXx2eVd2c0FsSm1aMWsyU2VRZDZxaldxSndKbGVRelFNbk4w%0D%0AMURtTlh2dTMwDQo%0D%0A&updated=1625833857219)

If `usera` has some files and folders in their storage, it will look like this:

![](https://neuro-platform.myjetbrains.com/youtrack/api/files/8-22?sign=MTYyNjY1MjgwMDAwMHwxLTZ8OC0yMnxGeXRoaldfczVZcWg5WVA2SG9GMWR1TDFLSUx2bW1Db0Rz%0D%0ARGpocWNqZE40DQo%0D%0A&updated=1625833922333)

Here's the general syntax for addressing files and folders on the storage through commands in CLI:

* `storage://<some>/<path>` is used for accessing files and folders from a different cluster. This path includes both the cluster name and username.
* `storage:/<some>/<path>` is used for accessing files and folders of a specific user on the same cluster.
* `storage:<some>/<path>` is used for accessing your own files and folders.

## Checking a cluster's storage usage

You can get information about used storage space on the current cluster by running `neuro storage df`:

```text
$ neuro storage df
Disk usage for cluster neuro-compute:
Total: 3.0T
Used:  2.0T
Free:  975.2G
```

## How do I manage files in the web UI? 

You can use the intuitive FileBrowser tool available in Neu.ro to manage files in storage. The FileBrowser is based on the popular file browsing interface available on git.

To manage files: 

* Log in to Neu.ro. 
* Click **RUN A JOB** in the Storage widget.

![The Storage widget](../../.gitbook/assets/zobrazhennya%20%288%29%20%282%29.png)

* Select a preset from a drop-down list and click **RUN**.

![Preset selecton window](../../.gitbook/assets/zobrazhennya%20%286%29.png)

This opens the file browser for the cluster. Here, you can host and manage files.

![FileBrowser interface](../../.gitbook/assets/image%20%2824%29.png)

The file browser has the following areas: 

* Toolbar: Lets you search for files, download, and upload files. It also provides various other context-based options that are available depending on the file or folder you select. 
* Options: Lets you create a new folder or file. It also lets you manage the storage settings. 
* Folders and Files: Lists the folders and files you have access to. You can select files to view context-based options in the toolbar.

The rest of this tutorial will describe managing your files using the CLI. For more information about FileBrowser, see the [FileBrowser](https://github.com/filebrowser/filebrowser) documentation.

## How do I manage files in the CLI? 

You can manage your files and folders using the `neuro storage` command. The Neu.ro CLI lets you perform all operations with your files and folders that are available from the FileBrowser.

### Viewing folders and files 

To view the folders and files in an image, use the command: 

`neuro ls [OPTIONS] PATHS` 

For more information, see [List directory contents](https://neu-ro.gitbook.io/neu-ro-cli-reference/commands/storage#ls).

**Sample command:**

```text
$ neuro storage ls storage:nero-assistant
ModelCode  apt.txt  config  data  modules  
notebooks  requirements.txt  results  setup.cfg
```

You could use `neuro storage tree` to view the contents in a tree format. This is recommended when you have multiple sub-folders.

**Sample command:**

```text
$ neuro storage tree storage:nero-assistant
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

### Creating folders from the CLI 

You can create folders in the storage from the CLI by using the command: 

`neuro storage mkdir [OPTIONS] PATHS`

This works the same way as in the web UI, but you can also create folders in any path without having to navigate to that path.

**Sample command:**

```text
$ neuro storage mkdir -p storage:nero-assistant/ModelCode/VoiceExperiment
```

The folder is created and you can verify this by running the `neuro storage ls` command or in the Web UI.

### Moving files between the storage and my local file system 

You can use the `neuro cp SOURCE DESTINATION` command to move files between the storage and the host machine. 

To make sure that the folder's contents are also moved during the copying process, you need to use the recursive copy command: `neuro cp -r SOURCE DESTINATION`.

Remember the following key points before moving files or folders to and from storage: 

* The command assumes that the provided path is relative. 
* File and folder names are case-sensitive.

Examples: 

* To move files from local machine to storage, use the command `neuro cp parameters.txt storage:nero-assistant` This command copies the file `parameters.txt` from the local host machine to the storage folder `nero-assistant`.

```text
$ neuro cp parameters.txt storage:nero-assistant
Copy 'file:///C:/Projects/parameters.txt' => 'storage://neuro-compute/.../parameters.txt' 'parameters.txt' 952B
```

* To move files from storage to the local machine, use the command `neuro cp storage:nero-assistant/TestResults.txt file:///Projects` This command copies the `TestResults.txt` file from the storage folder `nero-assistant` to the local host machine.

```text
$ neuro cp storage:nero-assistant/TestResults.txt file:///Projects 
Copy 'storage://neuro-compute/.../TestResults.txt' => 'file:///C:/Projects/TestResults.txt' 'TestResults.txt' 1.5K
```

* To move a folder from the local host system to storage, use the command `neuro cp -r samplecode storage:nero-assistant` This command copies the folder `samplecode` and all its files to the storage folder `nero-assistant`.

```text
$ neuro cp -r samplecode storage:nero-assistant 
Copy 'file:///C:/Projects/samplecode' => 'storage://neuro-compute/.../nero-assistant/samplecode' 'file:///C:/Projects/samplecode
```

* To move folders from storage to the local host, use the command `neuro cp -r storage:nero-assistant/testlogs file:///Projects` This command copies the storage folder `testlogs` and all its files to the local host machine.

```text
$ neuro cp -r storage:nero-assistant/testlogs file:///Projects 
Copy 'storage://neuro-compute/.../nero-assistant/testlogs' => 'file:///C:/Projects/testlogs' 'storage://neuro-compute/clarytyllc/nero-assistant/testlogs' ...
```

### Removing files from storage 

You can remove files or folders from storage using the command `neuro rm [Files/Folder]`

To make sure that the folder's contents are also removed, you need to use the recursive deletion command: `neuro rm -r [Files/Folder]`

Examples: 

* To remove files from storage, use the command `neuro rm storage:nero-assistant/parameters.txt`
* To remove folders from storage, use the command `neuro rm -r storage:nero-assistant/samplecode`
* To remove files with a certain pattern, e.g., all `.log` files, use the command `neuro rm -r storage:nero-assistant/*.log`

### Moving files and folders within storage 

The process of moving files and folders is slightly different from the process of copying them. When you move files or folders, they are first copied to the destination and then removed from the source. To move files or folders, use the command `neuro storage mv`. Optionally, you can also rename a file after it 's moved to the destination directory.

Examples: 

* To move a file from a storage folder to another storage folder and rename the file, use the following command: `neuro mv storage:nero-assistant\code.txt storage:neuro-tutorial\samplecode.txt` This command moves the file `code.txt` from the `nero-assistant` storage folder to the `neuro-tutorial` storage folder and renames this file to `samplecode.txt`.
* To move a storage folder and its contents to a new location, use the command: `neuro mv -T storage:nero-assistant/ModelCode storage:neuro-tutorial/SampleCode` This command copies the folder `ModelCode` and all its contents from the `nero-assistant` storage folder to the `neuro-tutorial` storage folder and renames it to `SampleCode`.

## How do I use storage folders in jobs? 

You can use storage folders in jobs. To do this, you can mount a storage folder to the container running a job. 

### **Notes**: 

* Jobs can get slower while using storage folders as compared to using a job container file system. Our experiments show that training CV models takes 10-20% longer when the dataset is read from a mounted storage folder. If such a difference is critical for you, we recommend downloading the datasets for jobs into a non-mounted directory before running the jobs.
* It is highly unrecommended to unpack datasets \(ZIP, TAR, etc\) into a storage folder, as it takes a lot of time.

There are a few key moments you should keep in mind while working with storage: 

* When you run a job, you can modify files inside its local file system. However, when you terminate a job, all changes are lost. To avoid this, you must use volumes. 
* A volume’s data persists even after the job container is deleted. 
* Volumes are a great way to push data to a container, pull data from a container, and share data between containers.

### Mounting volumes

To use a volume when running a job, you must mount the data from the storage to the container. It's also possible to mount multiple volumes to the container.

The general syntax for mounting a volume is this:

```text
--volume storage:/path/on/storage:/mount/path:[ro|rw]
```

The `storage:/path/on/storage` part specifies a path relative to the root folder on the platform storage. You can also specify a full URI including a cluster like this: `storage://cluster-name/path/on/storage`. 

`:/mount/path` specifies a path in the job container to which the volume should be mounted.

The last part specifies the mode in which to mount the volume - either `:ro` \(read-only\) or `:rw` \(read-write\). If this parameter is not specified, read-write mode is used by default.

{% hint style="info" %}
You can only mount volumes into volumes if the target root volumes is in read-write mode.
{% endhint %}

### Example

To mount a volume, use the `--volume` parameter in the `run` command. For example: 

```text
$ neuro run --name job303 --volume storage:nero-assistant/ModelCode:/code:rw --preset cpu-small ubuntu cat code/train.py
```

This command mounts the datasets from the `ModelCode` folder to the container for use during job execution. The folder `/data` is used to access the data from within the job.

When in the container, you can update the data in the volume. For example, you can update a file in a container by running the following command from within the job: 

```text
echo “New info” >/code/info.txt
```

## How do I share files with others? 

You can share files and folders on your storage with others, similar to how you would share images. You can specify the access level the users will get for files: read, write or manage.

To share a file or folder, use the `neuro share` command.

**Sample command:** 

```text
$ neuro share storage:nero-assistant/ModelCode alice manage
```

This shares the folder `storage:nero-assistant/ModelCode` with the user `alice`.

You can view the resources that are shared with you using the `neuro acl list` command:

```text
$ neuro acl list
...
storage://neuro-compute/johndoe/bone-age manage
storage://neuro-compute/neuromation/midi-generator read
storage://neuro-compute/neuromation/public read
```

## Storage operations with other clusters

All operations that were described earlier can be performed with storage from other clusters. 

For example, to copy a local `parameters.txt` file to the platform storage on the `<cluster-name>` cluster, run:

```text
$ neuro cp parameters.txt storage:/<cluster-name>/<user-name>/nero-assistant
```

