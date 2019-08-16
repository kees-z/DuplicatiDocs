
For each Duplicati command you have to specify a number of arguments. When performing a backup, you must supply the location and credentials for the backup files and one or more source folders. Optionally, you can specify one or more advanced options. These options will give you more control to how the command will be executed. Also additional features, like reporting, can be configured by supplying some advanced options.

Those additional options should only be used with care. For normal operation none of them should ever be required. Use this alphabetical list as a reference to find the advanced options that fit your needs.

## Core options

These options can be used to influence the behavior of the Duplicati backup engine.

### allow-full-removal
`--allow-full-removal = false`   
By default, the last fileset cannot be removed. This is a safeguard to make sure that all remote data is not deleted by a configuration mistake. Use this flag to disable that protection, such that all filesets can be deleted.

### allow-missing-source
`--allow-missing-source = false`  
Use this option to continue even if some source entries are missing.

### allow-passphrase
`--allow-passphrase-change = false`  
Use this option to allow the passphrase to change, note that this option is not permitted for a backup or repair operation.

### allow-sleep
`--allow-sleep = false`  
Allow system to enter sleep power modes for inactivity during backup/restore operations (Windows/OSX only).

### all-versions
`--all-versions = false`  
When searching for files, only the most recent backup is searched. Use this option to show all previous versions too.

