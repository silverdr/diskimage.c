diskimage.c
===========

diskimage.c is an ANSI C library for manipulating Commodore disk images.

The following formats are supported:

-   D64 - single sided 1541 disk image, with optional error info
    (currently ignored).
-   D71 - double sided 1571 disk image.
-   D81 - 3.5\" 1581 disk image (only root directory)

The following operations are implemented:

-   Open file (\$ reads directory).
-   Read file.
-   Write file.
-   Close file.
-   Delete file.
-   Rename file.
-   Format disk.
-   Allocate sector.
-   Deallocate sector.

Download
--------

`diskimage-0.95.zip`
:   The latest release.

`diskimage-0.96b.zip`
:   The latest beta release.

`diskimage-20090111.zip`
:   The latest CVS snapshot, with the unfinished merge of Groepaz
    patches.

Sample Code
-----------

`test_bam.c`
:   Prints out the BAM.

`test_dir.c`
:   Lists the directory.

`test_read.c`
:   Copies a file from a disk image.

`test_write.c`
:   Copies a file to a disk image.

`test_format.c`
:   Creates an empty disk image.

API
---

### Image Mounting

#### `DiskImage *di_load_image(char *name);`

::: {.comm}
Loads an image from disk and returns a pointer to the DiskImage
structure.
:::

#### `DiskImage *di_create_image(char *name, int size);`

::: {.comm}
Creates an empty image. The type is determined from the size. Needs to
be formatted before it can be used.
:::

#### `void di_free_image(DiskImage *di);`

::: {.comm}
Frees an image in memory. If the image has been modified, the changes
will be written to disk.
:::

#### `void di_sync(DiskImage *di);`

::: {.comm}
Writes the image to disk.
:::

### Status

#### `int di_status(DiskImage *di, char *status);`

::: {.comm}
Returns the drive status and copies the message to status.
:::

### File I/O

#### `ImageFile *di_open(DiskImage *di, unsigned char *rawname, FileType type, char *mode);`

::: {.comm}
Open a file for reading or writing. Mode should be either \"rb\" (for
reading) or \"wb\" for writing. If \"\$\" is given instead of the raw
filename, the directory will be read.
:::

#### `void di_close(ImageFile *imgfile);`

::: {.comm}
Close a file.
:::

#### `int di_read(ImageFile *imgfile, unsigned char *buffer, int len);`

::: {.comm}
Read len bytes of data into the buffer. Returns the number of bytes
actually read.
:::

#### `int di_write(ImageFile *imgfile, unsigned char *buffer, int len);`

::: {.comm}
Write len bytes of data from the buffer. Returns the number of bytes
actually written.
:::

### Disk Commands

#### `int di_format(DiskImage *di, unsigned char *rawname, unsigned char *rawid);`

::: {.comm}
Formats the image. If rawid is given, a full format is performed. If
rawid is NULL, a quick format is performed.
:::

#### `int di_delete(DiskImage *di, unsigned char *rawpattern, FileType type);`

::: {.comm}
Deletes files matching the pattern.
:::

#### `int di_rename(DiskImage *di, unsigned char *oldrawname, unsigned char *newrawname, FileType type);`

::: {.comm}
Renames a file.
:::

### Disk Geometry

#### `int di_sectors_per_track(ImageType type, int track);`

::: {.comm}
Returns the number of sectors in a given track.
:::

#### `int di_tracks(ImageType type);`

::: {.comm}
Returns the number of tracks in the image.
:::

### BAM

#### `unsigned char *di_title(DiskImage *di);`

::: {.comm}
Returns a pointer to the disk title and id in the BAM.
:::

#### `int di_track_blocks_free(DiskImage *di, int track);`

::: {.comm}
Returns the number of free sectors in a given track.
:::

#### `int di_is_ts_free(DiskImage *di, TrackSector ts);`

::: {.comm}
Returns non-zero if the given track and sector is free, and zero if
it\'s allocated.
:::

#### `void di_alloc_ts(DiskImage *di, TrackSector ts);`

::: {.comm}
Allocate a given track and sector.
:::

#### `void di_free_ts(DiskImage *di, TrackSector ts);`

::: {.comm}
Free a given track and sector.
:::

### Name Conversion

#### `int di_rawname_from_name(unsigned char *rawname, char *name);`

::: {.comm}
Converts a NULL-terminated string to 16-byte 0xa0 padding. Returns the
length.
:::

#### `int di_name_from_rawname(char *name, unsigned char *rawname);`

::: {.comm}
Converts a 0xa0 padded string to a NULL-terminated string. Returns the
length.
:::

History
-------

 diskimage.c v0.9, 2004-02-12
:   Initial release.

 diskimage.c v0.91, 2004-06-02
:   Bugfix: sector allocation would allocate from directory track.\
    Thanks to Ethan Dicks for finding this one.

 diskimage.c v0.92, 2005-08-06
:   Bugfix: 1541 ignores T/S link in BAM.\
    Thanks to Henrik Jansson for finding the bug.

 diskimage.c v0.93, 2005-11-05
:   Bugfix: 1541 ignores T/S link in BAM. Again.\
    Thanks to Henrik Jansson for finding the bug. Again.

 diskimage.c v0.94, 2005-11-05
:   Bugfix: 1571 also ignores T/S link in BAM.\
    Added support for D64s with error info (but info is ignored).

 diskimage.c v0.95, 2006-04-25
:   Bugfix: di\_load\_image didn\'t close the filehandle.\
    Added file name pattern matching.\
    Added di\_delete and di\_rename.\
    Added preliminary support for DOS status.

Links
-----

### Applications that use diskimage.c

-   [CGTerm](http://www.paradroid.net/cgterm/)
-   [D64scan](http://noname.c64.org/csdb/release/?id=43862)
-   [DiskImagery64](http://www.lallafa.de/blog/)
-   [Image.pm Perl
    module](http://search.cpan.org/~pawelkrol/D64-Disk-Image-0.01/lib/D64/Disk/Image.pm)

License
-------

diskimage.c and the sample code is released under a slightly modified
BSD license. In addition I\'d appreciate if you let me know if you use
it so that I can provide a link above, and that you link back to this
page from your home page.

Copyright (c) 2003-2006, Per Olofsson\
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are
met:

-   Redistributions of source code must retain the above copyright
    notice, this list of conditions and the following disclaimer.
-   Redistributions in binary form must reproduce the above copyright
    notice, this list of conditions and the following disclaimer in the
    documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS \"AS
IS\" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED
TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A
PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER
OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
