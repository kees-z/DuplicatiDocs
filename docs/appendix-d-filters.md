
# Filters

### General concept of filters

Duplicati 2.0 can include or exclude files and folders from your backup based on so-called filters. A filter can consist of various rules. And a rule decides if a file is to be included to the backup or excluded from it. This decision is made based on the name of the file or folder. Besides these name-based rules there is also a set of specific settings which exclude files with specific attributes (like system files or hidden files) or files that exceed a specific size.

Duplicati's filter engine processes folders first then files. The reason for that behavior is performance. If a folder is excluded from a backup, the files inside that folder don't have to be processed anymore.

When filter rules have been defined the first folder is taken and the filter rules are processed one by one. The first rule that matches is applied and the following rules are not processed anymore. For instance, if the first rule excludes a folder, then this folder and all files within will be excluded from the backup even if following rules include this folder or its files. Likewise, if the first rule includes a folder, then it will be included even if a following rule would exclude it.

It is recommended to write folder rules first and file rules afterwards. Also it is recommended to write the folder rules one directory level at a time. That way rules are written in the same order as they will be effective when Duplicati processes them and Duplicati's filters are easier to understand that way.

Per default, all files and folders will be backed up. That means, if no rule matches, the file or folder will be included. In the special case that all rules are include rules (which does not make sense when all files and folders are included per default) Duplicati assumes that all other files and folders are meant to be excluded (this had to be defined as another rule in Duplicati 1.3 but most people found that confusing so we changed that in Duplicati 2.0).

### Syntax

