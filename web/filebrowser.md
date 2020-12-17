---
description: >-
  This page is slightly outdated. Our technical writers are updating it right
  now.
---

# FileBrowser

## Introduction

FileBrowser is an intuitive web interface used to manage files and folders. The FileBrowser is based on the popular file browsing interface available on [git](https://github.com/filebrowser/filebrowser). FileBrowser lets you:

* [Create files and folders](filebrowser.md#creating-files-and-folders)
* [Download files and folders](filebrowser.md#downloading-files-and-folders)

To start the FileBrowser, click on the **Start FileBrowser** button on the `Neu.ro` homepage.

![](../.gitbook/assets/stor_start_filebrowser.jpg)

FileBrowser instances are jobs that run using the cpu-small preset, and are listed in the Jobs window of `Neu.ro` as a job with the tag `kind:web-widget target:filebrowser`. The platform storage is mounted at /var/storage which is also the root directory to let all files persist.

![](../.gitbook/assets/stor_browser.jpg)

The FileBrowser UI has the following areas:

* **Toolbar:** Displays the list of actions you can perform. The toolbar shows additional options when you select a file or folder, such as copy, rename, share, move, delete.
* **Options:** Displays additional options you have, such as My Files \(home\) link, New Folder, New File, and Settings.
* **Folders and Files:** Displays the contents of the current folder.

Optionally, you can also use Shell from within FileBrowser by click on the toggle shell ![](../.gitbook/assets/FB_Toggle.jpg) button, and run shell commands in the shell area.

![](../.gitbook/assets/FB_Shell.jpg)

After you are done working with FileBrowser, ensure that you kill the job associated with it.

![](../.gitbook/assets/FB_Job.JPG)

FileBrowser runs as a job with the cpu-small preset. All FileBrowser jobs are automatically killed after 24 hours.

## Creating Files and Folders

You can create files and folders from the FileBrowser. While creating files, FileBrowser has a basic text editor that lets you add content to the file. However, it is recommended that you upload instead of creating them.

**To create a folder:**

1. From the Options area of the FileBrowser, click on the **New Folder** button. ![](../.gitbook/assets/FB_NewFolder.jpg)
2. Enter a name for the new folder. ![](../.gitbook/assets/FB_NewDirectory.jpg)

**To create a file:**

1. From the Options area of the FileBrowser, click on the **New File** button. ![](../.gitbook/assets/FB_NewFile.jpg)
2. Enter a name for the file. ![](../.gitbook/assets/FB_NewFileName.jpg) The file is created and you are taken to the file editor where you can enter the contents of the file.
3. Click **Save** after entering the content of the files. ![](../.gitbook/assets/FB_NewFile_Save.JPG)

**To upload files:**

1. From the Toolbar, click on the **upload** button. ![](../.gitbook/assets/FB_UploadButton.jpg)
2. Select the file that you want to upload. The file is uploaded. You can edit a file by clicking on the **Edit** button. ![](../.gitbook/assets/FB_UpFile.JPG)

## Downloading Files and Folders

You can download files, folders, or the entire storage on FileBrowser using the download button.

**To download files and folders:**

1. Browse to the folder or the file directory and click on the **Download** button. ![](../.gitbook/assets/FB_Download.jpg) If you do not select any file, then the current folder is downloaded, else the selected files are downloaded.
2. If you are downloading a folder, then select the format in which you want to download the folder. ![](../.gitbook/assets/FB_DownFormat.jpg)

The current folder will be downloaded in the selected format.

## Managing Files and Folders from Mobile Devices

Neu.ro provides seamless support to mobile devices. You can access neu.ro and the FileBrowser from any mobile device to view, upload, download, and manage files on your mobile device. For example, you may want to upload sample images for the [**Object Detection**](https://docs.neu.ro/cookbook/object-detection) ML recipe from your mobile device.

**To download files to your mobile device:**

1. Login into `neu.ro`, and start a FileBrowser.

![](../.gitbook/assets/FBM_NeuroHome.jpg) ![](../.gitbook/assets/FBM_FileBrowser%20%281%29.jpg)

1. Navigate to the folder to download files from, and select the file or folder that you want to download.

![](../.gitbook/assets/FBM_Folder.jpg) ![](../.gitbook/assets/FBM_Down_Select_1.jpg)

1. Select Download from the options menu.

![](../.gitbook/assets/FBM_Down_Select.jpg) ![](../.gitbook/assets/FBM_DownloadDone%20%281%29.jpg)

The file is downloaded to your mobile device.

![](../.gitbook/assets/FBM_DownloadDone.jpg)

**To upload files from your mobile device:**

1. Login into neu.ro, and start a FileBrowser.

![](../.gitbook/assets/FBM_NeuroHome%20%281%29.jpg) ![](../.gitbook/assets/FBM_FileBrowser.jpg)

1. Navigate to the folder you want to upload files to from your mobile, and select Upload from the Options menu.

![](../.gitbook/assets/FBM_Up_Folder.jpg) ![](../.gitbook/assets/FBM_UploadButton.jpg)

1. Navigate to the folder from where you want to upload the file and select the file.

![](../.gitbook/assets/FBM_UploadFileFolder.jpg)

The file is uploaded to your mobile device.

![](../.gitbook/assets/FBM_FileUploaded.jpg)

