
A number of Command Line Utilities are included in the Duplicati package. Each command Line Utility serves a particular purpose. Using the Command Line Utilities, you can backup and restore files without using the Graphical User Interface (from the command prompt or by using your favorite task scheduler), launch a server instance, register Duplicati as a Windows service and perform disaster recovery tasks.

All Command Line Utilities can be found in the Duplicati program folder.

## Duplicati.GUI.TrayIcon.exe

This utility starts the Duplicati tray icon. Without additional parameters specified, the included webserver is activated. The webserver listens on TCP port 8200 by default. If port 8200 is unavailable, port 8300 is tried, increasing until a free port is found. You can disable the internal webserver if you are using a separate instance of the Duplicati Server component.

These command line options are supported:

* `--help`  
Displays the integrated help text.
* `--toolkit:`  
Choose the toolkit used to generate the TrayIcon, note that it will fail if the selected toolkit is not supported on this machine.  
Supported toolkits: `winforms`.
* `--hosturl`  
Supply the url that the TrayIcon will connect to and show status for.
* `--no-hosted-server`  
Set this option to not spawn a local service, use if the TrayIcon should connect to a running service.
* `--read-config-from-db`  
Set this option to read server connection info for running service from its database (only together with no-hosted-server).
* `--browser-command`  
Set this option to override the default browser detection.
* `--detached-process`  
This option runs the tray-icon in detached mode, meaning that the process will exit immediately and not send output to the console of the caller.

Additionally, these server options are also supported:

* `--unencrypted-database`
* `--portable-mode`
* `--log-file`
* `--log-level`
* `--webservice-webroot`
* `--webservice-port`
* `--webservice-sslcertificatefile`
* `--webservice-sslcertificatepassword`
* `--webservice-interface`
* `--ping-pong-keepalive`
* `--log-retention`
* `--server-datafolder`
* `--server-encryption-key`

