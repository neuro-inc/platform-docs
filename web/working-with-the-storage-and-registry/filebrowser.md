# Filebrowser

## Introduction

Filebrowser is an intuitive web interface used to manage files and folders. It's based on the popular file browsing interface available on [git](https://github.com/filebrowser/filebrowser). Filebrowser lets you:

* [Create files and folders](filebrowser.md#creating-files-and-folders)
* [Download files and folders](filebrowser.md#downloading-files-and-folders)

## Starting Filebrowser

{% tabs %}
{% tab title="Web UI" %}
Click **RUN A JOB** in the **Storage browser** widget on your `Neu.ro` dashboard.

![](../../.gitbook/assets/image%20%28215%29.png)

Select a preset from the drop-down list and click **RUN**.

![](../../.gitbook/assets/image%20%28210%29.png)
{% endtab %}

{% tab title="CLI" %}
Run the following command:

```text
$ neuro run -v storage://:/var/storage --http 80 --no-http-auth --browse filebrowser/filebrowser --noauth --root /var/storage
```

Filebrowser will be opened in a new window in your browser.
{% endtab %}
{% endtabs %}

## Working with Filebrowser

{% tabs %}
{% tab title="Computers" %}
FileBrowser instances are jobs that are listed in the Jobs tab with the tag `kind:web_widget target:filebrowser`. The platform storage is mounted at /var/storage which is also the root directory to let all files persist.

![FileBrowser instance](../../.gitbook/assets/stor_browser.jpg)

The FileBrowser UI has the following areas:

* **Toolbar:** Displays the list of actions you can perform. The toolbar shows additional options when you select a file or a folder - for example, Copy, Rename, Share, Move, Delete.
* **Options:** Displays additional options you have, such as My Files \(home\) link, New Folder, New File, and Settings.
* **Folders and Files:** Displays the contents of the current folder.

You can also use Shell from within FileBrowser by clicking the **Toggle shell** ![](../../.gitbook/assets/FB_Toggle.jpg) button and running commands in the **Shell** area.

![](../../.gitbook/assets/FB_Shell.jpg)

After you are done working with FileBrowser, ensure that you kill the job associated with it.

![](../../.gitbook/assets/image%20%2837%29.png)

All FileBrowser jobs are automatically killed after 24 hours.

### Creating Files and Folders

You can create files and folders from the FileBrowser. While creating files, FileBrowser has a basic text editor that lets you add content to the file. However, it is recommended that you upload instead of creating them.

**To create a folder:**

* From the Options area of the FileBrowser, click on the **New Folder** button. 

![](../../.gitbook/assets/FB_NewFolder.jpg)

* Enter a name for the new folder. 

![](../../.gitbook/assets/FB_NewDirectory.jpg)

**To create a file:**

* From the Options area of the FileBrowser, click on the **New File** button.

![](../../.gitbook/assets/FB_NewFile.jpg)

* Enter a name for the file and click **CREATE**.

![](../../.gitbook/assets/image%20%289%29.png)

* Click **Save** after entering the content of the files. 

![](../../.gitbook/assets/FB_NewFile_Save.JPG)

**To upload files:**

* From the Toolbar, click the **Upload** button. 

![](../../.gitbook/assets/FB_UploadButton.jpg)

* Select the file that you want to upload. The file is uploaded. You can edit a file by clicking on the **Edit** button. 

![](../../.gitbook/assets/FB_UpFile.JPG)

### Downloading Files and Folders

You can download files, folders, or the entire storage on FileBrowser using the download button.

**To download files and folders:**

* Browse to the necessary folder or file and click the **Download** button. If you don't select any files, the current folder is downloaded in its entirety.

![](../../.gitbook/assets/FB_Download.jpg) 

* If you're downloading a folder, select the format in which you want to download it.

![](../../.gitbook/assets/FB_DownFormat.jpg)
{% endtab %}

{% tab title="Mobile devices" %}
Neu.ro provides seamless support to mobile devices. You can access neu.ro and the FileBrowser from any mobile device to view, upload, download, and manage files on your mobile device. For example, you may want to upload sample images for the [**Object Detection**](https://docs.neu.ro/cookbook/object-detection) ML recipe from your mobile device.

### **Downloading files to your mobile device:**

* Log in to `neu.ro`, and start FileBrowser.

![](../../.gitbook/assets/mobile-dashboard.png) ![](../../.gitbook/assets/FBM_FileBrowser%20%281%29%20%281%29.jpg)

* Navigate to the folder to download files from, and select the file or folder that you want to download.

![](../../.gitbook/assets/FBM_Folder.jpg) ![](../../.gitbook/assets/FBM_Down_Select_1.jpg)

* Select **Download** from the options menu.

![](../../.gitbook/assets/FBM_Down_Select.jpg) ![](../../.gitbook/assets/FBM_DownloadDone%20%281%29%20%281%29.jpg)

The file is downloaded to your mobile device.

![](../../.gitbook/assets/FBM_DownloadDone%20%281%29.jpg)

### **Uploading files from your mobile device:**

* Log in to neu.ro, and start a FileBrowser.

![](../../.gitbook/assets/mobile-dashboard.png) ![](../../.gitbook/assets/FBM_FileBrowser.jpg)

* Navigate to the folder you want to upload files to from your mobile, and select **Upload** from the Options menu.

![](../../.gitbook/assets/FBM_Up_Folder.jpg) ![](../../.gitbook/assets/FBM_UploadButton.jpg)

* Navigate to the folder from where you want to upload the file and select the file.

![](../../.gitbook/assets/FBM_UploadFileFolder.jpg)

The file is uploaded to your mobile device.

![](../../.gitbook/assets/FBM_FileUploaded.jpg)
{% endtab %}
{% endtabs %}