### asynchronous-upload-folder
`--asynchronous-upload-folder = C:\Users\User\AppData\Local\Temp\`    
The pre-generated volumes will be placed into the temporary folder by default, this option can set a different folder for placing the temporary volumes, despite the name, this also works for synchronous runs.

### asynchronous-upload-limit
`--asynchronous-upload-limit = 4`  
When performing asynchronous uploads, Duplicati will create volumes that can be uploaded. To prevent Duplicati from generating too many volumes, this option limits the number of pending uploads. Set to zero to disable the limit

### auto-cleanup
`--auto-cleanup = false`  
If a backup is interrupted there will likely be partial files present on the backend. Using this flag, Duplicati will automatically remove such files when encountered.

### auto-compact-interval
`--auto-compact-interval = 0m`  
The minimum amount of time that must elapse after the last compaction before another will be automatically triggered at the end of a backup job. Automatic compaction can be a long-running process and may not be desirable to run after every single backup.

### auto-update
`--auto-update = false`  
Set this option if you prefer to have the commandline version automatically update

### auto-vacuum
`--auto-vacuum = false`  
Some operations that manipulate the local database leave unused entries behind. These entries are not deleted from a hard drive until a VACUUM operation is run. This operation saves disk space in the long run but needs to temporarily create a copy of all valid entries in the database. Setting this to true will allow Duplicati to perform VACUUM operations at its discretion.

### auto-vacuum-interval
`--auto-vacuum-interval = 0m`  
The minimum amount of time that must elapse after the last vacuum before another will be automatically triggered at the end of a backup job. Automatic vacuum can be a long-running process and may not be desirable to run after every single backup.

### backup-name
`--backup-name = Duplicati.CommandLine`  
A display name that is attached to this backup. Can be used to identify the backup when sending mail or running scripts.

### backup-test-samples
`--backup-test-samples = 1`  
After a backup is completed, some files are selected for verification on the remote backend. Use this option to change how many. If this value is set to 0 or the option --no-backend-verification is set, no remote files are verified

### block-hash-algorithm
`--block-hash-algorithm = SHA256`  
This is a very advanced option! This option can be used to select a block hash algorithm with smaller or larger hash size, for performance or storage space reasons.

### blocksize
`--blocksize = 100kb`  
The block size determines how files are fragmented. Choosing a large value will cause a larger overhead on file changes, choosing a small value will cause a large overhead on storage of file lists. Note that the value cannot be changed after remote files are created.

### changed-files
`--changed-files` 
This option can be used to limit the scan to only files that are known to have changed. This is usually only activated in combination with a filesystem watcher that keeps track of file changes.

### check-filetime-only
`--check-filetime-only = false`  
This flag instructs Duplicati to not look at metadata or filesize when deciding to scan a file for changes. Use this option if you have a large number of files and notice that the scanning takes a long time with unmodified files.

### compression-extension-file
`--compression-extension-file = C:\Program Files\Duplicati 2\default_compressed_extensions.txt`  
This property can be used to point to a text file where each line contains a file extension that indicates a non-compressible file. Files that have an extension found in the file will not be compressed, but simply stored in the archive. The file format ignores any lines that do not start with a period, and considers a space to indicate the end of the extension. A default file is supplied, that also serves as an example. The default file is placed in `C:\Program Files\Duplicati 2\default_compressed_extensions.txt`.

### compression-module
`--compression-module = zip`  
Duplicati supports pluggable compression modules. Use this option to select a module to use for compression. This is only applied when creating new volumes, when reading an existing file, the filename is used to select the compression module.

### concurrency-block-hashers
`--concurrency-block-hashers = 2`  
Use this option to set the number of processes that perform hashing of data.

### concurrency-compressors
`--concurrency-compressors = 2`  
Use this option to set the number of processes that perform compression of output data.

### concurrency-max-threads
`--concurrency-max-threads = 0`  
Use this option to set the maximum number of threads used. Setting this value to zero or less will dynamically balance the number of active threads to fit the hardware.

### console-log-filter
`--console-log-filter`  
This option accepts filters that removes or includes messages regardless of their log level. Multiple filters are supported by separating with :`.`
Filters are matched against the log tag and assumed to be including, unless they start with `-`. Regular expressions are supported within hard braces. Example: `+Path*:+*Mail*:-[.*DNS]`

### console-log-level
`--console-log-level = Warning`  
Console information level

### control-files
`--control-files = false`  
Use control files.

### dblock-size
`--dblock-size = 50mb`  
This option can change the maximum size of dblock files. Changing the size can be useful if the backend has a limit on the size of each individual file.

### dbpath
`--dbpath`  
Path to the file containing the local cache of the remote file database.

### debug-output
`--debug-output = false`  
Activating this option will make some error messages more verbose, which may help you track down a particular issue.

### debug-retry-errors
`--debug-retry-errors = false`  
When an error occurs, Duplicati will silently retry, and only report the number of retries. Enable this option to have the error messages displayed when a retry is performed.

### default-filters
`--default-filters`  
Exclude files that match the given filter sets. Which default filter sets should be used. Valid sets are `Windows`, `OSX`, `Linux` and `All`. If this parameter is set with no value, the set for the current operating system will be used.

### deleted-files
`--deleted-files`  
This option can be used to supply a list of deleted files. This option will be ignored unless the option `--changed-files` is also set.

### disable-autocreate-folder
`--disable-autocreate-folder = false`  
If Duplicati detects that the target folder is missing, it will create it automatically. Activate this option to prevent automatic folder creation.

### disable-filepath-cache
`--disable-filepath-cache = true`  
This option can be used to reduce the memory footprint by not keeping paths and modification timestamps in memory.

### disable-filetime-check
`--disable-filetime-check = false`  
The operating system keeps track of the last time a file was written. Using this information, Duplicati can quickly determine if the file has been modified. If some application deliberately modifies this information, Duplicati won't work correctly unless this flag is set.

### disable-on-battery
`--disable-on-battery = false`  
When this flag is enabled, a scheduled backup will not run if the system is detected to be running on battery power (manual or command line backups will still be run).  If the detected power source is mains (i.e., AC) or unknown, then scheduled backups will proceed as normal.

### disable-module
`--disable-module`  
Supply one or more module names, separated by commas to unload them.

### disable-piped-streaming
`--disable-piped-streaming = false`  
Use this option to disable multithreaded handling of up- and downloads, that can significantly speed up backend operations depending on the hardware you're running on and the transfer rate of your backend.

### disable-streaming-transfers
`--disable-streaming-transfers = false`  
Enabling this option will disallow usage of the streaming interface, which means that transfer progress bars will not show, and bandwidth throttle settings will be ignored.

### disable-synthetic-filelist
`--disable-synthetic-filelist = false`  
If Duplicati detects that the previous backup did not complete, it will generate a filelist that is a merge of the last completed backup and the contents that were uploaded in the incomplete backup session.

### disable-time-tolerance
`--disable-time-tolerance = false`  
When matching timestamps, Duplicati will adjust the times by a small fraction to ensure that minor time differences do not cause unexpected updates. If the option `--keep-time` is set to keep a week of backups, and the backup is made the same time each week, it is possible that the clock drifts slightly, such that full week has just passed, causing Duplicati to delete the older backup earlier than expected. To avoid this, Duplicati inserts a 1% tolerance (max 1 hour). Use this option to disable the tolerance, and use strict time checking.

### dont-compress-restore-paths
`--dont-compress-restore-paths = false`  
When restore a subset of a backup into a new folder, the shortest possible path is used to avoid generating deep paths with empty folders. Use this flag to skip this compression, such that the entire original folder structure is preserved, including upper level empty folders.

### dont-read-manifests
`--dont-read-manifests = false`  
This option will make sure the contents of the manifest file are not read. This also implies that file hashes are not checked either. Use only for disaster recovery.

### dry-run
`--dry-run = false`  
This option can be used to experiment with different settings and observe the outcome without changing actual files.

### enable-module
`--enable-module`  
Supply one or more module names, separated by commas to load them.

### encryption-module
`--encryption-module = aes`  
Duplicati supports pluggable encryption modules. Use this option to select a module to use for encryption. This is only applied when creating new volumes, when reading an existing file, the filename is used to select the encryption module.

### exclude
`--exclude`  
Exclude files that match this filter. The special characterc `*` means any number of character, and the special character `?` means any single character, use `*.txt` to exclude all files with a txt extension. Regular expressions are also supported and can be supplied by using hard braces, i.e. `[.*\.txt]`.

### exclude-empty-folders
`--exclude-empty-folders = false`  
Use this option to remove all empty folders from a backup.

### exclude-files-attributes
`--exclude-files-attributes`  
Use this option to exclude files with certain attributes. Use a comma separated list of attribute names to specify more than one. Possible values are: `ReadOnly`, `Hidden`, `System`, `Directory`, `Archive`, `Device`, `Normal`, `Temporary`, `SparseFile`, `ReparsePoint`, `Compressed`, `Offline`, `NotContentIndexed`, `Encrypted`, `IntegrityStream`, `NoScrubData`.

### file-hash-algorithm
`--file-hash-algorithm = SHA256`  
This is a very advanced option! This option can be used to select a file hash algorithm with smaller or larger hash size, for performance or storage space reasons.

### file-read-buffer-size
`--file-read-buffer-size = 0kb`  
Use this size to control how many bytes a read from a file before processing.

### force-locale
`--force-locale`  
By default, your system locale and culture settings will be used. In some cases you may prefer to run with another locale, for example to get messages in another language. This option can be used to set the locale. Supply a blank string to choose the "Invariant Culture".

### full-block-verification
`--full-block-verification = false`  
Use this option to increase verification by checking the hash of blocks read from a volume before patching restored files with the data.

### full-remote-verification
`--full-remote-verification = false`  
After a backup is completed, some files are selected for verification on the remote backend. Use this option to turn on full verification, which will decrypt the files and examine the insides of each volume, instead of simply verifying the external hash, If the option `--no-backend-verification` is set, no remote files are verified. This option is automatically set when then verification is performed directly.

### full-result
--full-result = false  
Use this option to increase the amount of output generated as the result of the operation, including all filenames.

### hardlink-policy
`--hardlink-policy = All`  
Use this option to handle hardlinks (only works on Linux/OSX). The `first` option will record a hardlink ID for each hardlink to avoid storing hardlinked paths multiple times. The option `all` will ignore hardlink information, and treat each hardlink as a unique path. The option `none` will ignore all hardlinks with more than one link.

### ignore-filenames
`--ignore-filenames`  
Use this option to set a filename, or list of filenames, that indicate exclusion of a folder which contains it. A common use would be to have a file named something like ".nobackup" and place this file into folders that should not be backed up.

### include
`--include`  
Include files that match this filter. The special character `*` means any number of character, and the special character `?` means any single character, use `*.txt` to include all files with a txt extension. Regular expressions are also supported and can be supplied by using hard braces, i.e.`[.*\.txt]`.

### index-file-policy
`--index-file-policy = Full`  
The index files are used to limit the need for downloading dblock files when there is no local database present.  
The more information is recorded in the index files, the faster operations can proceed without the database. The tradeoff is that larger index files take up more remote space and which may never be used.

### keep-time
`--keep-time`  
Use this option to set the timespan in which backups are kept.

### keep-versions
`--keep-versions = 0`  
Use this option to set number of versions to keep, supply `-1` to keep all versions.

### list-folder-contents
`--list-folder-contents = false`  
When searching for files, all matching files are returned. Use this option to return only the entries found in the folder specified as filter.

### list-prefix-only
`--list-prefix-only = false`  
When searching for files, all matching files are returned. Use this option to return only the largest common prefix path.

### list-sets-only
`--list-sets-only = false`  
Use this option to only list filesets and avoid traversing file names and other metadata which slows down the process.

### list-verify-uploads
`--list-verify-uploads = false`  
Verify uploads by listing contents.

### log-file
`--log-file`  
Log internal information.

### log-file-log-filter
`--log-file-log-filter`  
This option accepts filters that removes or includes messages regardless of their log level. Multiple filters are supported by separating with :.
Filters are matched against the log tag and assumed to be including, unless they start with `-`. Regular expressions are supported within hard braces. Example: `+Path*:+*Mail*:-[.*DNS]`

### log-file-log-level
`--log-file-log-level = Warning`  
Log file information level

### log-level
`--log-level = Warning`  
Specifies the amount of log information to write into the file specified by `--log-file`.

### log-retention
`--log-retention = 30D`  
Set the time after which log data will be purged from the database.

### no-auto-compact
`--no-auto-compact = false`  
If a large number of small files are detected during a backup, or wasted space is found after deleting backups, the remote data will be compacted. Use this option to disable such automatic compacting and only compact when running the compact command.

### no-backend-verification
`--no-backend-verification = false`  
If this flag is set, the local database is not compared to the remote filelist on startup. The intended usage for this option is to work correctly in cases where the filelisting is broken or unavailable.

### no-connection-reuse
`--no-connection-reuse = false`  
Duplicati will attempt to perform multiple operations on a single connection, as this avoids repeated login attempts, and thus speeds up the process. This option can be used to ensure that each operation is performed on a separate connection.

### no-encryption
`--no-encryption = false`  
If you store the backups on a local disk, and prefer that they are kept unencrypted, you can turn of encryption completely by using this switch.

### no-local-blocks
`--no-local-blocks = false`  
Duplicati will attempt to use data from source files to minimize the amount of downloaded data. Use this option to skip this optimization and only use remote data.

### no-local-db
`--no-local-db = false`  
When listing contents or when restoring files, the local database can be skipped. This is usually slower, but can be used to verify the actual contents of the remote store.

### number-of-retries
`--number-of-retries = 5`  
If an upload or download fails, Duplicati will retry a number of times before failing. Use this to handle unstable network connections better.

### overwrite
`--overwrite = false`  
Use this option to overwrite target files when restoring, if this option is not set the files will be restored with a timestamp and a number appended.

### parameters-file
`--parameters-file`  
This option can be used to store some or all of the options given to the commandline client. The file must be a plain text file, UTF-8 encoding is preferred. Each line in the file should be of the format `--option=value`. The special options `--source` and `--target` can be used to override the localpath and the remote destination uri, respectively. The options in this file take precedence over the options provided on the commandline. You cannot specify filters in both the file and on the commandline. Instead, you can use the special`--replace-filter`, `--append-filter`, or `--prepend-filter` options to specify filters inside the parameter file. Each filter must be prefixed with either a `+` or a `-`, and multiple filters must be joined with `;`.

### passphrase
`--passphrase`  
Supply a passphrase that Duplicati will use to encrypt the backup volumes, making them unreadable without the passphrase. This variable can also be supplied through the environment variable `PASSPHRASE`.

### patch-with-local-blocks
`--patch-with-local-blocks = false`  
Enable this option to look into other files on this machine to find existing blocks. This is a fairly slow operation but can limit the size of downloads.

### prefix
`--prefix = duplicate`  
A string used to prefix the filenames of the remote volumes, can be used to store multiple backups in the same remote folder. The prefix cannot contain a hyphen (`-`), but can contain all other characters allowed by the remote storage.

### quiet-console
`--quiet-console = false`  
If this option is set, progress reports and other messages that would normally go to the console will be redirected to the log.

### quota-size
`--quota-size`  
This value can be used to set a known upper limit on the amount of space a backend has. If the backend reports the size itself, this value is ignored.

### repair-only-paths
`--repair-only-paths = false`  
Use this option to build a searchable local database which only contains path information. This option is usable for quickly building a database to locate certain content without needing to reconstruct all information. The resulting database can be searched, but cannot be used to restore data with.

### restore-path
`--restore-path`  
By default, files will be restored in the source folders, use this option to restore to another folder.

### restore-permissions
`--restore-permissions = false`  
By default permissions are not restored as they might prevent you from accessing your files. Use this option to restore the permissions as well.

### restore-symlink-metadata
`--restore-symlink-metadata = false`  
If symlink metadata is applied, it will usually mean changing the symlink target, instead of the symlink itself. For this reason, metadata is notapplied to symlinks, but this option can be used to override this, suchthat metadata is applied to symlinks as well.

### retention-policy
`--retention-policy`  
Use this option to reduce the number of versions that are kept with increasing version age by deleting most of the old backups. The expected format is a comma separated list of colon separated time frame and interval pairs. For example the value `7D:0s,3M:1D,10Y:2M` means "For 7 day keep all backups, for 3 months keep one backup per day and for 10 years one backup every 2nd month.

### retry-delay
`--retry-delay = 10s`  
After a failed transmission, Duplicati will wait a short period before attempting again. This is useful if the network drops out occasionally during transmissions.

### skip-file-hash-checks
`--skip-file-hash-checks = false`  
If the hash for the volume does not match, Duplicati will refuse to use the backup. Supply this flag to allow Duplicati to proceed anyway.

### skip-files-larger-than
`--skip-files-larger-than`  
This option allows you to exclude files that are larger than the given value. Use this to prevent backups becoming extremely large.

### skip-metadata
`--skip-metadata = false`  
Use this option to disable the storage of metadata, such as file timestamps. Disabling metadata storage will speed up the backup and restore operations, but does not affect file size much.

### skip-restore-verification
`--skip-restore-verification = false`  
After restoring files, the file hash of all restored files are checked to verify that the restore was successful.  
Use this option to disable the check and avoid waiting for the verification.

### small-file-max-count
`--small-file-max-count = 20`  
To avoid filling the remote storage with small files, this value can force grouping small files. The small volumes will always be combined when they can fill an entire volume.

### small-file-size
`--small-file-size`  
When examining the size of a volume in consideration for compacting, a small tolerance value is used, by default 20 percent of the volume size. This ensures that large volumes which may have a few bytes wasted space are not downloaded and rewritten.

### snapshot-policy
`--snapshot-policy = off`  
This setting controls the usage of snapshots, which allows Duplicati to backup files that are locked by other programs. If this is set to `off`, Duplicati will not attempt to create a disk snapshot. Setting this to `auto` makes Duplicati attempt to create a snapshot, and fail silently if that was not allowed or supported. A setting of `on` will also make Duplicati attempt to create a snapshot, but will produce a warning message in the log if it fails. Setting it to `required` will make Duplicati abort the backup if the snapshot creation fails. On Windows this uses the Volume Shadow Copy Services (VSS) and requires administrative privileges. On Linux this uses Logical Volume Management (LVM) and requires root privileges.

### store-metadata
`--store-metadata = true`  
Stores metadata, such as file timestamps and attributes. This increases the required storage space as well as the processing time.

### symlink-policy
`--symlink-policy = Store`  
Use this option to handle symlinks differently. The `store` option will simply record a symlink with its name and destination, and a restore will recreate the symlink as a link. Use the option `ignore` to ignore all symlinks and not store any information about them. Previous versions of Duplicati used the setting `follow`, which will cause symlinked files to be included and restore as normal files.

### synchronous-upload
`--synchronous-upload = false`  
Duplicati will upload files while scanning the disk and producing volumes, which usually makes the backup faster.  
Use this flag to turn the behavior off, so that Duplicati will wait for each volume to complete.

### tempdir
`--tempdir = C:\Users\User\AppData\Local\Temp\`  
Duplicati will use the system default temporary folder. This option can be used to supply an alternative folder for temporary storage. Note that SQLite will always put temporary files in the system default temporary folder.  
Consider using the `TMPDIR` environment variable on Linux to set the temporary folder for both Duplicati and SQLite.

### thread-priority
`--thread-priority = normal`  
Selects another thread priority for the process. Use this to set Duplicati to be more or less CPU intensive.

### threshold
`--threshold = 25`  
As files are changed, some data stored at the remote destination may not be required. This option controls how much wasted space the destination can contain before being reclaimed. This value is a percentage used on each volume and the total storage.

### throttle-download
`--throttle-download = 0kb`  
By setting this value you can limit how much bandwidth Duplicati consumes for downloads. Setting this limit can make the backups take longer, but will make Duplicati less intrusive.

### throttle-upload
`--throttle-upload = 0kb`  
By setting this value you can limit how much bandwidth Duplicati consumes for uploads. Setting this limit can make the backups take longer, but will make Duplicati less intrusive.

### time
`--time = now`  
By default, Duplicati will list and restore files from the most recent backup, use this option to select another item. You may use relative times, like "-2M" for a backup from two months ago.

### unittest-mode
`--unittest-mode = false`  
When running in unittest mode, no automatic fixes are applied, which assumes that the input data is always in perfect shape. This option is not intended for use in daily backups, but required for testing purposes to reveal potential problems.

### upload-unchanged-backups
`--upload-unchanged-backups = false`  
If no files have changed, Duplicati will not upload a backup set. If the backup data is used to verify that a backup was executed, this option will make Duplicati upload a backupset even if it is empty.

### upload-verification-file
`--upload-verification-file = false`  
Use this option to upload a verification file after changing the remote storage. The file is not encrypted and contains the size and SHA256 hashes of all the remote files and can be used to verify the integrity of the files.

###   use-background-io-priority
`--use-background-io-priority = false`  
This option instructions the operating system to set the current process to use the lowest IO priority level, which can make operations run slower but will interfere less with other operations running at the same time.

### use-block-cache
`--use-block-cache = false`  
Store an in-memory copy of the block table. This potentially reduces lookup time during backup as it limits the number of times the database must be accessed when examining blocks.

### usn-policy
`--usn-policy = off`  
This setting controls the usage of NTFS USN numbers, which allows Duplicati to obtain a list of files and folders much faster. If this is set to `off`, Duplicati will not attempt to use USN. Setting this to `auto` makes Duplicati attempt to use USN, and fail silently if that was not allowed or supported. A setting of `on` will also make Duplicati attempt to use USN, but will produce a warning message in the log if it fails. Setting it to `required` will make Duplicati abort the backup if the USN usage fails. This feature is only supported on Windows and requires administrative privileges.

### verbose
`--verbose = false`  
Use this option to increase the amount of output generated when running an option. Generally this option will produce a line for each file processed.

### version
`--version`  
By default, Duplicati will list and restore files from the most recent backup, use this option to select another item. You may enter multiple values separated with comma, and ranges using `-`, e.g. `0,2-4,7`.

### vss-exclude-writers
`--vss-exclude-writers`  
Use this option to exclude faulty writers from a snapshot. This is equivalent to the -wx flag of the vshadow.exe tool, except that it only accepts writer class GUIDs, and not component names or instance GUIDs. Multiple GUIDs must be separated with a semicolon, and most forms of GUIDs are allowed, including with and without curly braces.

### vss-use-mapping
`--vss-use-mapping = false`  
Activate this option to map VSS snapshots to a drive (similar to SUBST, using Win32 DefineDosDevice). This will create temporary drives that are then used to access the contents of a snapshot. This workaround can speed up file access on Windows XP.



## HTTP options

With these options can be used to change the way http requests are issued. The module that provide these options is loaded automatically, use `--disable-module` to prevent this.

### disable-expect100-continue
`--disable-expect100-continue (Boolean)`  
Disable the expect header.  
The default HTTP request has the header "Expect: 100-Continue" attached, which allows some optimizations when authenticating, but also breaks some web servers, causing them to report "417 - Expectation failed".  
Default value: `false`

### disable-nagling
`--disable-nagling (Boolean)`  
Disable nagling.  
By default the http requests use the RFC 896 nagling algorithm to support transfer of small packages more efficiently.  
Default value: `false`

### accept-specified-ssl-hash
`--accept-specified-ssl-hash (String)`  
Optionally accept a known SSL certificate.  
If your server certificate is reported as invalid (eg. with self-signed certificates), you can supply the certificate hash (SHA1) to approve it anyway. The hash value must be entered in hex format without spaces or colons. You can enter multiple hashes separated by commas.

### accept-any-ssl-certificate
`--accept-any-ssl-certificate (Boolean)`  
Accept any server certificate.  
Use this option to accept any server certificate, regardless of what errors it may have. Please use `--accept-specified-ssl-hash` instead, whenever possible.

### oauth-url
`--oauth-url (String)`  
Alternate OAuth URL.  
Duplicati uses an external server to support the OAuth authentication flow. If you have set up your own Duplicati OAuth server, you can supply the refresh url.  
Default value: `https://duplicati-oauth-handler.appspot.com/refresh`

