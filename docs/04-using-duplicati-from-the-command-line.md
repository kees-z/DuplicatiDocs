

## Introduction to the Duplicati Command Line tool

The integrated webserver in Duplicati offers a convenient way to schedule and run backup jobs. However, if you can't or don't want to use the graphical user interface and/or the built-in scheduler, you can use the Duplicati Commandline tool. The filename is `Duplicati.CommandLine.exe`, the tool can be found in the Duplicati program folder.

With the Commandline tool you can perform all operations that are available in the Graphical User Interface. You can even do more with the Commandline tool. If you want to perform specific operations, like deleting a particular backup or comparing 2 backups, you will not be able to do this in the Graphical User Interface, but the Commandline tool supports these operations.

## How to use the Duplicati Command Line tool

The Commandline tool can be used by typing `Duplicati.CommandLine.exe` followed by a number of arguments in a command prompt window. Linux and Mac OS X users should type `mono Duplicati.CommandLine.exe` or `duplicati-cli`, which is a wrapper for running `mono Duplicati.CommandLine.exe`.

Which arguments you need to specify depend on the command you run with the Commandline tool. Generally, these arguments need to be supplied:

* **The command to execute**  
This tells the Commandline tool what to do. Supported commands will be described one by one. The Commandline tool supports these commands:  
`backup`, `find`, `restore`, `delete`, `compact`, `test`, `compare`, `purge`, `vacuum`, `repair`, `affected`, `list-broken-files`, `purge-broken-files`
* **Target URL**  
If the command needs access to the files at the backend, you need to specify the protocol, URL and credentials as the first argument.  
Example: to access host.myftpserver.com/backup with username User and password Pass, using the FTP protocol, the target URL will be:  
`ftp://User:Pass@host.myftpserver.com`  
Each storage provider has its own set of required and optional parameters. See [Storage Providers](../05-storage-providers) for more info about specific backends.
* **Command arguments**  
Some commands need additional information. For example, if you want to compare 2 backups, you have to specify which 2 backups from the available list Duplicati should compare.
* **Advanced options**  
Duplicati offers a wide range of advanced options. With advanced options you give Duplicati additional information, like the location of the local database, where to store temporary files or information to fine-tune the command you want to execute. There are general advanced options and advanced options for specific storage providers. See< [Storage Providers](../05-storage-providers) and [Advanced options](../06-advanced-options) for more information.

Generally, each operation from the command line has the following format:

**For Windows:**  
`Duplicati.CommandLine.exe <command> [storage-URL] [arguments] [advanced-options]`

**For Linux and Mac OS X:**  
`duplicati-cli <command> [storage-URL] [arguments] [advanced-options]`

Storage-URL, arguments and advanced-options may or may not be mandatory, depending on the command you execute.

*****
> ![](icon_info.png) From this point, only Duplicati.CommandLine.exe will be used as reference to the Commandline tool. Linux and Mac OS X users should replace this with duplicati-cli or mono Duplicati.CommandLine.exe.

*****


## Getting help from the Command Line Tools

The Commandline tool provides online help with the special `help` command. To get started, type `Duplicati.CommandLine.exe help`.

![](ss_gethelp_01.png)

To get help about a specific topic, add it to the help command. So if you need help about the `find` command, type `Duplicati.CommandLine.exe help find`, which will return the following result:

![](ss_gethelp_02.png)

*****
> ![](icon_info.png) The special help topic `example` will show a few examples of how the Commandline tool should be used to perform some simple operations.  
To show the examples, type `Duplicati.CommandLine.exe help example`.

*****


## The BACKUP command

This is probably the most important command, after all Duplicati is a backup program. You can run a backup with the backup command using the following format:

`Duplicati.CommandLine.exe backup <storage-URL> "<source-path>" [<options>]`

The storage-URL should be specified in this format:

`protocol://username:password@hostname:port/path?backend_option1=value1&backend_option2=value2`

Multiple source paths can be specified if they are separated by a space.

