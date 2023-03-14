# Filebrowser

## Introduction

Filebrowser is an intuitive web interface used to manage files and folders. It's based on the popular file browsing interface available on [git](https://github.com/filebrowser/filebrowser). Filebrowser lets you:

* [Create files and folders](filebrowser.md#creating-files-and-folders)
* [Download files and folders](filebrowser.md#downloading-files-and-folders)

## Starting Filebrowser

{% tabs %}
{% tab title="Web UI" %}
Click **RUN A JOB** in the **Storage browser** widget on your `Neu.ro` dashboard.

![](<../../.gitbook/assets/image (195).png>)

Next, you have two options:

* If you want to access your own storage with Filebrowser, just click **RUN** in the **Storage browser** dialog box:

![
](<../../.gitbook/assets/image (52).png>)

* If you want to access a storage volume that was shared with you, first click **ADD NEW VOLUME**. Then, fill in the **Volume Path** field and specify which access rights you have for this volume: Read Only (**RO**) or Read/Write (**RW**). Once this is done, click **RUN**:

![](broken-reference)
{% endtab %}

{% tab title="CLI" %}
Run the following command:

```
$ neuro run -v storage://:/var/storage --http 80 --no-http-auth --browse filebrowser/filebrowser --noauth --root /var/storage
```

Filebrowser will be opened in a new tab of your browser.
{% endtab %}
{% endtabs %}

## Working with Filebrowser

{% tabs %}
{% tab title="Computers" %}
FileBrowser instances are jobs that are listed in the Jobs tab with the tag `kind:web_widget target:filebrowser`. The platform storage is mounted at /var/storage which is also the root directory to let all files persist.

![FileBrowser instance](../../.gitbook/assets/Stor\_Browser.jpg)

The FileBrowser UI has the following areas:

* **Toolbar:** Displays the list of actions you can perform. The toolbar shows additional options when you select a file or a folder - for example, Copy, Rename, Share, Move, Delete.
* **Options:** Displays additional options you have, such as My Files (home) link, New Folder, New File, and Settings.
* **Folders and Files:** Displays the contents of the current folder.

You can also use Shell from within FileBrowser by clicking the **Toggle shell** ![](../../.gitbook/assets/FB\_Toggle.jpg) button and running commands in the **Shell** area.

![](../../.gitbook/assets/FB\_Shell.jpg)

After you are done working with FileBrowser, ensure that you kill the job associated with it.

![](<../../.gitbook/assets/image (101).png>)

All FileBrowser jobs are automatically killed after 24 hours.

### Creating Files and Folders

You can create files and folders from the FileBrowser. While creating files, FileBrowser has a basic text editor that lets you add content to the file. However, it is recommended that you upload instead of creating them.

**To create a folder:**

* From the Options area of the FileBrowser, click on the **New Folder** button.&#x20;

![](../../.gitbook/assets/FB\_NewFolder.jpg)

* Enter a name for the new folder.&#x20;

![](../../.gitbook/assets/FB\_NewDirectory.jpg)

**To create a file:**

* From the Options area of the FileBrowser, click on the **New File** button.

![](../../.gitbook/assets/FB\_NewFile.jpg)

* Enter a name for the file and click **CREATE**.

![](<../../.gitbook/assets/image (117).png>)

* Click **Save** after entering the content of the files.&#x20;

![](../../.gitbook/assets/FB\_NewFile\_Save.JPG)

**To upload files:**

* From the Toolbar, click the **Upload** button.&#x20;

![](../../.gitbook/assets/FB\_UploadButton.jpg)

* Select the file that you want to upload. The file is uploaded. You can edit a file by clicking on the **Edit** button.&#x20;

![](../../.gitbook/assets/FB\_UpFile.JPG)

### Downloading Files and Folders

You can download files, folders, or the entire storage on FileBrowser using the download button.

**To download files and folders:**

* Browse to the necessary folder or file and click the **Download** button. If you don't select any files, the current folder is downloaded in its entirety.

![](../../.gitbook/assets/FB\_Download.jpg)&#x20;

* If you're downloading a folder, select the format in which you want to download it.

![](../../.gitbook/assets/FB\_DownFormat.jpg)
{% endtab %}

{% tab title="Mobile devices" %}
Neu.ro provides seamless support to mobile devices. You can access neu.ro and the FileBrowser from any mobile device to view, upload, download, and manage files on your mobile device. For example, you may want to upload sample images for the [**Object Detection**](https://docs.neu.ro/cookbook/object-detection) ML recipe from your mobile device.

### **Downloading files to your mobile device:**

* Log in to `neu.ro`, and start FileBrowser.

![](../../.gitbook/assets/Mobile-dashboard.png) ![](../../.gitbook/assets/FBM\_FileBrowser.jpg)

* Navigate to the folder to download files from, and select the file or folder that you want to download.

![](../../.gitbook/assets/FBM\_Folder.jpg) ![](../../.gitbook/assets/FBM\_Down\_Select\_1.jpg)

* Select **Download** from the options menu.

![](../../.gitbook/assets/FBM\_Down\_Select.jpg) ![](<../../.gitbook/assets/FBM\_DownloadDone (2).jpg>)

The file is downloaded to your mobile device.

![](<../../.gitbook/assets/FBM\_DownloadDone (1).jpg>)

### **Uploading files from your mobile device:**

* Log in to neu.ro, and start a FileBrowser.

![](../../.gitbook/assets/Mobile-dashboard.png) ![](<../../.gitbook/assets/FBM\_FileBrowser (2).jpg>)

* Navigate to the folder you want to upload files to from your mobile, and select **Upload** from the Options menu.

![](../../.gitbook/assets/FBM\_Up\_Folder.jpg) ![](../../.gitbook/assets/FBM\_UploadButton.jpg)

* Navigate to the folder from where you want to upload the file and select the file.

![](../../.gitbook/assets/FBM\_UploadFileFolder.jpg)

The file is uploaded to your mobile device.

![](../../.gitbook/assets/FBM\_FileUploaded.jpg)
{% endtab %}
{% endtabs %}