### allowed-ssl-versions
`--allowed-ssl-versions (Flags)`  
Sets allowed SSL versions.  
This option changes the default SSL versions allowed. This is an advanced option and should only be used if you want to enhance security or work around an issue with a particular SSL protocol.  
Values: `Ssl3`, `Tls`, `Tls11`, `Tls12`, `SystemDefault`  
Default value: `SystemDefault,Ssl3,Tls`

### http-operation-timeout
`--http-operation-timeout (Timespan)`  
Sets the default operation timeout.  
This option changes the default timeout for any HTTP request, the time covers the entire operation from initial packet to shutdown.

### http-readwrite-timeout
`--http-readwrite-timeout (Timespan)`  
Sets readwrite.  
This option changes the default read-write timeout. Read-write timeouts are used to detect a stalled requests, and this option configures the maximum time between activity on a connection.

### http-enable-buffering
`--http-enable-buffering (Boolean)`  
Sets HTTP buffering.  
This option sets the HTTP buffering. Setting this to "true" can cause memory leaks, but can also improve performance in some cases.  
Default value: `false`



## Scripting options

With these options you can executes a script before starting an operation, and again on completion. Module is loaded automatically, use `--disable-module` to prevent this.

### run-script-before
`--run-script-before (Path)`  
Run a script on startup.  
Executes a script before performing an operation. The operation will block until the script has completed or timed out.

