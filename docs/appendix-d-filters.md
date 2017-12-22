
# Filters

### General concept of filters

Duplicati 2.0 can include or exclude files and folders from your backup based on so-called filters. A filter can consist of various rules. And a rule decides if a file is to be included to the backup or excluded from it. This decision is made based on the name of the file or folder. Besides these name-based rules there is also a set of specific settings which exclude files with specific attributes (like system files or hidden files) or files that exceed a specific size.

Duplicati's filter engine processes folders first then files. The reason for that behavior is performance. If a folder is excluded from a backup, the files inside that folder don't have to be processed anymore.

When filter rules have been defined the first folder is taken and the filter rules are processed one by one. The first rule that matches is applied and the following rules are not processed anymore. For instance, if the first rule excludes a folder, then this folder and all files within will be excluded from the backup even if following rules include this folder or its files.

It is recommended to write folder rules first and file rules afterwards. That way rules are written in the same order as they will be effective when Duplicati processes them and Duplicati's filters are easier to understand that way.

Per default, all files and folders will be backed up. That means, if no rule matches, the file or folder will be included. In the special case that all rules are include rules (which does not make sense when all files and folders are included per default) Duplicati assumes that all other files and folders are meant to be excluded (this had to be defined as another rule in Duplicati 1.3 but most people found that confusing so we changed that in Duplicati 2.0).

### Syntax

If you want to use file globbing to specify rules, `?` and `*` are allowed placeholders. `?` matches any single character. `*` specifies none or multiple characters. Rules can also be specified as regular expression. In this case put the regular expression (using .NET syntax) into hard brackets `[]`. Folder names always end with a slash `/` on Linux or Mac and a backslash `\` on Windows. For instance, `log` is a file, `log/` is a folder. In the UI a rule to include is started with a `+`, a rule to exclude is started with a `-`. Using the command-line there are specific settings to specify include or exclude rules. These are `--include` and `--exclude`. Using the command-line various rules can be specified using `--include` or `--exclude` repeatedly.

### Settings

Besides filter rules there are settings that can exclude specific files by their attributes. Those settings are `--skip-files-larger-than` and `--exclude-files-attributes`. The latter is able to exclude files that have any of the following attributes: ReadOnly, Hidden, System, Directory, Archive, Device, Normal, Temporary. Those settings are applied to all files of the backup.

### Common use cases

**Exclude specific sub-folders.** On your NAS you want to backup all photos. Your photos are stored in hundreds of folders and each of those folders contains a sub-folder called "@eaDir" that contains thumbnails in different sizes that your NAS uses for a web interface. You want to backup your photos but not the thumbnails. In this example you just exclude all thumbnail folders and thus their content. The rule is: `-*/@eaDir/`. Don't forget the trailing slash that defines @eaDir as a folder.

**Exclude specific files.** You store your photos and movies in the same folders. For some reason, you do not want to include the movies into your backup. Depending on why you want to exclude those movies, there are different solutions. First solution is to define a rule like `-*.mov` or `-*.avi`. Second solution is to say `--skip-files-larger-than=10M` which will exclude all files that are larger than 10MB which will probably affect all movies but no photos. Third way is to explicitly say that you only want to include the photos. Then read on how to do that.

**Include specific files only.** You have a folder structure that contains a lot of photos and movies from your camera. For some reason you only want to include the photos to your backup. The rule for your backup is `+*.jpg +*.jpeg`. As there are including rules in this filter, Duplicati automatically excludes all other files. This had to be done manually in Duplicati 1.3 which made including rules a little bit difficult for most users.

**Include some files, exclude others.** Now let's define a filter that does both of the above. First it excludes @eaDir specifying `-*/@eaDir/`. Then it includes only JPG files specifying `+*.jpg`. The problem here is, that Duplicati includes all files and folders per default. This means that e.g. /photos/movie.avi will also be part of the backup. To make the including rule effective an additional rule is required that excludes all files that do not match any of the current rules. The filter must say "exclude this, exclude that, include this but nothing else". The best rule for "but nothing else" is a regular expression that excludes all files. It is `-[.*[^/]]` on Linux or Mac, and on Windows the rule is `-[.*[^\\]]`. The rule says "exclude everything that is not a folder". The final filter then is `-*/@eaDir/ +*.jpg +*.jpeg -[.*[^/]]`. Duplicati will process all folders but @eaDir/ and it will include JPG and JPEG files but exclude all other files.