`username` must not contain `:` and `password` must not contain `@`. If they do, specify username and password using `--auth-username` and `--auth-password`, or url-encode them.

Add as many advanced options as needed, like `--passphrase` and `--dblock-size`.

*****
> ![](icon_info.png) Instead of composing the complete backup command yourself, including all advanced options, you can create a backup job in the Graphical User Interface, without scheduling it. Once completed, you can export the backup job to the command line, resulting in a Duplicati.CommandLine.exe backup command with all settings that you specified in the wizard. You can paste this generated command in your favorite task scheduler. For more information about creating a backup job in the Graphical User Interface, see [Creating a new backup job](../03-using-the-graphical-user-interface/#creating-a-new-backup-job). For more information about exporting the backup job to the command line, see [Exporting a backup job configuration](../03-using-the-graphical-user-interface/#exporting-a-backup-job-configuration).

*****

The Commandline equivalent of the backup job described in [Creating a new backup job](../03-using-the-graphical-user-interface/#creating-a-new-backup-job) is:

```nohighlight
Duplicati.CommandLine.exe backup "ftp://myftpserver.com/Backup/Pictures?auth-username=Duplicati&auth-password=backup" "C:\Users\User\Pictures" --backup-name="Pictures Collection" --dbpath="C:\Users\User\DuplicatiCanary\data\LFYXSFKFFN.sqlite" --encryption-module="aes" --compression-module="zip" --dblock-size="50mb" --keep-time="3M" --passphrase="%@/%78kUPKlZtz" --skip-files-larger-than="2GB" --default-filters="Windows" --exclude-files-attributes="temporary" --disable-module="console-password-input" --exclude="desktop.ini"
```

## The RESTORE command

The `restore` command can restore files from a specific restore point to the local system. Use the following format:

`Duplicati.CommandLine.exe restore <storage-URL> ["<filename>"] [<options>]`

This will restore `<filename>` to its original location. If the specified filename exists already, a timestamp will be added to the filename. If you want to restore all files, use "*" or leave the filename empty.

Some advanced options frequently used with restore operations are:

* `--overwrite=<boolean>`  
Overwrites existing files.
* `--restore-path=<string>`  
Restores files to <restore-path> instead of their original destination. Top folders are removed if possible.
* `--time=<time>`  
Restore files that are older than the specified time.
* `--version=<int>`  
Restore files from a specific backup.

## The FIND command

This command is used to check what's in your backup. It can show a list of all backups, or list occurrences of a specific file (or files when using wildcards or a regular expression).

Usage format for the `find` command is:

`Duplicati.CommandLine.exe find <storage-URL> ["<filename>"] [<options>]`

If `<filename>` is specified, all occurrences of `<filename>` in the backup are listed. `<filename>` can contain `*` and `?` as wildcards. File names in `[`brackets`]` are interpreted as regular expression. Latest backup is searched by default. If entire path is specified, all available versions of the file are listed. If no `<filename>` is specified, a list of all available backups is shown.

Useful advanced options are:

* `--time=<time>`  
Shows what the files looked like at a specific time. Absolute and relative times can be specified.
* `--version=<int>`  
Shows what the files looked like in a specific backup. If no version is specified, the latest backup (version=0) will be used. If nothing is found, older backups will be searched automatically.
* `--include=<string>`  
Reduces the list of files in a backup to those that match the provided string. This is applied before the search is executed.
* `--exclude=<string>`  
Removes matching files from the list of files in a backup. This is applied before the search is executed.
* `--all-versions=<boolean>`  
Searches in all backup sets, instead of just searching the latest.

## The COMPARE command

This is a useful command that shows the differences between two backup versions. The versions do not need to be subsequent, you can compare any two backup versions. If no versions are given, changes are shown between the two latest backups. The versions can either be timestamps or backup version numbers. If only one version is given, the most recent backup is compared to that version. Usage:

`Duplicati.CommandLine.exe compare <storage-URL> [<base-version>] [<compare-to>] [<options>]`

Some advanced options that can be used with the `compare` command:

* `--verbose`  
Shows names of files
* `--include=<filter>`  
Adds an include filter (for verbose output)
* `--exclude=<filter>`  
Adds an exclude filter (for verbose output)

## The TEST command

After a backup operation, some backup files are verified by downloading them and check if the contents of these files is what Duplicati expects. This automatic verification after each backup operation can be disabled. To be able to check the integrity of the backup files, use the `test` command. Usage:

`Duplicati.CommandLine.exe test <storage-URL> <samples> [<options>]`

Verifies integrity of a backup. A random sample of dlist, dindex, dblock files is downloaded, decrypted and the content is checked against recorded size values and data hashes. `<samples>` specifies the number of samples to be tested. If "all" is specified, all files in the backup will be tested. This is a rolling check, i.e. when executed another time different samples are verified than in the first run. A sample consists of 1 dlist, 1 dindex, 1 dblock.

Suggested advanced options:

* `--time=<time>`  
Checks samples from a specific time.
* `--version=<int>`  
Checks samples from specific versions. Delimiters are `,` and `-`.
* `--full-remote-verification`  
Checks the internal structure of each file instead of just verifying the file hash.

## The COMPACT command

Old data is not deleted immediately as in most cases only small parts of a dblock file are old data. When the amount of old data in a dblock file grows it might be worth to replace it. This is especially the case when the number of dblock files and thus the required storage space can be reduced. When backups are frequently made and only few files have changed, the uploaded dblock files are small. At some point it might make sense to replace a large number of small files with one large file. This is what compacting does. Usage:

`Duplicati.CommandLine.exe compact <storage-URL> [<options>]`

A few advanced options to use with the `compact` command:

* `--small-file-max-count=<int>`  
The maximum allowed number of small files.
* `--small-file-size=<int>`  
Files smaller than this size are considered to be small and will be compacted with other small files as soon as there are `<small-file-max-count>` of them. `--small-file-size=20` means 20% of `<dblock-size>`.
* `--threshold=<percent_value>`  
The amount of old data that a dblock file can contain before it is considered to be replaced.

## The DELETE command

Marks old data deleted and removes outdated dlist files. A backup is deleted when it is older than `<keep-time>` or when there are more newer versions than `<keep-versions>`. Data is considered old, when it is not required from any existing backup anymore. Usage:

`Duplicati.CommandLine.exe delete <storage-URL> [<options>]`

Some advanced options:

* `--dry-run`  
Performs the operation, but does not write changes to the local database or the remote storage.
* `--keep-time=<time>`  
Marks data outdated that is older than `<time>`.
* `--keep-versions=<int>`  
Marks data outdated that is older than <int> versions.
*  `--version=<int>`  
Deletes all files that belong to the specified version(s).
* `--allow-full-removal`  
Disables the protection against removing the final fileset.

## The PURGE command

Purges (removes) files from remote backup data. This command can either take a list of filenames or use the filters to choose which files to purge. The purge process creates new filesets on the remote destination with the purged files removed, and will start the compacting process after a purge. By default, the matching files are purged in all versions, but this can be limited by choosing one or more versions. To test what will happen, use the `--dry-run` flag. Usage:

`Duplicati.CommandLine.exe purge <storage-URL> <filenames> [<options>]`

Useful advanced options:

* `--dry-run`  
Performs the operation, but does not write changes to the local database or the remote storage.
* `--version=<int>`  
Selects specific versions to purge from, multiple versions can be specified with commas.
* `--time=<time>`  
Selects a specific version to purge from.
* `--no-auto-compact`  
Performs a compact process after purging files
* `--include=<filter>`  
Selects files to purge, using filter syntax

## The REPAIR command

Tries to repair the backup. If no local database is found or the database is empty, the database is re-created with data from the storage. If the database is in place but the remote storage is corrupt, the remote storage gets repaired with local data (if available). Usage: 

`Duplicati.CommandLine.exe repair <storage-URL> [<options>]`

Example for B2 storage:
`Duplicati.CommandLine.exe repair "b2://blacksmith/photos?auth-username=1234567890&auth-password=123ABc456dEfGH" --dbpath="C:\Users\User1\AppData\Local\Duplicati\SONSONSHE.sqlite" --passphrase="1234567890ABCdef"`

## The AFFECTED command

Returns a report explaining what backup sets and files are affected by a remote file. You can use this option to see what source files are affected if one or more remote files are damaged or deleted. The advanced option `dbpath` is required to specify the location of the local database. Usage:

`Duplicati.CommandLine.exe affected <storage-URL> <remote-filename> [<remote-filenames>] [<options>]`

## The LIST-BROKEN-FILES command

Checks the database for missing data that cause files not not be restoreable. Files can become unrestoreable if remote data files are defect or missing. Use the list-broken-files command to see what the purge-broken-files command will remove. Usage:

`Duplicati.CommandLine.exe list-broken-files <storage-URL> [<options>]`

## The PURGE-BROKEN-FILES command

Removes all files from the database and remote storage that are no longer restoreable. Use this operation with caution, and only if you cannot recover the missing remote files, but want to continue a backup. Even with missing remote files, it may be possible to restore parts of the files that will be removed with this command. Usage:

`Duplicati.CommandLine.exe purge-broken-files <storage-URL> [<options>]`

Recommended advanced option before actually purge the files:

* `--dry-run`  
Performs the operation, but does not write changes to the local database or the remote storage.

## The CREATE-REPORT command

Analyses the backup and prepares a report with anonymous information. This report can be sent to the developers for a better analysis in case something went wrong. Usage:

`Duplicati.CommandLine.exe create-report <storage-URL> <output-file> [<options>]`

## The TEST-FILTERS command

Scans the source files and tests against the filters specified, the console output shows which files and folders are examined and the result. Usgae:

`Duplicati.CommandLine.exe test-filters <source-path> [<options>]`

## The SYSTEM-INFO command

Issue this following command to see a variety of system information relevant to Duplicati. Usage:

`Duplicati.CommandLine.exe system-info`

## The SEND-MAIL command

Duplicati can send email notifications after each operation. Use the send-mail command to test this. Usage:

`Duplicati.CommandLine.exe send-mail --send-mail-to=<email-address> [<options>]`

Advanced options you can use with this command:

* `--send-mail-to=<email-address>`  
Send an email to <email-address> after a backup. Valid formats are `Name <test@example.com>`, `Other <test2@example.com>`, `test3@example.com`. Multiple addresses must be separated with a comma.
* `--send-mail-from=<email-address>`  
This is the sender address of the email that is sent.
* `--send-mail-subject=<subject>`  
This is the subject line of the email that is sent. E.g. this can be `"Duplicati %OPERATIONNAME% Report"`
* `--send-mail-body=<body>`  
The content of the email message. This should contain `"%RESULT%"`.
* `--send-mail-url=<smtp-url>`  
A URL to connect to an SMTP server to send out an email. Example: `"tls://smtp.example.com:587"`, `"smtps://smtp.example.com:465"` or `"smtp://smtp.example.com:25"`.
* `--send-mail-username=<username>`  
Required username to authenticate with SMTP server.
* `--send-mail-password=<password>`  
Required password to authenticate with SMTP server.
* `--send-mail-level=<level>`  
When email messages are sent: `Success`, `Warning`, `Error` are possible.
*  `--send-mail-any-operation=true`  
Also send emails after other operations like restore etc.

Allowed placeholders are:
* `%PARSEDRESULT%`  
The parsed result op the operation: `Success`, `Warning`, `Error`
* `%RESULT%`  
When used in the body, this is the result/log of the backup,  
When used in the subject line, this is the same as `%PARSEDRESULT%`
* `%OPERATIONNAME%`  
The name of the operation, usually `backup`, but could also be `restore` etc.
* `%REMOTEURL%`  
The backend url.
* `%LOCALPATH%`  
The path to the local folders involved (i.e. the folders being backed up).