See [Duplicati.Server.exe](#_Duplicati.Server.exe) for more information about these command line options.



## Duplicati.Server.exe

Once started, The Duplicati Server component can perform a number of tasks in the background. A scheduler keeps track of configured backup jobs and starts them at scheduled points in time. Backup jobs can be configured and monitored using the built-in webserver. If no port is specified, the webserver listens on TCP port 8200\. If this port is unavailable, the webserver will try to start listening on port 8300 and so on, until an available port is found.

The server component is completely included in Duplicati.GUI.TrayIcon.exe, so launching this utility will result in starting the Duplicati Server component also, unless disabled with the `--no-hosted-server` command line option.

The following command line options can be specified:

* `--help`  
Displays the help text.
* `--unencrypted-database`  
Disables database encryption.
* `--portable-mode`  
Activates portable mode where the database is placed below the program executable.
* `--log-file`  
Outputs log information to the file given.
* `--log-level`  
Determines the amount of information written in the log file.
* `--webservice-webroot`  
The path to the folder where the static files for the webserver is present. The folder must be located beneath the installation folder.
* `--webservice-port`  
The port the webserver listens on. Multiple values may be supplied with a comma in between.
* `--webservice-sslcertificatefile`  
The certificate and key file in PKCS #12 format the webserver use for SSL. Only RSA/DSA keys are supported.
* `--webservice-sslcertificatepassword`  
The password for decryption of certificate PKCS #12 file.
* `--webservice-interface`  
The interface the webserver listens on. The special values `*` and `any` means any interface. The special value `loopback` means the loopback adapter.
* `--webservice-password`  
The password required to access the webserver. This option is saved so you do not need to set it on each run. Setting an empty value disables the password.
* `--ping-pong-keepalive`  
When running as a server, the service daemon must verify that the process is responding. If this option is enabled, the server reads stdin and writes a reply to each line read.
* `--log-retention`  
Set the time after which log data will be purged from the database.
* `--server-datafolder`  
Duplicati needs to store a small database with all settings. Use this option to choose where the settings are stored. This option can also be set with the environment variable `DUPLICATI_HOME`.
* `--server-encryption-key`  
This option sets the encryption key used to scramble the local settings database. This option can also be set with the environment variable `DUPLICATI_DB_KEY`. Use the option `--unencrypted-database` to disable the database scrambling.


## Duplicati.WindowsService.exe

This command line tool can be used to register Duplicati.Server.exe as a Windows service. Windows services are started in the background at bootup time, regardless if a user is logged on or not.

The service is started with the SYSTEM account by default. The SYSTEM account has full access to the complete local filesystem. As a result, all files on the local system can be accessed by the Duplicati Web interface. Therefore it is strongly recommended to secure the web interface with a password when using Duplicati as a Windows service.

To register Duplicati.Server.exe as a Windows service, open an elevated command prompt (Run as Administrator). `Duplicati.WindowsService.exe` accepts the following commands:

* `install`  
Installs the service.
* `uninstall`  
Uninstalls the service.

Supported options for the install command:  
* `/localuser`  
Installs the service as a local user.

It is possible to pass arguments to `Duplicati.Server.exe`, simply add them to the commandline:  
`Duplicati.WindowsService.exe install --webservice-interface=loopback --log-retention=3M`

See [Duplicati.Server.exe](#_Duplicati.Server.exe) for more information about supported command line options.

## Duplicati.CommandLine.BackendTester.exe

Before you start using a particular backend to use as a backup target, you can use the Backend Tester to get an indication of the integrity of that backend. The Backend Tester will perform the following actions:

* Generate a number of files with a random size and random filenames.
* Upload these files to the backend.
* Download the uploaded files.
* Check the hash of each downloaded file to verify its integrity.
* Repeat this procedure a number of times.
* The results of all individual actions are reported back.

You can specify how many files will be generated, what size they should have, which characters are allowed for the file names and how many times the test procedure should be repeated.

Usage:  
`<protocol>://<username>:<password>@<path>`

Supported protocols are:  
`aftp`, `amzcd`, `azure`, `b2`, `box`, `cloudfiles`, `dropbox`, `file`, `ftp`, `googledrive`, `gcs`, `hubic`, `jottacloud`, `mega`, `onedrive`, `openstack`, `s3`, `od4b`, `mssp`, `ssh`, `tahoe`, `webdav`

Use one or more of the following command line options:

* `--reruns`  
Value: Integer  
The number of test runs to perform.  
A number that describes how many times the test is performed.  
Default value: `5`
* `--tempdir`  
Value: Path  
The path used to store temporary files.  
The backend tester will use the system default temp path. You can set this option to choose another path.
* `--extended-chars`  
Value: String  
A list of allowed extended filename chars. A list of characters besides `{a-z, A-Z, 0-9}` to use when generating filenames.  
Default value:`-_',=)(&%$#@! +`
* `--number-of-files`  
Value: Integer  
The number of files to test with.  
An integer describing how many files to upload during a test run.  
Default value: `10`
* `--min-file-size`  
Value: Size  
The minimum allowed file size.  
File sizes are chosen at random, this value is the lower bound.  
Default value: `1kb`
* `--max-file-size`  
Value: Size  
The maximum allowed file size.  
File sizes are chosen at random, this value is the upper bound.  
Default value: `50mb`
* `--min-filename-length`  
Value: Integer  
The minimum allowed filename length.  
File name lengths are chosen at random, this value is the lower bound.  
Default value: `5`
* `--max-filename-length`  
Value: Integer  
The minimum allowed filename length.  
File name lengths are chosen at random, this value is the upper bound  
Default value: `80`
* `--auto-create-folder`  
Value: Boolean  
Allows automatic folder creation.  
A value that indicates if missing folders are created automatically.  
Default value: `false`
* `--skip-overwrite-test`  
Value: Boolean  
Bypasses the overwrite test.  
A value that indicates if dummy files should be uploaded prior to uploading the real files.  
Default value: `false`
* `--auto-clean`  
Value: Boolean  
Removes any files found in target folder.  
A value that indicates if all files in the target folder should be deleted before starting the first test.  
Default value: `false`
* `--force`  
Value: Boolean  
Activates file deletion.  
A value that indicates if existing files should really be deleted when using auto-clean.  
Default value: `false`

Example:  
`Duplicati.CommandLineBackendTester.exe ftp://user:pass@server/folder`



## Duplicati.CommandLine.BackendTool.exe

A wide range of backends can be used by Duplicati, both using standard protocols like FTP or WebDAV and proprietary cloud storage services like Google Drive, Microsoft OneDrive or Dropbox. The requirements for these backends are very low. Basically, the only requirements are that Duplicati can perform the following operations to the backend:

* **PUT**  
Of course Duplicati should be able to write files to the backend in order to store the data to be backed up.
* **GET**  
For restore operations and verification of backups, Duplicati needs to be able to read (download) files from the backend.
* **LIST**  
Duplicati needs to retrieve the contents of the backend by requesting a list of files.
* **DELETE**  
To remove old or unneeded files and for reorganizing backend files, Duplicati should be able to delete files from the backend.
* **CREATEFOLDER**  
When adding a new backup job, Duplicati can automatically create a new folder at the backend to store the backup files. Once configured, the Create Folder is no longer needed.

There are no more requirements. Duplicati doesn't need to rename files or add data to existing files.

With the Backend tool you can perform the 5 operations to the backend from the command line. This could be useful for testing purposes or disaster recovery (if one or more backup files are missing or corrupt).

Use this tool very carefully! Usually Duplicati will take care of checking, repairing and recovering from inconsistencies at the backend. Incorrect use of this tool may cause unrecoverable data loss. Only advanced users should use it.

Usage:  
`Duplicati.CommandLine.BackendTool.exe <command> <protocol>://<username>:<password>@<path> [filename]`

The following commands are supported:  
`GET`, `PUT`, `LIST`, `DELETE`, `CREATEFOLDER`

he supported protocols are:  
`aftp`, `amzcd`, `azure`, `b2`, `box`, `cloudfiles`, `dropbox`, `file`, `ftp`, `googledrive`, `gcs`, `hubic`, `jottacloud`, `mega`, `onedrive`, `openstack`, `s3`, `od4b`, `mssp`, `ssh`, `tahoe`, `webdav`

Example:  
`LIST ftp://user:pass@server/folder`



## Duplicati.Library.Snapshots.exe

Duplicati can create snapshots, enabling making backups of files that are in use. Windows provides Volume Shadowcopy Services (VSS) to backup open files, Linux and OS X provide LVM for the same purpose. You can use Duplicati.Library.Snapshots.exe to test if snapshots can be created successfully and if they can be used to access open files.

To test this, this utility creates a small file and locks it. Then a snapshot is created, which is tried to be read. If the open file can be read, open files can be backed up. The Duplicati.Library.Snapshot utility reports these steps back, which may help troubleshooting failing backups of open files.

The tool doesn't need any command line arguments, it just needs to be executed from a command line interface with administrative privileges.

## Duplicati.Library.AutoUpdater.exe

With this tool you can check if a new Duplicati version is available and install updates unattended. You can subscribe to one of the following update channels: Stable, Beta, Experimental, Canary, Nightly, Debug. The Stable Update channel guarantees that you only will receive versions that were thoroughly tested, the Debug channel is for developers only.

How the Updater tool works, can be customized by some environment variables:

* `AUTOUPDATER_Duplicati_SKIP_UPDATE`  
Disables updates completely.
* `AUTOUPDATER_Duplicati_POLICY`  
Choose how to handle updates, valid settings: `CheckBefore`, `CheckDuring`, `CheckAfter`, `InstallBefore`, `InstallDuring`, `InstallAfter`, `Never`
* `AUTOUPDATER_Duplicati_URLS`  
Use alternate updates urls.
* `AUTOUPDATER_Duplicati_CHANNEL`  
Choose different channel than the default Experimental, valid settings: `Stable`, `Beta`, `Experimental`, `Canary`, `Nightly`, `Debug`

Updates are downloaded from: https://updates.duplicati.com/experimental/latest.manifest and 
https://alt.updates.duplicati.com/experimental/latest.manifest

Updates are installed in `C:\ProgramData\Duplicati\updates`.

Usage:  
`Duplicati.Library.AutoUpdater.exe [LIST|CHECK|INSTALL|HELP]`

The following commands are supported:

* `help`  
Displays the help text.
* `list`  
Show which updates are downloaded and available at your local system.
* `check`  
Check online if new updates are available in the currently used update channel.
* `install`  
Install the latest update from the currently selected update channel.



## Duplicati.CommandLine.RecoveryTool.exe

This tool can be used in very specific situations, where you have to restore data from a corrupted backup. The procedure for recovering from this scenario is covered in [Disaster Recovery](#_Disaster_Recovery).

Additionally, you can use the Recovery Tool to convert your backup files to another compression type. When creating a new backup job, you have to choose a compression type (default is Zip). After the first backup is made, the compression type cannot be changed. With the Duplicati Recovery Tool, you can download all files from the backend to your local filesystem, encrypt and uncompress them.

Then you can recompress these files using a different compression type, re-encrypt the files and upload them back to the backend. Change the backup configuration to use the new compression type for future backup operations to complete the procedure.

Use the Duplicati Recoverytool with the `recompress` command for this task:
`Duplicati.RecoveryTool.exe recompress <targetcompression> <remoteurl> <localfolder> --reupload --reencrypt [options]`

This command downloads all files to the local folder specified by `<localfolder>`. Then files are uncompressed and recompressed using the compression type specified with `<targetcompression>`. If `--reencrypt` is supplied, all files are re-encrypted using the same passphrase. If `--reupload` is supplied, files with the old compression type are deleted and recompressed files are uploaded back to remote storage.

*****
> ![](icon_info.png) This is a radical operation to your backup files. Therefore, it is recommended to keep a copy of your remote files before this operation is performed. Using the Duplicati Recovery Tool incorrectly may result in unusable backup files.

*****

*****
> ![](icon_important.png) Warning: Before recompress delete local database and after recompress recreate local database before executing any operation on backup. This allows Duplicati to read new file names from remote storage.

*****
