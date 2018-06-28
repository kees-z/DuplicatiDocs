## About this manual

This manual can be used as a guide to lead you through the features of Duplicati, but it can also be used as a reference guide.

Some parts are marked with an icon. Very important information is marked with a small triangle containing an exclamation mark. Example:

*****
> ![](icon_important.png) This is very important information.

*****

Additional information about a subject is marked with a small circle containing an i character. Example:

*****
> ![](icon_info.png) This is some additional information.

*****


This manual tries to cover all types of installations, but focuses on Windows installations. Most procedures are identical for installations on different operating systems, because many operations are controlled by a built-in web interface.  
If a procedure is different for an operating system other than Windows, a small section will be added to explain that procedure for Linux and/or OS X. OS-specific procedures are marked with these symbols:

*****
![](icon_windows_begin.png) This is an example of a Windows-specific procedure. ![](icon_windows_end.png)

*****

*****
![](icon_linux_begin.png) This is an example of a Linux-specific procedure. ![](icon_linux_end.png)

*****

*****
![](icon_apple_begin.png) This is an example of a MacOS-specific procedure. ![](icon_apple_end.png)

*****



## Overview

Duplicati is a backup client that securely stores encrypted, incremental, compressed backups on local storage, cloud storage services and remote file servers. The Duplicati project was inspired by Duplicity and had similar functionality until 2008\. In that year the storage model was redesigned completely and the program was rebuilt from scratch. This manual describes Duplicati 2, the version based on the new storage model.  
Duplicati can be installed on a variety of operating systems. Most common platforms are Windows, Linux and OSX.

Duplicati is not:

* **A file synchronization program.**  
Duplicati is a block based backup solution. Files are split up in small chunks of data (blocks),  which are optionally encrypted and compressed before they are sent to the backup location. In backup location, Duplicati uploads not original files but files that contain blocks of original files and other necessary data that allows Duplicati to restore stored files to its original form by restoration process. This block based backup system allows features like file versioning and deduplication. If you need to be able to access your files directly from the backup location, you will need file synchronization software, not block based backup software like Duplicati.
* **A hard disk imaging program.**  
Duplicati can make a backup of selected files and folders. Hard disk imaging software can create an image file of a complete volume or hard disk, including the boot sector. If you want to be able to restore a complete volume or hard disk, including the boot sector and operating system, you need a hard disk imaging solution.
* **Software that can make a backup of files that are stored in the cloud.**  
Duplicati needs to be installed on the host where the backup source files are stored. Optionally files and folders on locations in the local network can be selected for backup by using UNC paths. Duplicati can not log into cloud services over the internet to make a backup of remotely stored files.</span>

## Features

Duplicati has a lot of advanced features that can only be found in high-end enterprise backup solutions. Duplicati offers these features for free:

* **Strong encryption**  
Duplicati uses strong AES-256 encryption to protect your backups. It is designed following the TNO principle: Trust No One. For instance, all data is encrypted locally before it is transferred to the remote storage system. The password/key to your backup never leaves your computer. Instead of AES-256 you can use a local GPG instance to encrypt your backup.
* **Incremental backups**  
Duplicati performs a full backup initially. Afterwards, Duplicati updates the initial backup by adding the changed data only. That means, if only tiny parts of a huge file have changed, only those tiny parts are added to the backup. This saves time and space and the backup size usually grows slowly.</span>
* **Compression**  
All backup data is compressed before it is encrypted and uploaded. Duplicati supports Zip/Deflate or 7z/LZMA2 compression. For performance reasons, Duplicati detects files that are compressed already and adds those as they are to the Zip or 7z archives. For example, media files such as mp3, jpeg or mkv files contain very well compressed data already.
* **Online backup verification**  
Duplicati is built to work with simple storage systems. Many providers offer compatible storages and often at cheap prices. As a downside of this, some storage systems may store corrupted data. Most people only notice the corruption when they attempt to restore files they have lost and restoring fails. To avoid that Duplicati regularly downloads a random set of backup files, restores their content and checks their integrity. That way you can detect problems with your online storage before you run into trouble.
* **Deduplication**  
Duplicati analyzes the content of the files and stores data blocks. Due to that, Duplicati will find duplicate files and similar content and store them only once in the backup. As Duplicati analyzes the content of files it can handle situations very well if files and folders are moved or renamed. As the content does not change, the next backup will be tiny.
* **Fail-safe design**  
Duplicati is designed to handle various kinds of issues: Network hiccups, interrupted backups, unavailable or corrupt storage systems. Even if a backup run was interrupted, it can be continued at a later time. Duplicati will then backup everything that was missed in the last backup. And even if remote files get corrupted, Duplicati can try to repair them if local data is still present or restore as much as possible.
* **Web interface**  
Duplicati comes with a web interface. It can be used to configure and run backups on your local machine. But it also allows you to configure and run backups remotely on headless machines like a Network Attached Storage (NAS). Just install Duplicati on your NAS and configure and run it through its web interface.
* **Command Line interface**  
We did not forget about system admins! Duplicati offers all functions and feature via Duplicati.Commandline.exe. This allows you to add backup features to your scripts or run backups in a terminal window.
* **Meta data**  
Duplicati also stores the meta data of files in the backup. When backup files are restored, the timestamps (last modified, created) will also be restored as well as the system's access permissions. To avoid inaccessible files e.g. when the system user's have changed, restoring of access permissions is optional.
* **Scheduler**  
The built-in scheduler runs your backups automatically at the times and intervals you define. One backup everyday, at the weekend, every hour or even 3pm every 3rd Monday is possible. And even if a date is missed, Duplicati will run the job as soon as possible.
* **Auto-updater**  
Duplicati comes with a built-in updater that downloads and installs the latest available version for you. That way you can easily keep Duplicati up-to-date.
* **Backup open files**  
When a file is in use by a process or application, it usually cannot be read by another process, making it impossible to backup that file. On Windows systems, Duplicati can use Volume Shadow Copy Services (VSS). For Linux based devices Duplicati can use Logical Volume Management (LVM). VSS and LVM offer the possibility to create an application consistent snapshot of a volume, which is used by Duplicati to make a reliable backup of these open files.