Folder names always end with a slash `/` on Linux or Mac and a backslash `\` on Windows. For instance, `log` is a file, `log/` is a folder.

If you want to use file globbing to specify rules, `?` and `*` are allowed placeholders. `?` matches any single character. `*` specifies none or multiple characters. For example, `*.txt` would specify all `.txt` files.

Rules can also be specified as regular expression. In this case put the regular expression (using [.NET syntax](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)) into hard brackets `[]`. For example, `[.*/IMG_\d{4}\.jpg]` would specify all files named `IMG_0000.jpg` through `IMG_9999.jpg`.

Duplicati also has several built-in filter groups, which define set of well-known files and folders for different operating systems. These can be specified by putting the names or aliases of the desired groups inside curly brackets `{}` separated by commas `,`, for example, `{SystemFiles,OperatingSystem}`. The following groups are currently defined:

- **SystemFiles**  
Files and paths that are owned by the operating system or are not real files.  
For example, `System Volume Information` on Windows and `/proc/` on Linux.
- **OperatingSystem**  
Files and folders that are normally part of the operating system.  
For example, `C:\Windows` on Windows and `/bin/` on Linux.
- **CacheFiles**  
Files and folders that are known to be cache locations.  
For example, Google Chrome and Firefox web caches.
- **TemporaryFiles**  
Files and folders that are temporary.  
For example, the recycle bin on Windows and `/tmp/` on Linux.
- **Applications**  
Files and folders that are application binaries (i.e., installed programs).  
For example, `C:\Program Files` on Windows and `/lib/` on Linux.
- **DefaultExcludes**  
Meta group which contains all groups that are commonly excluded, which is currently all other groups.

### Using filters

In the UI, filters can be created using drop down boxes for common rule types. More advanced users can also use a textbox to specify multiple rules, with a rules to include starting with `+` and rules to exclude starting with `-`.

Using the command-line there are specific settings to specify include or exclude rules. These are `--include` and `--exclude`. Multiple rules can be specified by using `--include` or `--exclude` repeatedly. Note that on Windows a trailing backslash (part of a folder rule) and only that must be doubled to avoid escaping the quotes. Thus a rule looks like `--exclude="*\a\folder rule\\"`. Backslashes in regular expressions must be doubled always: `--exclude="[.*\\a\\folder rule\\]"`

### Creating and validating your filters

Duplicati UI updates the file and folder include/exclude icons on the fly to reflect the current filters. A green check-mark indicates that the folder will be traversed by Duplicati, but its content may be excluded by other filters.

![Filter example](duplicati-filters-match-example.png "Filter example")

### Settings

Besides filter rules there are settings that can exclude specific files by their attributes. Those settings are `--skip-files-larger-than` and `--exclude-files-attributes`. The latter is able to exclude files that have any of the following attributes: `ReadOnly`, `Hidden`, `System`, `Directory`, `Archive`, `Device`, `Normal`, `Temporary`. Those settings are applied to all files of the backup.

### Common use cases

**Exclude specific sub-folders.** On your NAS you want to backup all photos. Your photos are stored in hundreds of folders and each of those folders contains a sub-folder called "@eaDir" that contains thumbnails in different sizes that your NAS uses for a web interface. You want to backup your photos but not the thumbnails. In this example you just exclude all thumbnail folders and thus their content. The rule is: `-*/@eaDir/`. Don't forget the trailing slash that defines @eaDir as a folder.

**Exclude specific files.** You store your photos and movies in the same folders. For some reason, you do not want to include the movies into your backup. Depending on why you want to exclude those movies, there are different solutions. First solution is to define a rule like `-*.mov` or `-*.avi`. Second solution is to say `--skip-files-larger-than=10M` which will exclude all files that are larger than 10MB which will probably affect all movies but no photos. Third way is to explicitly say that you only want to include the photos. Then read on how to do that.

**Include specific files only.** You have a folder structure that contains a lot of photos and movies from your camera. For some reason you only want to include the photos to your backup. The rule for your backup is `+*.jpg +*.jpeg`. As there are including rules in this filter, Duplicati automatically excludes all other files. This had to be done manually in Duplicati 1.3 which made including rules a little bit difficult for most users.

**Include some files, exclude others.** Now let's define a filter that does both of the above. First it excludes @eaDir specifying `-*/@eaDir/`. Then it includes only JPG files specifying `+*.jpg`. The problem here is, that Duplicati includes all files and folders per default. This means that e.g. /photos/movie.avi will also be part of the backup. To make the including rule effective an additional rule is required that excludes all files that do not match any of the current rules. The filter must say "exclude this, exclude that, include this but nothing else". The best rule for "but nothing else" is a regular expression that excludes all files. It is `-[.*[^/]]` on Linux or Mac, and on Windows the rule is `-[.*[^\\]]`. The rule says "exclude everything that is not a folder". The final filter then is `-*/@eaDir/ +*.jpg +*.jpeg -[.*[^/]]`. Duplicati will process all folders but @eaDir/ and it will include JPG and JPEG files but exclude all other files.


**Advanced regular expression filter example** 

 * Suppose we want `/mnt/(user|disk\d+)/media/.*` but not `Movie.*` folders within `media/`
   * This includes `/mnt/user/media/X`, `/mnt/disk23/media/Y`, but not `/mnt/user/media/Movie/DieHard.mkv`. 
   * We don't want `/mnt/user0/.*`, or `/mnt/user/secrets`

Duplicati applies the filters to a folder before its children, searching for the first filter-line matching the folder. If a parent path matches an exclude then that whole tree is cut off. Conceptually its easiest to build the expressions by starting at the top of the folder hierarchy and move down one level including/excluding the desired files. So let us first match the parent folders we want to be processed, then remove those we don't want. Then we add the subfolder we want, and exclude all others. 

 * Source set: /mnt/
 * `+[/mnt/(disk\d*|user)/]`
 * `-[/mnt/[^/]*/]`
 * `+[/mnt/[^/]*/media/]`
 * `-[/mnt/[^/]*/[^/]*/]`
 * `-[/mnt/[^/]*/media/Movie[^/]*/]`
 * `+[/mnt/[^/]*/media/[^/]*/]`
 * `-[/mnt/[^/]*/.*\.log]`

Note that `.*` matches anything, and `[^/]*` matches anything NOT containing a `/` (linux path separator).

* The first two lines match the root folders in our `/mnt` source-set, including only folders like `disk123` and `user`.
* The next two lines include only media subfolder in the already included. Notice how `[^/]*` does not match a path-separator `/`
* The 5-6 lines exclude directories like `Movie` and `MovieSeen`, but includes the rest
* The last line excludes all files with `.log` extension (case-sensitive). Here we use `.*` to match including paths. An equivalent alternative is `[/mnt/[^/]*/.*/[^/]*\.log]` where the last path separator is included so `[^/]*\.log` matches a filename. Or just `[.*\.log]` to skip all that verbosity (and stay in the reg-exp world, which isn't requried).

Notice also in lines 5-6 we stay on one directory level, we do not match `Movie.*`. Both would be valid, The reason being that it is then easier to remember that Duplicati works through directories a level at a time.

The 6. filter (include) can be omitted, as files which do not match a filter are included (unless all filters are include filters).

Here we utilize that in Duplicati all folders end in / (in Linux), while a file does not end in / (for Windows its backslash).

# Testing Filters with command line tool

Filters can be tested with the command line tool, see https://github.com/duplicati/duplicati/wiki/Headless-installation-on-Debian-or-Ubuntu and https://duplicati.readthedocs.io/en/latest/04-using-duplicati-from-the-command-line/

For instance test the above example on a [linuxserver docker](https://hub.docker.com/r/linuxserver/duplicati) or [duplicati/duplicati](https://hub.docker.com/r/duplicati/duplicati) with 

```
docker exec duplicati mono /app/duplicati/Duplicati.CommandLine.exe test-filters /mnt/ \
  --include="[/mnt/(disk\d*|user)/]"\
  --exclude="[/mnt/[^/]*/]"\
  --include="[/mnt/[^/]*/media/]"\
  --exclude="[/mnt/[^/]*/[^/]*/]"\
  --exclude="[/mnt/[^/]*/media/Movie[^/]*/]"\
  --include="[/mnt/[^/]*/media/[^/]*/]"\
  --exclude="[/mnt/[^/]*/.*\.log]"
```

**Building Regular Expressions:** There are lots of online services such as [Skinners RegExp engine](https://regexr.com) to help build correct regular expressions.
