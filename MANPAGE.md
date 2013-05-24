# NAME

*drotate* - archives and deletes (rotates) files and directories older than a given age

# SYNOPSIS

If the directory to rotate is specified in the config file:

    drotate [-d dir] [-o dir] [-a file] [-i days] [-c configfile] [-m file] [-psnfhv]

Otherwise the `-d` option is mandatory:

    drotate -d dir [-o dir] [-a file] [-i days] [-c configfile] [-m file] [-psnfhv]

# DESCRIPTION

*drotate* recursively finds all files and directories in a given directory older than a certain number of days
and archives and deletes (rotates) them.  It can be used to archive regularly accumulating files  that  don't
need to be in direct access forever and can be archived after some time.

*drotate* can  be  automatically called on a daily/weekly/monthly basis but the rotation will only happen when
the last rotation dates back a given amount of days. This prevents the creation of many  small  archives  and
allows periodic execution via cron.

# OPTIONS

Everything can be configured in a configuration file. The only mandatory parameter is the name of the
directory drotate shall process. If you want to do without a configuration file, you have to  pass  at  least  the
name of the directory to process with the `-d` option.

* `-d dir` specifies  the  directory  to process. Everything in it will be considered for rotation. The directory itself and the contained state file will never be subject to rotation.
* `-o dir` specifies where to save the archive file. Default is to save the archive to the current directory.
* `-a file` specifies an alternate name for the resulting archive. See the DEFAULTS section on the default archive name.
* `-i days` tells  drotate  which  interval  to  use for rotation. This serves two purposes. In the first place it specifies how old files and directories have to be in order for them to be rotated. At the  same  time this  value determines if the rotation will take place at all (certain amount of time must have passed between two rotations).
* `-g` tests for access time instead of modification time when determining the age of a file.
* `-p` prepends the hostname to the archive filename.
* `-c file` reads an alternate/additional configuration file.
* `-m file` multiplex mode: reads given file which holds a list of drotate configuration files and starts  a drotate instance for each one of them. drotate ignores any command line configuration in this mode.
* `-n` performs a dry run, i.e. doesn't touch anything and just to shows what would be done.
* `-k` keeps archived files, instead of deleting them.
* `-f` forces overwriting of an existing archive. Default is to terminate if an archive already exists.
* `-s` prevents drotate from descending into subdirectories
* `-h` displays help information. Verbosity can be increased by using the `-v` switch.
* `-v` increases verbosity.

# CONFIGURATION

drotate reads the configuration file at `$HOME/.drotate.conf` at startup if it exists.
If additional configuration files are provided with the `-c` option, these are read in
the provided order.

The definitions can be overridden by providing the appropriate options on the command line.

The configuration variables have to be set like this:

   VARIABLE=value

Notice that some values need to be quoted.

drotate recognizes the following options (for a description see the OPTIONS section):

    DIR            specifies the directory to process.
   
    ARCHDIR        specifies where to save the archive file.

    ARCHFILE       specifies an alternate name for the resulting archive.

    INTERVAL       specifies the interval of rotation.

    ACCESS         tells drotate to test for access time instead of modification time when determining the age of a file.

    PREPEND        tells drotate to prepend the hostname.

    DRYRUN         tells drotate to perform a dry run, i.e. not to touch anything and just to show what would be done.

    KEEP           tells drotate to keep archived files, instead of deleting them.

    MULTPLEX       puts drotate in multiplex mode, see `-m` above.

    DESCEND        whether to descend into subdirectories

    OVERWRITE      tells drotate to overwrite an existing archive.

    LASTROTATEFILE tells drotate to use another name for the state file.

    VERBOSE        tells drotate to be verbose.

# DEFAULTS

## Interval

The default interval is 150 days.

## Archive filename

The default archive filename has this format:

    [HOSTNAME_]directory-path_YYYY-MM-DD_hh-mm.tar.gz

whereas the directory-path is specified by the `-d` switch or the DIR config file  variable.
`YYYY-MM-DD_hh-mm` reflects the current system time. If the hostname is to be prepended, it is determined by the *hostname* command.

## Archive directory

The default directory the archive file is saved to is the current working directory.

## State file

The default name of the state file is `.lastdrotate`. It is saved in the directory to process.

## Prepending of hostname

The default is no to prepend the hostname.

# EXIT STATUS

drotate exits with status 0 if the rotation completed successfully. It exits with 1  if  an  operating  error
occurred  and  with 2 if one of the backend tools exited unexpectedly. If the archive file already exists but
drotate must not overwrite it, it exits with 3.

# BACKEND TOOLS

drotate requires the following backend tools to work:

* find, to find files to rotate
* mktemp, to create temporary files
* tr, to process lists of filenames delimited by the NULL character
* sed, to create a nice default archive name
* pwd, to get the current working directory if $PWD isn't set
* cat, to output the list of filenames
* tac, to output the list of directories in reverse order
* hostname, to get the hostname of the system
* wc, to count the number of directories and files to be processed

The availability of the following tools is optional but recommended:

* tput, to get the number of columns if $COLUMNS isn't set
* fold, to prevent line-breaks within word-boundaries

# AUTHOR

Tobias Nissen <tn@movb.de>