### run-script-after
`--run-script-after (Path)`  
Run a script on exit.  
Executes a script after performing an operation. The script will receive the operation results written to stdout.

### run-script-before-required
`--run-script-before-required (Path)`  
Run a required script on startup.  
Executes a script before performing an operation. The operation will block until the script has completed or timed out. If the script returns a non-zero error code or times out, the operation will be aborted.

### run-script-timeout
`--run-script-timeout (Timespan)`  
Sets the script timeout.  
Sets the maximum time a script is allowed to execute. If the script has not completed within this time, it will continue to execute but the operation will continue too, and no script output will be processed.  
Default value: `60s`

## Reporting options

These options provide support for sending status reports via HTTP messages. Modules sendhttp, sendmail and sendxxmp are loaded automatically, use `--disable-module` to prevent this.

### send-http-url
 `--send-http-url (String)`  
HTTP report url.

### send-http-message
`--send-http-message (String)`  
The message template.  
This value can be a filename. If the file exists, the file contents will be used as the message. In the message, certain tokens are replaced:

* `%OPERATIONNAME%`  
The name of the operation, normally `Backup`.
* `%REMOTEURL%`  
Remote server url.
* `%LOCALPATH%`  
The path to the local files or folders involved in the operation (if any).
* `%PARSEDRESULT%`  
* The parsed result, if the operation is a backup. Possible values are: `Error`, `Warning`, `Success`

