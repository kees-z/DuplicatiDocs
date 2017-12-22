
# Choosing sizes in Duplicati

All options in Duplicati are chosen to fit a wide range of users, such that as few as possible of the users need to change settings.

Some of these options are related to sizes of various elements. Choosing these options optimally is a balance between different usage scenarios and has different tradeoffs. This documents explains what these tradeoffs are and how to choose those that fit a specific backup best.



### The block size

As Duplicati makes backups with `blocks`, aka "file chunks", one option is to choose what size a "chunk" should be.

The chunk size is set via the advanced option `--block-size` and is set to 100kb by default. If a file is smaller than the chunk size, or the size is not evenly divisible by the block size, it will generate a block that is smaller than the chunk size.

Due to the way blocks are referenced (by hashes), it is not possible to change the chunk size after the first backup has been made. Duplicati will abort the operation with an error if you attempt to change the chunk size on an existing backup.

### Using larger block sizes

If you choose a larger chunk size, that will obviously generate "fewer but larger blocks", provided your files are larger than the chunk size. It is also possible to choose a smaller chunk size, but for most cases this has a negative impact.

Internally each block needs to be stored, so having fewer blocks, means smaller (and thus faster) lookup tables. This effect is more noticeable if the database is stored on non-ssd disks (aka spinning disks).

If you have large files, choosing a large chunk size, will also reduce the storage overhead a bit. When restoring, this is also a benefit, as more data can be streamed into the new file, and the data will likely span fewer remote files.

The downside to choosing a large chunk size, is that change detection and deduplication covers a larger area.

If a single byte is changed in a file, Duplicati will need to upload a new chunk. If there are many small changes to the files, this will generate many new blocks, which increases the required storage space as well as the required bandwidth.

With larger chunk sizes, it is also less likely that deduplication will detect any matching chunks, as the shared chunks contain more data.

If there is sufficient bandwidth to the remote destination, choosing a larger chunk size is usually beneficial. The lower limit is 10kb, and there is no upper limit, but choosing values larger than 1mb should only be done after evaluating the above impacts.

### Remote Volume Size

Rather than storing the chunks individually, Duplicati groups data in volumes, which reduces the number of the remote files, and calls to the remote server. The volumes are then compressed, which saves storage space and bandwidth. Encryption is applied to the volumes, which reduces the possibility of someone deducing properties about the contents inside the volume.

The volume size can be set in the graphical user interface, as well as on the commandline with the option `--dblock-size`. The remote volumes are called `dblock` files internally, and that is the extension used for the files.

The default size is 50mb, which is chosen as a sensible default for home users with limited upload speeds.

Unlike the chunk size described above, it can be beneficial to both increase or decrease the volume size to fit your connection characteristics. Also, the volume size can be changed after a backup has been created.

### Increasing the Remote Volume Size

If you increase the volume size, it will again mean "fewer but larger files". On some servers, FTP in particular, there may be a limit on the number of files that can be listed. If you decrease the number of files, you can avoid hitting that limitation, and some servers will be faster when listing the contents. This delay is caused by the servers, such as Amazon S3 and OneDrive, using "pagination" where large file lists must be collected with multiple calls.

As there are significantly fewer volume files than chunks, the impact on the local database and related operations is insignificant.

The downside of using larger volumes are seen when restoring files. As Duplicati cannot read data from inside the volumes, it needs to download the entire remote volume before it can extract the desired data. If a file is split across many remote volumes, e.g. due to updates, this will require a large amount of downloads to extract the chunks.

Another potential downside of a larger remote volume, is that the compression and encryption usually means that data corruption destroys the entire volume, instead of just a few chunks.

If you have large datasets and a stable connection, it is usually desirable to user a larger volume size. Using volume sizes larger than 1gb usually gives slowdowns as the files are slower to process, but there is no limit applied.

If you have an unstable connection, you may want to use smaller volume sizes, such that a re-transmit after a failed transfer will not require such a large re-transfer. If you need frequent restores, with often changed data, you may also want to use smaller volume sizes. To reduce the number of remote files in this scenario, consider splitting the backup into separate smaller backups.

