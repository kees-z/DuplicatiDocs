

Duplicati supports many storage providers to use as backend for your backups. Both standard protocols and a wide range of proprietary cloud storage solutions are supported. Each storage provider has its own set of options that you can specify. Some options are mandatory, other options are optional. Use this list of providers as a reference to compose a valid command for communication with the storage provider of your choice.

## Local folder or drive

Duplicati can use the local file system to store backups. The following target URL formats can be used:

`file://hostname/folder%20for%20backup`  
`file://\\server\folder%20for%20backup (UNC path)`  
`"C:\folder for backup"`  
`file://c:\folder%20for%20backup (Windows)`  
`file:///usr/pub/folder%20for%20backup (Linux)`  

Options:

* `--auth-password`  
The password used to connect to the server. This may also be supplied as the environment variable `AUTH_PASSWORD`.
* `--auth-username`  
The username used to connect to the server. This may also be supplied as the environment variable `AUTH_USERNAME`.
* `--alternate-destination-marker`  
This option only works when the `--alternate-target-paths` option is also specified. If there are alternate paths specified, this option indicates the name of a marker file that must be present in the folder. This can be used to handle situations where an external drive changes drive letter or mount point. By ensuring that a certain file exists, it is possible to prevent writing data to an unwanted external drive. The contents of the file are never examined, only file existence.
* `--alternate-target-paths`  
This option allows multiple targets to be specified. The primary target path is placed before the list of paths supplied with this option. Before starting the backup, each folder in the list is checked for existence and optionally the presence of the marker file supplied by `--alternate-destination-marker`. The first existing path that optionally contains the marker file is then used as the destination. Multiple destinations are separated with a "`;`". On Windows, the path may be a UNC path, and the drive letter may be substituted with an asterisk (`*`), eg.: " *:\backup ", which will examine all drive letters. If a username and password is supplied, the same credentials are used for all destinations.
* `--use-move-for-put`  
When storing the file, the standard operation is to copy the file and delete the original. This sequence ensures that the operation can be retried if something goes wrong. Activating this option may cause the retry operation to fail. This option has no effect unless the `--disable-streaming-transfers` options is activated.
* `--force-smb-authentication`  
If this option is set, any existing authentication against the remote share is dropped before attempting to authenticate.

## FTP

Duplicati can use FTP servers to store backups. The following target URL formats can be used:

`ftp://hostname/folder`

Options:

* `--ftp-passive = false`  
If this flag is set, the FTP connection is made in passive mode, which works better with some firewalls. If the `ftp-regular` flag is also set, this flag is ignored.
* `--ftp-regular = true`  
If this flag is set, the FTP connection is made in active mode. Even if the `ftp-passive` flag is also set, the connection will be made in active mode.
* `--auth-password`  
The password used to connect to the server. This may also be supplied as the environment variable `AUTH_PASSWORD`.
* `--auth-username`  
The username used to connect to the server. This may also be supplied as the environment variable `AUTH_USERNAME`.
* `--use-ssl`  
Use this flag to communicate using Secure Socket Layer (SSL) over ftp (ftps).
* `--disable-upload-verify`  
To protect against network failures, every upload will be attempted verified. Use this option to disable this verification to make the upload faster but less reliable.

## FTP (Alternative)

This backend can read and write data to an FTP based backend using an alternative FTP client. Allowed formats are

`aftp://hostname/folder`

or

`aftp://username:password@hostname/folder`

upported options:

* `--auth-password (Password)`  
Supplies the password used to connect to the server  
The password used to connect to the server. This may also be supplied as the environment variable `AUTH_PASSWORD`.
* `--auth-username (String)`  
Supplies the username used to connect to the server  
The username used to connect to the server. This may also be supplied as the environment variable `AUTH_USERNAME`.
* `--disable-upload-verify (Boolean)`  
Disable upload verification.  
To protect against network or server failures, every upload will be attempted to be verified. Use this option to disable this verification to make the upload faster but less reliable.
* `--aftp-data-connection-type (Enumeration)`  
Configure the FTP data connection type.  
If this flag is set, the FTP data connection type will be changed to the selected option.  
Values: `AutoPassive`, `PASV`, `PASVEX`,`EPSV`, `AutoActive`, `PORT`,`EPRT`  
Default value: `AutoPassive`
* `--aftp-encryption-mode (Enumeration)`  
Configure the FTP encryption mode.  
If this flag is set, the FTP encryption mode will be changed to the selected option.  
Values: `None`, `Implicit`, `Explicit`  
Default value: `None`
* `--aftp-ssl-protocols (Flags)`  
Configure the SSL policy to use when encryption is enabled.  
This flag controls the SSL policy to use when encryption is enabled.  
Values: `None`, `Ssl2`, `Ssl3`, `Tls`, `Default`, `Tls11`, `Tls12`  
Default value: `Default`

## OpenStack Object Storage / Swift

This backend can read and write data to Swift (OpenStack Object Storage). Supported format is

`openstack://container/folder`

Supported options:

* `--auth-username (String)`  
Supplies the username used to connect to the server.  
The username used to connect to the server. This may also be supplied as the environment variable `AUTH_USERNAME`.
* `--auth-password (Password)`  
Supplies the password used to connect to the server.  
The password used to connect to the server. This may also be supplied as the environment variable `AUTH_PASSWORD`. If the password is supplied,`--openstack-tenant-name` must also be set.
* `--openstack-tenant-name (String)`  
Supplies the Tenant Name used to connect to the server.  
The Tenant Name is commonly the paying user account name. This option must be supplied when authenticating with a password, but is not required when using an API key.
* `--openstack-apikey (Password)`  
Supplies the API key used to connect to the server.  
The API key can be used to connect without supplying a password and tenant ID with some providers.
* `--openstack-authuri (String)`  
Supplies the authentication URL.  
The authentication URL is used to authenticate the user and find the storage service. The URL commonly ends with "/v2.0". Known providers are:  
    * Rackspace US: [https://identity.api.rackspacecloud.com/v2.0](https://identity.api.rackspacecloud.com/v2.0)
    * Rackspace UK: [https://lon.identity.api.rackspacecloud.com/v2.0</span></span>](https://lon.identity.api.rackspacecloud.com/v2.0)
    * OVH Cloud Storage: [https://auth.cloud.ovh.net/v2.0](https://auth.cloud.ovh.net/v2.0)
    * Selectel Cloud Storage: [https://auth.selcdn.ru</span></span>](https://auth.selcdn.ru)
* `--openstack-region (String)`  
Supplies the region used for creating a container.  
This option is only used when creating a container, and is used to indicate where the container should be placed.  
Consult your provider for a list of valid regions, or leave empty for the default region.

## S3 Compatible

Duplicati can use S3-compatible servers to store backups. The following target URL format is used:

`s3://bucketname/prefix`

Options:

* `--aws_secret_access_key`  
The AWS "Secret Access Key" can be obtained after logging into your AWS account, this can also be supplied through the `auth-password` property.
* `--aws_access_key_id`  
The AWS "Access Key ID" can be obtained after logging into your AWS account, this can also be supplied through the `auth-username` property.
* `--s3-european-buckets = false`  
This flag is only used when creating new buckets. If the flag is set, the bucket is created on a European server.  
This flag forces the `s3-use-new-style` flag. Amazon charges slightly more for European buckets.
* `--s3-use-rrs = false`  
This flag toggles the use of the special RRS header. Files stored using RRS are more likely to disappear than those stored normally, but also costs less to store. See the full description here:  
[http://aws.amazon.com/about-aws/whats-new/2010/05/19/announcing-amazon-s3-reduced-redundancy-storage/](http://aws.amazon.com/about-aws/whats-new/2010/05/19/announcing-amazon-s3-reduced-redundancy-storage/)
* `--s3-storage-class`  
Use this option to specify a storage class. If this option is not used, the server will choose a default storage class.
* `--s3-server-name = s3.amazonaws.com`  
 Companies other than Amazon are now supporting the S3 API, meaning that this backend can read and write data to those providers as well. Use this option to set the hostname. Currently known providers are:
    * Amazon S3: s3.amazonaws.com
    * Hosteurope: cs.hosteurope.de
    * Dunkel: dcs.dunkel.de
    * DreamHost: objects.dreamhost.com
    * dinCloud - Chicago: d3-ord.dincloud.com
    * dinCloud - Los Angeles: d3-lax.dincloud.com
    * IBM COS (S3) Public US: s3-api.us-geo.objectstorage.softlayer.net
    * Wasabi Hot Storage: s3.wasabisys.com<
* `--s3-location-constraint`  
This option is only used when creating new buckets. Use this option to change what region the data is stored in.  
Amazon charges slightly more for non-US buckets. Known bucket locations:
    * (default):
    * Europe (EU): EU
    * Europe (EU, Frankfurt): eu-central-1
    * Europe (EU, Ireland): eu-west-1
    * Europe (EU, London): eu-west-2
    * US East (Northern Virginia): us-east-1
    * US East (Ohio): us-east-2
    * US West (Northern California): us-west-1
    * US West (Oregon): us-west-2
    * Canada (Central): ca-central-1
    * Asia Pacific (Mumbai): ap-south-1
    * Asia Pacific (Singapore): ap-southeast-1
    * Asia Pacific (Sydney): ap-southeast-2
    * Asia Pacific (Tokyo): ap-northeast-1
    * Asia Pacific (Seoul): ap-northeast-2
    * South America (São Paulo): sa-east-1
* `--use-ssl`  
Use this flag to communicate using Secure Socket Layer (SSL) over http (https). Note that bucket names containing a period has problems with SSL connections.
* `--auth-password`  
The password used to connect to the server. This may also be supplied as the environment variable `AUTH_PASSWORD`.
* `--auth-username`  
The username used to connect to the server. This may also be supplied as the environment variable `AUTH_USERNAME`.
* `--s3-ext-forcepathstyle = False`  
Extended option ForcePathStyle
* `--s3-ext-useaccelerateendpoint = False`  
Extended option UseAccelerateEndpoint
* `--s3-ext-signaturemethod = HmacSHA256`  
Extended option SignatureMethod
* `--s3-ext-signatureversion = 4`  
Extended option SignatureVersion
* `--s3-ext-serviceurl`  
Extended option ServiceURL
* `--s3-ext-usehttp = False`  
Extended option UseHttp
* `--s3-ext-authenticationregion`  
Extended option AuthenticationRegion
* `--s3-ext-authenticationservicename = s3`  
Extended option AuthenticationServiceName
* `--s3-ext-maxerrorretry = 4`  
Extended option MaxErrorRetry
* `--s3-ext-logresponse = False`  
Extended option LogResponse
* `--s3-ext-readentireresponse = False`  
Extended option ReadEntireResponse
* `--s3-ext-buffersize = 8192`  
Extended option BufferSize
* `--s3-ext-progressupdateinterval = 102400`  
Extended option ProgressUpdateInterval
* `--s3-ext-resignretries = False`  
Extended option ResignRetries
* `--s3-ext-allowautoredirect = False`  
Extended option AllowAutoRedirect
* `--s3-ext-logmetrics = False`  
Extended option LogMetrics
* `--s3-ext-disablelogging = False`  
Extended option DisableLogging
* `--s3-ext-usedualstackendpoint = False`  
Extended option UseDualstackEndpoint
* `--s3-ext-throttleretries = True`  
Extended option ThrottleRetries
* `--s3-ext-proxyhost`  
Extended option ProxyHost
* `--s3-ext-proxyport = 0`  
Extended option ProxyPort
* `--s3-ext-proxybypassonlocal = False`  
Extended option ProxyBypassOnLocal
* `--s3-ext-maxidletime = 50000`  
Extended option MaxIdleTime
* `--s3-ext-connectionlimit = 50`  
Extended option ConnectionLimit
* `--s3-ext-usenaglealgorithm = False`  
Extended option UseNagleAlgorithm

## SFTP (SSH)

Duplicati can use SSH servers to store backups. The following target URL formats can be used:

`ssh://hostname/folder`

Options:

* `--auth-password`  
The password used to connect to the server. This may also be supplied as the environment variable `AUTH_PASSWORD`.
* `--auth-username`  
The username used to connect to the server. This may also be supplied as the environment variable `AUTH_USERNAME`.
* `--ssh-fingerprint`  
The server fingerprint used for validation of server identity. Format is eg. `ssh-rsa 4096 11:22:33:44:55:66:77:88:99:00:11:22:33:44:55:66`.
* `--ssh-accept-any-fingerprints`  
To guard against man-in-the-middle attacks, the server fingerprint is verified on connection. Use this option to disable host-key fingerprint verification. You should only use this option for testing.
* `--ssh-keyfile`  
Points to a valid OpenSSH keyfile. If the file is encrypted, the password supplied is used to decrypt the keyfile.  
If this option is supplied, the password is not used to authenticate. This option only works when using the managed SSH client.
* `--ssh-key`  
An url-encoded SSH private key. The private key must be prefixed with `sshkey://`. If the file is encrypted, the password supplied is used to decrypt the keyfile. If this option is supplied, the password is not used to authenticate. This option only works when using the managed SSH client.
* `--ssh-operation-timeout = 0`  
Use this option to manage the internal timeout for SSH operations. If this options is set to zero, the operations will not time out
* `--ssh-keepalive = 0`  
This option can be used to enable the keep-alive interval for the SSH connection. If the connection is idle, aggressive firewalls might close the connection. Using keep-alive will keep the connection open in this scenario. If this value is set to zero, the keep-alive is disabled.

## Amazon Cloud Drive

This backend can read and write data to Amazon Cloud Drive. Supported format is

`amzcd://folder/subfolder`

Supported options:

* `--authid (Password)`  
The authorization code.  
The authorization token retrieved from https://duplicati-oauth-handler.appspot.com?type=amzcd
* `--amzcd-labels (String)`  
The labels to set.  
Use this option to set labels on the files and folders created  
Default value: `duplicati,backup`
* `--amzcd-consistency-delay (Timespan)`  
The consistency delay.  
Amazon Cloud drive needs a small delay for results to stay consistent.  
Default value: `15s`

## Azure blob

This backend can read and write data to Azure blob storage. Allowed format:

`azure://bucketname`

Supported options:

* `--azure_account_name (String)`  
The storage account name.  
The Azure storage account name which can be obtained by clicking the "Manage Access Keys" button on the storage account dashboard.
* `--azure_access_key (Password)`  
The access key.  
The Azure access key which can be obtained by clicking the "Manage Access Keys" button on the storage account dashboard.
* `--azure_blob_container_name (String)`  
The name of the storage container.  
All files will be written to the container specified.
* `--auth-password (Password)`  
Supplies the password used to connect to the server.  
The password used to connect to the server. This may also be supplied as the environment variable `AUTH_PASSWORD`.
* `--auth-username (String)`  
Supplies the username used to connect to the server.  
The username used to connect to the server. This may also be supplied as the environment variable `AUTH_USERNAME`.

## B2 Cloud Storage

This backend can read and write data to the Backblaze B2 Cloud Storage. Allowed format:

`b2://bucketname/prefix`
Supported options:

* `--b2-accountid (String)`  
The "B2 Cloud Storage Account ID".  
The "B2 Cloud Storage Account ID" can be obtained after logging into your Backblaze account, this can also be supplied through the `auth-username` property.  
Aliases: `--auth-password`
* `--b2-applicationkey (Password)`  
The "B2 Cloud Storage Application Key" can be obtained after logging into your Backblaze account, this can also be supplied through the `auth-password` property.  
The "B2 Cloud Storage Application Key".  
Aliases: `--auth-username`
* `--auth-password (Password)`  
Supplies the password used to connect to the server.  
The password used to connect to the server. This may also be supplied as the environment variable `AUTH_PASSWORD`.
* `--auth-username (String)`  
Supplies the username used to connect to the server.  
The username used to connect to the server. This may also be supplied as the environment variable `AUTH_USERNAME`.
* `--b2-create-bucket-type (String)`  
The bucket type used when creating a bucket.  
By default, a private bucket is created. Use this option to set the bucket type. Refer to the B2 documentation for allowed types.  
Default value: `allPrivate`
* `--b2-page-size (Integer)`  
The size of file-listing pages.  
Use this option to set the page size for listing contents of B2 buckets. A lower number means less data, but can increase the number of Class C transaction on B2\. Suggested values are between `100` and `1000`  
Default value: `500`

## Box.com

This backend can read and write data to Box.com. Supported format is

`box://folder/subfolder`

Supported options:

* `--authid (Password)`  
The authorization code.  
The authorization token retrieved from https://duplicati-oauth-handler.appspot.com?type=box.com
* `--box-delete-from-trash (Boolean)`  
Force delete files.  
After deleting a file, it may end up in the trash folder where it will be deleted after a grace period. Use this command to force immediate removal of delete files.

## Dropbox

This backend can read and write data to Dropbox. Supported format is

`dropbox://folder/subfolder`

Supported options:

* `--authid (Password)`  
The authorization code.  
The authorization token retrieved from [https://duplicati-oauth-handler.appspot.com?type=dropbox](https://duplicati-oauth-handler.appspot.com?type=dropbox)

## Google Cloud Storage

This backend can read and write data to Google Cloud Storage. Supported format is

`googlecloudstore://bucket/folder`

Supported options:

* `--gcs-location (String)`  
Specifies location option for creating a bucket.  
This option is only used when creating new buckets. Use this option to change what region the data is stored in.  
Charges vary with bucket location. Known bucket locations:
    * (default):
    * Europe: EU
    * United States: US
    * Asia: ASIA
    * Eastern Asia-Pacific: ASIA-EAST1
    * Central United States 1: US-CENTRAL1
    * Central United States 2: US-CENTRAL2
    * Eastern United States 1: US-EAST1
    * Eastern United States 2: US-EAST2
    * Eastern United States 3: US-EAST3
    * estern United States: US-WEST1
* `--gcs-storage-class (String)`  
Specifies storage class for creating a bucket.  
This option is only used when creating new buckets. Use this option to change what storage type the bucket has.  
Charges and functionality vary with bucket storage class. Known storage classes:
    * (default):
    * Europe: EU
    * United States: US
    * Asia: ASIA
    * Eastern Asia-Pacific: ASIA-EAST1
    * Central United States 1: US-CENTRAL1
    * Central United States 2: US-CENTRAL2
    * Eastern United States 1: US-EAST1
    * Eastern United States 2: US-EAST2
    * Eastern United States 3: US-EAST3
    * Western United States: US-WEST1
* `--authid (Password)`  
The authorization code.  
The authorization token retrieved from https://duplicati-oauth-handler.appspot.com?type=gcs
* `--gcs-project (String)`  
Specifies project for creating a bucket.  
This option is only used when creating new buckets. Use this option to supply the project ID that the bucket is attached to. The project determines where usage charges are applied.

## Google Drive

This backend can read and write data to Google Drive. Supported format is

`googledrive://folder/subfolder`

Supported options:

* `--authid (Password)`  
The authorization code.  
The authorization token retrieved from [https://duplicati-oauth-handler.appspot.com?type=googledrive](https://duplicati-oauth-handler.appspot.com?type=googledrive)

## HubiC

This backend can read and write data to HubiC. Supported format is

`hubic://container/folder`

Supported options:

* `--authid (Password)`  
The authorization code.  
The authorization token retrieved from [https://duplicati-oauth-handler.appspot.com?type=hubic](https://duplicati-oauth-handler.appspot.com?type=hubic)

## Jottacloud

This backend can read and write data to Jottacloud using it's REST protocol. Allowed format is

`jottacloud://folder/subfolder`

Supported options:

* `--auth-password (Password)`  
Supplies the password used to connect to the server.  
The password used to connect to the server. This may also be supplied as the environment variable `AUTH_PASSWORD`.
* `--auth-username (String)`  
Supplies the username used to connect to the server.  
The username used to connect to the server. This may also be supplied as the environment variable `AUTH_USERNAME`.
* `--jottacloud-device (String)`  
Supplies the backup device to use.  
The backup device to use. Will be created if not already exists. You can manage your devices from the backup panel in the Jottacloud web interface. When you specify a custom device you should also specify the mount point to use on this device with the `jottacloud-mountpoint` option.
* `--jottacloud-mountpoint (String)`  
Supplies the mount point to use on the server.  
The mount point to use on the server. The default is `Archive` for using the built-in archive mount point. Set this option to` `Sync` to use the built-in synchronization mount point instead, or if you have specified a custom device with option `jottacloud-device` you are free to name the mount point as you like.

## Mega.nz

This backend can read and write data to Mega.co.nz. Allowed format:

`mega://folder/subfolder

Supported options:

* `--auth-password (Password)`  
Supplies the password used to connect to the server.  
The password used to connect to the server. This may also be supplied as the environment variable `AUTH_PASSWORD`.
* `--auth-username (String)`  
Supplies the username used to connect to the server.  
The username used to connect to the server. This may also be supplied as the environment variable `AUTH_USERNAME`.

## Microsoft OneDrive

Duplicati can use Microsoft Onedrive to store backups. The following target URL format is used:

`onedrive://folder/subfolder`

Options:

* `--authid`  
The authorization token retrieved from [https://duplicati-oauth-handler.appspot.com?type=onedrive](https://duplicati-oauth-handler.appspot.com?type=onedrive)

## Microsoft OneDrive for Business

Supports connections to Microsoft OneDrive for Business. Allowed formats are

`od4b://tennant.sharepoint.com/personal/username_domain/Documents/subfolder`

or

`od4b://username:password@tennant.sharepoint.com/personal/username_domain/Documents/folder`

You can use a double slash '//' in the path to denote the base path from the documents folder.

Supported options:

* `--auth-password (Password)`  
Supplies the password used to connect to the server.  
The password used to connect to the server. This may also be supplied as the environment variable `AUTH_PASSWORD`.
* `--auth-username (String)`  
Supplies the username used to connect to the server.  
The username used to connect to the server. This may also be supplied as the environment variable `AUTH_USERNAME`.
* `--integrated-authentication (Boolean)`  
Use windows integrated authentication to connect to the server.  
If the server and client both supports integrated authentication, this option enables that authentication method.  
This is likely only available with windows servers and clients.
* `--delete-to-recycler (Boolean)`  
Move deleted files to the recycle bin.  
Use this option to have files moved to the recycle bin folder instead of removing them permanently when compacting or deleting backups.
* `--binary-direct-mode (Boolean)`  
Upload files using binary direct mode.  
Use this option to upload files to SharePoint as a whole with BinaryDirect mode. This is the most efficient way of uploading, but can cause non-recoverable timeouts under certain conditions. Use this option only with very fast and stable internet connections.  
Default value: `false`
* `--web-timeout (Timespan)`  
Set timeout for SharePoint web operations.  
Use this option to specify a custom value for timeouts of web operation when communicating with SharePoint Server.  
Recommended value is `180s`.
* `--chunk-size (Size)`  
Set block size for chunked uploads to SharePoint.  
Use this option to specify the size of each chunk when uploading to SharePoint Server. Recommended value is 4MB.  
Default value: `4mb`

## Microsoft SharePoint

Supports connections to a SharePoint server (including OneDrive for Business). Allowed formats are

`mssp://tennant.sharepoint.com/PathToWeb//BaseDocLibrary/subfolder`

or

`mssp://username:password@tennant.sharepoint.com/PathToWeb//BaseDocLibrary/subfolder`

Use a double slash '//' in the path to denote the web from the documents library.

Supported options:

* `--auth-password (Password)`  
Supplies the password used to connect to the server.  
The password used to connect to the server. This may also be supplied as the environment variable `AUTH_PASSWORD`.
* `--auth-username (String)`  
Supplies the username used to connect to the server.  
The username used to connect to the server. This may also be supplied as the environment variable `AUTH_USERNAME`.
* `--integrated-authentication (Boolean)`  
Use windows integrated authentication to connect to the server.  
If the server and client both supports integrated authentication, this option enables that authentication method.  
This is likely only available with windows servers and clients.
* `--delete-to-recycler (Boolean)`  
Move deleted files to the recycle bin.  
Use this option to have files moved to the recycle bin folder instead of removing them permanently when compacting or deleting backups.
* `--binary-direct-mode (Boolean)`  
Upload files using binary direct mode.  
Use this option to upload files to SharePoint as a whole with BinaryDirect mode. This is the most efficient way of uploading, but can cause non-recoverable timeouts under certain conditions. Use this option only with very fast and stable internet connections.  
Default value: `false`
* `--web-timeout (Timespan)`  
Set timeout for SharePoint web operations.  
Use this option to specify a custom value for timeouts of web operation when communicating with SharePoint Server.  
Recommended value is `180s`.
* `--chunk-size (Size)`  
Set block size for chunked uploads to SharePoint.  
Use this option to specify the size of each chunk when uploading to SharePoint Server. Recommended value is `4MB`.  
Default value: `4mb`

## Rackspace Cloudfiles

Supports connections to the CloudFiles backend. Allowed formats is

`cloudfiles://container/folder`

Supported options:

* `--auth-password (Password)`  
Supplies the password used to connect to the server.  
The password used to connect to the server. This may also be supplied as the environment variable `AUTH_PASSWORD`.
* `--auth-username (String)`  
Supplies the username used to connect to the server.  
The username used to connect to the server. This may also be supplied as the environment variable `AUTH_USERNAME`.
* `--cloudfiles-username (String)`  
Supplies the username used to authenticate with CloudFiles.  
Aliases: `--auth-username
* `--cloudfiles-accesskey (Password)`  
Supplies the access key used to connect to the server.  
Supplies the API Access Key used to authenticate with CloudFiles.  
Aliases: `--auth-password`
* `--cloudfiles-uk-account (Boolean)`  
Use a UK account.  
Duplicati will assume that the credentials given are for a US account, use this option if the account is a UK based account. Note that this is equivalent to setting `--cloudfiles-authentication-url=https://lon.auth.api.rackspacecloud.com/v1.0`.
* `--cloudfiles-authentication-url (String)`  
Provide another authentication URL.  
CloudFiles use different servers for authentication based on where the account resides, use this option to set an alternate authentication URL. This option overrides `--cloudfiles-uk-account`.  
Default value: `https://identity.api.rackspacecloud.com/auth`

## Rclone

This backend can read and write data to Rclone. Allowed format:

`rclone://server/folder`

Supported options:

* `--rclone-local-repository (String)`  
Local repository for Rclone. Make sure it is configured as a local drive, as it needs access to the files generated by Duplicati.  
Default value: `local`  
* `--rclone-remote-repository (String)`  
Remote repository for Rclone. This can be any of the backends provided by Rclone. More info available on https://rclone.org/.  
Default value: `remote`  
* `--rclone-remote-path (String)`  
Path on the Remote repository.  
Default value: `backup`  
* `--rclone-option (String)`  
Options will be transferred to rclone.  
* `--rclone-executable (String)`  
Full path to the rclone executable. Only needed if it's not in your path.  
Default value: `rclone`  

## Sia Decentralized Cloud

This backend can read and write data to Sia. Allowed format:

`sia://server/folder/BaseDocLibrary/subfolder`

Supported options:

* `--sia-targetpath (String)`  
Backup path.  
Target path, ie `/backup`  
Default value: `/backup`
* `--sia-password (Password)`  
Sia password
* `--sia-redundancy (String)`  
Minimum value is `3`.  
Default value: `1.5`

## Tahoe-LAFS

Duplicati can use the TahoeLAFS to store backups. The following target URL format is used:

`tahoe://hostname:port/uri/$DIRCAP`

Options:

* `--use-ssl` 
Use this flag to communicate using Secure Socket Layer (SSL) over http (https).