All command line options are also reported within `%value%`, e.g. `%volsize%`. Any unknown/unset value is removed.  
Default value: `Duplicati %OPERATIONNAME% report for %backup-name%`  
`%RESULT%`

### send-http-message-parameter-name
`--send-http-message-parameter-name (String)`  
The name of the parameter to send the message as.  
Default value: `message`

### send-http-extra-parameters
`--send-http-extra-parameters (String)`  
Extra parameters to add to the http message. I.e. `parameter1=value1&parameter2=value2`.

### send-http-level
`--send-http-level (Enumeration)`  
The messages to send.  
You can specify one of `Success`, `Warning`, `Error`, `Fatal`. You can supply multiple options with a comma separator, e.g. `Success,Warning`. The special value `All is a shorthand for `Success,Warning,Error,Fatal` and will cause all backup operations to send a message.  
Values: `Unknown`, `Success`, `Warning`, `Error`, `Fatal`, `All`  
Default value: `all`

### send-http-any-operation
`--send-http-any-operation (Boolean)`  
Send messages for all operations.  
By default, messages will only be sent after a Backup operation. Use this option to send messages for all operations.

### send-mail-to
`--send-mail-to (String)`  
Email recipient(s).  
This setting is required if mail should be sent, all other settings have default values. You can supply multiple email addresses separated with commas, and you can use the normal address format as specified by RFC2822 section 3.4.  
Example with 3 recipients: `Peter Sample <peter@example.com>, John Sample <john@example.com>, admin@example.com`

### send-mail-from
`--send-mail-from (String)`  
Email sender.  
Address of the email sender. If no host is supplied, the hostname of the first recipient is used. Examples of allowed formats:

* `sender`
* `sender@example.com`
* `Mail Sender <sender>`
* `Mail Sender <sender@example.com>`

Default value: `no-reply`

### send-mail-subject
`--send-mail-subject (String)`  
The email subject.  
This setting supplies the email subject. Values are replaced as described in the description for `--send-mail-body`.  
Default value: `Duplicati %OPERATIONNAME% report for %backup-name%`

### send-mail-body
`--send-mail-body (String)`  
The message body.  
This value can be a filename. If the file exists, the file contents will be used as the message body.  
In the message body, certain tokens are replaced:

* `%OPERATIONNAME%`  
The name of the operation, normally `Backup`.
* `%REMOTEURL%`  
Remote server url.
* `%LOCALPATH%`  
The path to the local files or folders involved in the operation (if any).
* `%PARSEDRESULT%`  
The parsed result, if the operation is a backup. Possible values are: `Error`, `Warning`, `Success`.

All command line options are also reported within `%value%`, e.g.`%volsize%`. Any unknown/unset value is removed.  
Default value: `%RESULT%`

### send-mail-url
`--send-mail-url (String)`  
SMTP Url.  
A url for the SMTP server, e.g. `smtp://example.com:25`. Multiple servers can be supplied in a prioritized list, separated with semicolon. If a server fails, the next server in the list is tried, until the message has been sent.  
If no server is supplied, a DNS lookup is performed to find the first recipient's MX record, and all SMTP servers are tried in their priority order until the message is sent.  
To enable SMTP over SSL, use the format `smtps://example.com`. To enable SMTP STARTTLS, use the format `smtp://example.com:25/?starttls=when-available` or `smtp://example.com:25/?starttls=always`. If no port is specified, port 25 is used for non-ssl, and 465 for SSL connections. To force not to use STARTTLS use `smtp://example.com:25/?starttls=never`.

