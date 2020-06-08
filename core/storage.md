# Storage

Neu.ro provides storage to host files that are required to run your jobs. All users on a platform can access the platform storage to manage data, code, and artifacts. Each storage is specific to that cluster, and you cannot reuse files between clusters. You can manage your storage either from the Web UI or the CLI. 

The following image shows an overview of data flows between your local machine, the storage, and the job file system: 

![](../.gitbook/assets/storage-2.png)

### How do I manage files in the web UI? 

You can use the intuitive FileBrowser tool available in Neu.ro to manage files in storage. The FileBrowser is based on the popular file browsing interface available on git.

To manage files: 

* Login into Neu.ro. 
* Click on Start FileBrowser.

![Start FileBrowser](../.gitbook/assets/stor_start_filebrowser.jpg)

This opens the file browser for the cluster, where you can host and manage files.

![FileBrowser interface](../.gitbook/assets/stor_browser.jpg)

The file browser has the following areas: 

* Toolbar: Lets you search for files, download, and upload files. It also provides various other context-based options that are available depending on the file or folder you select. 
* Options: Lets you create a new folder or file. It also lets you manage the storage settings. 
* Folders and Files: Lists the folders and files you have access to. You can select files to view context-based options in the toolbar.

The rest of this tutorial will describe managing your files using the CLI. For more information about FileBrowser, see the [FileBrowser](https://github.com/filebrowser/filebrowser) documentation.

### How do I manage files in the CLI? 

You can manage your files and folders using the neuro storage command. The neu.ro CLI lets you perform all operations with your files and folders that are available from the FileBrowser.

#### Viewing folders and files 

To view the folders and files in an image, use the command: 

`neuro ls [OPTIONS] PATHS` 

For more information, see [List directory contents](https://docs.neu.ro/references/cli-reference/storage#ls).

**Sample command:**

```text
> neuro storage ls storage:nero-assistant
ModelCode  apt.txt  config  data  modules  
notebooks  requirements.txt  results  setup.cfg
```

You could use `neuro storage tree` to view the contents in a tree format. This is recommended when you have multiple sub-folders.

**Sample command:**

```text
> neuro storage tree storage:nero-assistant
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

#### Creating folders from the CLI 

You can create folders in the storage from the CLI by using the command: 

`neuro storage mkdir [OPTIONS] PATHS`

This works the same way as the web UI, but, in CLI, you can create folders in any path without having to navigate to that path.

**Sample command:**

```text
neuro storage mkdir -p storage:nero-assistant/ModelCode/VoiceExperiment
```

The folder is created and you can verify by running the `neuro storage ls` command or in the Web UI.

#### Moving files between the storage and my local file system 

You can use the `neuro cp SOURCE DESTINATION` command to move files between the storage and the host machine. 

For moving folders, you must use the recursive copy command so that the folder contents are also moved: `neuro cp -r SOURCE DESTINATION`.

Remember the following key points before moving files or folders to and from storage: 

* The command assumes that the path provided is relative. 
* File and folder names are case-sensitive.

Examples: 

* To move files from local machine to storage, use the command `neuro cp parameters.txt storage:nero-assistant` This command copies the file `parameters.txt` from the local host machine to the storage folder `nero-assistant`.

```text
> neuro cp parameters.txt storage:nero-assistant
Copy 'file:///C:/Projects/parameters.txt' => 'storage://neuro-public/.../parameters.txt' 'parameters.txt' 952B
```

* To move files from storage to the local machine, use the command `neuro cp storage:nero-assistant/TestResults.txt file:///Projects` This command copies the `TestResults.txt` file from the storage folder `nero-assistant` to the local host machine.

```text
> neuro cp storage:nero-assistant/TestResults.txt file:///Projects 
Copy 'storage://neuro-public/.../TestResults.txt' => 'file:///C:/Projects/TestResults.txt' 'TestResults.txt' 1.5K
```

* To move folder from local host system to storage, use the command `neuro cp -r samplecode storage:nero-assistant` This command copies the folder `samplecode` and all its files to the storage folder `nero-assistant`.

```text
> neuro cp -r samplecode storage:nero-assistant 
Copy 'file:///C:/Projects/samplecode' => 'storage://neuro-public/.../nero-assistant/samplecode' 'file:///C:/Projects/samplecode
```

* To move folders from storage to the local host, use the command `neuro cp -r storage:nero-assistant/testlogs file:///Projects` This command copies the storage folder `testlogs` and all its files to the local host machine.

```text
> neuro cp -r storage:nero-assistant/testlogs file:///Projects 
Copy 'storage://neuro-public/.../nero-assistant/testlogs' => 'file:///C:/Projects/testlogs' 'storage://neuro-public/clarytyllc/nero-assistant/testlogs' ...
```

#### Removing files from storage 

You can remove files or folders from storage using the command `neuro rm [Files/Folder]`

For removing folders, you must use the recursive remove command so that the folder contents are also removed: `neuro rm -r [Files/Folder]`

Examples: 

* To remove files from a storage, use the command `neuro rm storage:nero-assistant/parameters.txt`
* To remove folders from storage, use the command `neuro rm -r storage:nero-assistant/samplecode`
* To remove files with a certain pattern, such as `.log`, use the command `neuro rm -r storage:nero-assistant/*.log`

#### Moving files and folders within storage 

The moving of files or folders is different from copying files or folders. When you move files or folders, the files and folders are copied to the destination and then removed from the source. To move files or folders, use the command: `neuro storage mv`. Optionally, you can also rename a file after it is moved to the destination directory.

Examples: 

* To move a file from storage folder to another storage folder and rename the file, use the command `neuro mv storage:nero-assistant\code.txt storage:neuro-tutorial\samplecode.txt` This command moves the file `code.txt` from the `nero-assistant` storage folder to the `neuro-tutorial` storage folder and renames the file to `samplecode.txt`.
* To move a folder and its contents from storage folder to another, use the command: `neuro mv -T storage:nero-assistant/ModelCode storage:neuro-tutorial/SampleCode` This command copies the folder `ModelCode` and all its contents from the `nero-assistant` storage folder to the `neuro-tutorial` storage folder and renames the folder to `SampleCode`.

### How do I use storage folders in jobs? 

You can use storage folders in jobs. You can mount a storage directory to the container running the job in order to use the storage folder. 

**Notes**: 

* Jobs can be slower while using storage folders when compared to using a job container file system. Our experiments show that training CV models takes 10-20% longer when the dataset is read from a mounted storage folder. If such a difference is critical for you, we recommend downloading the datasets for jobs into a non-mounted directory before running the jobs.
* It is highly unrecommended to unpack datasets \(ZIP, TAR, etc\) in a storage folder, as it takes a lot of time.

Before working with storage, you must understand the following: 

* When you run a job, you can modify files inside its local file system. However, when you terminate a job, all changes are lost. To avoid this, you must use volumes. 
* A volume’s data persists even after the job container is deleted. 
* Volumes are a great way to push data to a container, pull data from a container, and share data between containers.

To use a volume when running a job, you must mount the data from the storage to the container. Optionally, you can mount multiple volumes into the container.

To mount a container, use the `--mount` parameter in the run command. For example: 

```text
neuro run --name job303 --volume storage:nero-assistant/ModelCode:/code:rw --preset cpu-small ubuntu cat /code/train.py
```

This command mounts the datasets from the `ModelCode` folder to the container, for use during the job execution. The folder `/data` is used to access the data from within the job.

When in the container, you can update the data in the volume. For example, you can use the following command to update a file from within a container: 

```text
ubuntu:/ # echo “New info” >/code/info.txt
```

### How do I share files with others? 

You can share files and folders on your storage with others, similar to sharing images. You can specify the access the users can have on files: read, write or manage.

To share a file or folder, use the `neuro share` command.

Sample command: `neuro share storage:nero-assistant/ModelCode alice manage`

This shares the folder `storage:nero-assistant/ModelCode` with the user `alice`.

You can view the resources that are shared with you using the `neuro acl list` command:

```text
> neuro acl list
...
storage://neuro-public/johndoe/bone-age manage
storage://neuro-public/neuromation/midi-generator read
storage://neuro-public/neuromation/public read
```