## License

Duplicati is licensed under LGPL and available for Windows and Linux. The software is free to use, even commercially. More information about the LGPL licensing model can be found in [License Agreement](appendix-f-license-agreement).

## Supported backends

Duplicati can make backups to a large number of targets. For local backups, all devices can be used that are attached locally or using a UNC path, like:

* External USB hard disk drive
* USB thumb drive
* Shared folder on another computer in the same network
* Network-attached Storage (NAS)

Backups to these targets using the following standard network protocols are supported:

* FTP
* FTP (Alternative)
* OpenStack Object Storage / Swift
* S3 Compatible
* SFTP (SSH)
* WebDAV

The following Cloud Storage Providers are supported natively by Duplicati:

* Amazon Cloud Drive</span>
* Amazon S3
* Azure blob
* B2 Cloud Storage
* Box.com
* Dropbox
* Google Cloud Storage
* Google Drive
* HubiC
* Jottacloud
* Mega.nz
* Microsoft Office 365 Groups
* Microsoft OneDrive
* Microsoft OneDrive for Business
* Microsoft SharePoint
* OpenStack Simple Storage
* Rackspace CloudFiles
* Rclone
* Sia Decentralized Cloud

Other supported targets:

* Tahoe-LAFS

## System requirements

Duplicati must be installed on a device with a supported operating system. Currently, these operating systems are supported:

* Windows Vista and higher (both 32 and 64 bit versions)
* Windows Server 2008 and higher (both 32 and 64 bit versions)
* Linux
* Apple Mac OSX

Because many devices run on an operating system based on Linux, Duplicati can be installed on some devices that are not personal computers, like a NAS or Raspberry Pi.

Windows-based devices should have .NET Framework 3.5 or higher installed. For Linux and OSX, a recent version of Mono is a requirement.

Duplicati can make backups of files that are opened by other processes. For Windows, a snapshot of the file system is created using Volume Shadowcopy Services (VSS), LVM is used on Linux Systems. To be able to create a VSS snapshot, Duplicati needs C++ run-time components for Visual Studio 2015 to be installed and must be run with administrator privileges.

Duplicati is resource-friendly by design. There are no specific requirements for internal memory or processor performance.

Duplicati needs about 40 MB of free harddisk space for installation. However, additional space is required for execution:

* Duplicati creates a small database that contains program settings and all backup configurations.
* For each backup configuration, a local database is created, enabling Duplicati to retrieve information about files at the remote location, without actually upload or download files. This database makes Duplicati a lot faster, because a query is done to the local database instead of downloading remote files.  
The size of these local databases varies, depending on the number of source files selected for backup, the total amount of data and the chosen block size. In most situations, a local database consumes 10 MB to a few GB's of local storage capacity.
* Duplicati creates temporary files while doing backup or restore operations. The amount of storage needed depends on the chosen upload volume (DBLOCK) size. The default size is 50 MB, but this value can be modified for each backup job. A small number (1 to 5) of temporary DBLOCK files are stored locally before they are uploaded to the backup target. After a successful upload, these temporary files will be deleted automatically.

## The backup process explained

Traditional backup software makes a full backup at regular intervals (for example once a week). All other backups are incremental. These incremental backups send all new and changed files to the backup target. The drawback is that if a folder needs to be restored from the most recent backup, the latest full backup has to be restored first, followed by all incremental backups that were made after the latest full backup. This is a cumbersome and error-sensitive procedure.

Making a full backup every day results in reliable backups, but is very time consuming and resource-unfriendly. All source data has to be sent to and stored at the backup target every time the backup task is executed.

Duplicati combines the best of both worlds. When a backup is made, only changed parts of files are sent to the destination. From this point of view, Duplicati behaves like it is making an incremental backup. When one or more files (or all files and folders) need to be restored from the most recent backup, this backup (and all other ones) look like a full backup: all data can be restored with a single operation, without replaying a set of incremental backups.

Duplicati 2.0 introduced a new, revolutionary storage format for backups. The storage format is block-based. This means, it does not store the files, but chops all files into tiny blocks. Here is a simple explanation.

Imagine your local files consist of many small bricks in different shapes and colors. Duplicati takes your files, breaks them down into single bricks and stores these bricks in small bags. Whenever a bag is full, it is stored in a huge box (which is your online storage). When something changes, Duplicati puts new bricks into a new bag and puts it into the box. When a local file needs to be restored, Duplicati knows what bricks it needs and in which bags these are. So, it grabs the required bags, takes out the bricks and rebuilds your file. If the file is still on your computer (in a version you do not want anymore), Duplicati can just replace the wrong bricks, thus updating the existing file.

From time to time, Duplicati will notice that there are a few bags that contain bricks it does not need anymore. It grabs those bags, sorts the bricks. It throws away the bricks that are not needed anymore, then it puts the required bricks into new bags and puts them back into the box. Duplicati will also notice if there is a large number of bags that only contain a very small number of bricks. Duplicati grabs all those bags, takes out the bricks, puts them into a small number of new bags and puts these into the box.

And to say the good news again: There is no need to upload full backups regularly. This makes Duplicati a perfect choice for incremental backups of large media libraries.