### send-mail-username
`--send-mail-username (String)`  
SMTP Username.  
The username used to authenticate with the SMTP server if required.

### send-mail-password
`--send-mail-password (String)` 
SMTP Password.  
The password used to authenticate with the SMTP server if required.

### send-mail-level
`--send-mail-level (String)`  
The messages to send.  
You can specify one of `Success`, `Warning`, `Error`, `Fatal`. You can supply multiple options with a comma separator, e.g. `Success,Warning`. The special value `All` is a shorthand for `Success,Warning,Error,Fatal` and will cause all backup operations to send an email.  
Values: `Unknown`, `Success`, `Warning`, `Error`, `Fatal`, `All`  
Default value: `all`

### send-mail-any-operation
`--send-mail-any-operation (Boolean)`  
Send email for all operations.  
By default, mail will only be sent after a Backup operation. Use this option to send mail for all operations.

### send-xmpp-to
`--send-xmpp-to (String)`  
XMPP recipient email.  
The users who should have the messages sent, specify multiple users separated with commas.

### send-xmpp-message
`--send-xmpp-message (String)`  
The message template.  
This value can be a filename. If the file exists, the file contents will be used as the message.  
In the message, certain tokens are replaced:

* `%OPERATIONNAME%`  
The name of the operation, normally `Backup`.
* `%REMOTEURL%`  
Remote server url.
* `%LOCALPATH%`  
The path to the local files or folders involved in the operation (if any).
* `%PARSEDRESULT%`  
The parsed result, if the operation is a backup. Possible values are: `Error`, `Warning`, `Success`.

All command line options are also reported within `%value%`, e.g. `%volsize%`. Any unknown/unset value is removed.  
Default value: `Duplicati %OPERATIONNAME% report for %backup-name%`  
`%RESULT%`

### send-xmpp-username
`--send-xmpp-username (String)`  
The XMPP username.  
The username for the account that will send the message, including the hostname. I.e. `account@jabber.org/Home`.

### send-xmpp-password
`--send-xmpp-password (String)`  
The XMPP password.  
The password for the account that will send the message.

### send-xmpp-level
`--send-xmpp-level (Enumeration)`  
The messages to send.  
You can specify one of `Success`, `Warning`, `Error`, `Fatal`.  
You can supply multiple options with a comma separator, e.g. `Success,Warning`. The special value `All` is a shorthand for `Success,Warning,Error,Fatal` and will cause all backup operations to send a message.  
Values: `Unknown`, `Success`, `Warning`, `Error`, `Fatal`, `All`  
Default value: `all`

### send-xmpp-any-operation
`--send-xmpp-any-operation (Boolean)<`  
Send messages for all operations.  
By default, messages will only be sent after a Backup operation. Use this option to send messages for all operations.


