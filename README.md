# drotate

drotate archives and deletes (rotates) files and directories older than a
given age.

## Description

*drotate* recursively finds all files and directories in a given directory
older than a certain number of days and archives and deletes (rotates)
them.  It can be used to archive regularly accumulating files  that  don't
need to be in direct access forever and can be archived after some time.

*drotate* can  be  automatically called on a daily/weekly/monthly basis but
the rotation will only happen when the last rotation dates back a given
amount of days. This prevents the creation of many  small  archives  and
allows periodic execution via cron.

See the man-page for detailed information on usage and options.

## Sample run

Sample run

This is what a first run on a directory without subdirectories can look like:

    tobi@kara:~/webnews$ drotate -v -i 2 -d heise
    2008-04-02 14:58:03 drotate 0.9.2
    
    INFO: No status file found in 'heise/.lastdrotate'; creating it
    when archiving was successful.
    
    Creating list of all files older than 2 days ...
    Creating list of all directories older than 2 days ...
    The following 14 files will be archived and deleted:
    heise_105706.html
    heise_105701.html
    heise_105709.html
    heise_105700.html
    heise_105702.html
    heise_105710.html
    heise_105707.html
    heise_105708.html
    heise_105704.html
    heise_105699.html
    heise_105703.html
    heise_105711.html
    heise_105698.html
    heise_105705.html
    
    The following 0 directories will be archived and deleted:
    
    Creating archive 'heise_2008-04-02_14-58.tar.gz' ...
    Deleting 14 files ...
    Rotation complete.

## License

*drotate*, Copyright (C) 2008-2016 Tobias M.-Nissen <tn@movb.de>, 2016 Tobias König <tn@movb.de>

*drotate* is free software: you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free
Software Foundation, either version 3 of the License, or (at your option)
any later version.

This program is distributed in the hope that it will be useful, but WITHOUT
ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
FITNESS FOR A PARTICULAR PURPOSE. See the
[GNU General Public License](http://www.gnu.org/licenses/) for more details.


