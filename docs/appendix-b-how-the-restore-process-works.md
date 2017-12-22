

# How the restore process works


### Duplicati restore process

If you have read the How the backup process works document, you might be wondering how the restore process uses the stored data to restore your files. This document explains this process, using the `Duplicati.CommandLine.RecoveryTool.exe` process as the starting point, so we can skip the complications added by the local database. Unless you are curious about the inner workings of Duplicati, you do not need to read this document, you can simply use the normal restore process, or the recovery tool.

### The example files

For this document we continue with the example from the backup document, and walk through the restore process.

From the initial backup, we saw that the directory structure that was backed up looks like this:

```nohighlight
C:\data
|----> mydoc.txt, 4kb
|----> myvideo.mp4, 210kb
|----> extra
       |-----> olddoc.txt, 2kb
       |-----> samevideo.mp4, 210kb
```

To simplify matters, we will not use a local database, and we only look at file data, not metadata and not directories.

### Getting the initial list

To start with, we need to pick the `dlist` file that contains the version of the files that we want. In a real-world example, you can observe the names of the `dlist` files, which have a timestamp embedded in them. The timestamps are stored in UTC, so you may need to adjust to your local timezone.

For this example we only have a single `dlist` file, so we pick that one, download it and see the contents:

```nohighlight
[
  {
  "type": "Folder",
  "path": "C:\\data\\"
  },
  {
  "type": "File",
  "path": "C:\\data\\mydoc.txt",
  "size": 4096,
  "hash": "qaFXpxVTuYCuibb9P41VSeVn4pIaK8o3jUpJKqI4VF4="
  },
  {
  "type": "File",
  "path": "C:\\data\\myvideo.mp4",
  "size": 215040,
  "hash": "4sGwVN/QuWHD+yVI10qgYa4e2F5M4zXLKBQaf1rtTCs=",
  "blocklists": [ "Uo1f4rVjNRX10HkxQxXauCrRv0wJOvStqt9gaUT0uPA=" ]
  },
  {
  "type": "Folder",
  "path": "C:\\data\\extra"
  },
  {
  "type": "File",
  "path": "C:\\data\\extra\\olddoc.txt",
  "size": 2048,
  "hash": "R/XSNsb4ln/SkeJwFDd4Fv4OnW2QNIxMR4HItgg9qCE="
  },
  {
  "type": "File",
  "path": "C:\\data\\extra\\samevideo.mp4",
  "size": 215040,
  "hash": "4sGwVN/QuWHD+yVI10qgYa4e2F5M4zXLKBQaf1rtTCs=",
  "blocklists": [ "Uo1f4rVjNRX10HkxQxXauCrRv0wJOvStqt9gaUT0uPA=" ]
  },
]
```

From this we can see that there are 4 files we need to restore. We can also see that there are files that need "blocklists".


### Expanding blocklists

As explained in the How the Backup Process Works document, the blocklists are data blocks that contain additional hashes needed to restore the files. Thus we start by "expanding" the blocklists into a list of hashes that we need.

As the two files share the blocklist has `Uo1f4rVjNRX10HkxQxXauCrRv0wJOvStqt9gaUT0uPA=`, we only need to get this block to complete the expansion phase.

Unfortunately, there is no correlation between the names of the `dblock` files and the data they contain, so we need to download all of them, until we find the data we need. Since this is slow in a real-world scenario, Duplicati replicates this information in the `dindex` files, which are much smaller than the `dblock` files.

Assuming we have the same `dblock` file as mentioned in the backup document, we get a zip file with these files:

```nohighlight
qaFXpxVTuYCuibb9P41VSeVn4pIaK8o3jUpJKqI4VF4= (4kb)
0td8NEaS7SMrQc5Gs0Sdxjb/1MXEEuwkyxRpguDiWsY= (100kb)
PN2oO6eQudCRSdx3zgk6SJvlI5BquP6djt5hG4ZfRCQ= (100kb)
uS/2KMSmm2IWlZ77JiHH1p/yp7Cvhr8CKmRHJNMRqwA= (10kb)
Uo1f4rVjNRX10HkxQxXauCrRv0wJOvStqt9gaUT0uPA= (96b)
R/XSNsb4ln/SkeJwFDd4Fv4OnW2QNIxMR4HItgg9qCE= (2kb)
```

Here we see that the file `Uo1f4rVjNRX10HkxQxXauCrRv0wJOvStqt9gaUT0uPA=` is 96 bytes, and we know that a sha256 is 32 bytes. We can then compute that this chunk of data expands to `96/32 = 3` hashes. We could also compute this size, by looking at the size of the file, and then find out how many 100kb blocks it spans.

For efficiency, the hashes are stored in raw binary format, but here we can represent them as base64 encoded strings:

```nohighlight
0td8NEaS7SMrQc5Gs0Sdxjb/1MXEEuwkyxRpguDiWsY=
PN2oO6eQudCRSdx3zgk6SJvlI5BquP6djt5hG4ZfRCQ=
uS/2KMSmm2IWlZ77JiHH1p/yp7Cvhr8CKmRHJNMRqwA=
```

We can now expand `Uo1f4rVjNRX10HkxQxXauCrRv0wJOvStqt9gaUT0uPA=` into the three real hashes, and we now have an expanded list of hashes for each file we need to restore.


### Restoring small files

Restoring the small files is simple: extract the contents and put them into a file with that name. The process is then to locate the `dblock` file that contains the block we need, and extracting it. In Duplicati, this is improved with `dindex` files, that contains a map showing which blocks can be found in a `dblock` file, such that a download can be avoided.


### Restoring large files

As we have expanded the list of hashes, we follow the same process as for the small files, and simply locate blocks, one at a time. If we restore the blocks in the order specified, each block will represent the correct length (100kb), and we can simply extract the files from the zip archive, and append it to the file. Since we use a fixed size block, we can also choose to restore out-of-order, by computing the file offset for each block before inserting the data.

### Verification of the restored files

As we saw in the file list contents, each file also has a `hash` property, which we can use after the restore process to verify that each file is restored correctly.

### Building a local index

For the `Duplicati.CommandLine.RecoveryTool.exe` the above process is followed as described, but with an two additional steps: download and index.

The download process, merely downloads (and decrypts) all the dblock files it can find on the remote storage, such that all the following operations can be done with local files.

Since the recovery tool does not rely on `dindex` files, it would be extremely slow if it had to open all zip files to check if they contain the block it wants to process. The index process speeds this up significantly, by producing a plain text file, where each line presents a (block, zip archive) pair.

The index step then opens each `dblock` file, and lists the contents, and outputs a line for each data block it finds. After each `dblock` file has been processed, the file is sorted alphabetically.

There are more efficient ways to store this data, but the text file allows an expert user to monitor, update and adjust the index file with a simple text editor, should something go wrong. An expert user can also investigate the `dlist` file, and use the index file to figure out where a particular block is.

For the final phase in the recovery tool, restore, the sorted index is used to locate the `dblock` file. This search relies on the alphabetic sorting to ensure that lookup times does not grow linearly when the number of data blocks.

