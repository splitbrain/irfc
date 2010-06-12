IR Filechooser
==============

WHAT IS IT?
-----------

The Infra Red File Chooser (or short irfc) is a remote controlled menu for
selecting files and loading them by an assigned program. It can be used on
standalone video/mp3 players for navigating through a list of files.
You can add as much filetypes and assoziated programs as you want. By using
Perl or Shell scripts you can extend it's usefulness much more.
The GUI is created with Perl::GTK and it uses RCU::Lirc to fetch the remote
controllers commands.


INSTALLATION
------------

irfc needs some perl modules to run. First you need the Gtk module Debian
users can invoke 'apt-get install libgtk-perl' to install it others should
install the right RPM or get it direct from CPAN.

For the remote control the RCU module suite is needed get it from CPAN by
using

    perl -MCPAN -e shell
    install Event
    install RCU

The RCU module needs Time::Hires to work, too. Again Debian users can use

    apt-get install libtime-hires-perl

Put the irfc to a place you like it (eg. your home directory).<BR>
If you like you can add irfc to your .xsession or .xinit to start it when
the X server comes up.

CONFIGURATION
-------------

Options

Open the irfc script with your favourite editor and change the configuration
variables at top of it.

    $MENUDIR

Set this to the full path of your directory structure (See next Chapter)

    $CDDRIVE[0]
    $CDDRIVE[1]

You can define which directories inside your menu structure act as mountpoints
for CD-ROM drives. Those will be mounted before shown by irfc and unmounted
when the directory is exited. You have to add this path as a usermountable
device to your `/etc/fstab`.

    %FILETYPES

This is the most important part. Here you cann assign programs to fileextensions.
For example if you want to play all mp3 files with xmms you have to add
`mp3  => "xmms"` to this hash. For more complex commands you can use the
`§f` placeholder, which will be replaced by the selected filename.

Note: unknownfiletypes will not be displayed in the Filechooser

%BUTTONS

This hash configures your remote control buttons. Assign the button names as
defined in your `lircd.conf` to the builtin IR Filechooser commands.

These Commands are:

* `[down]` Moves the selection in the list one file down.
* `[up]` Moves the selection in the list one file up.
* `[enter]` Runs the associated program with the selected file.
* `[fast_up]` Moves the selection in the list five files up.
* `[fast_down]` Moves the selection in the list five files down.
* `[back]` Goes up one directory (It's like selecting [..] in the list and pressing `[enter]`)

You can run other programs by assigning buttons, too. For example if you want
to run xmms by pressing the button named `1` simply add `"1"  => "xmms"` to
the hash.

    $DROPREPEATS

This slows down the reaction on repeated button presses. For example setting
it to `3` only every third repeation of a keypress is recognized.

    @WINDOWSIZE

This sets the size of the Window (x,y)

    $FONT

This changes the used font of the listbox. To get a valid fontname use
the program `xfontsel`

    $USE_POLL

Setting this to 1 will use the polling method of RCU which consumes a lot of
CPU time (up to 100%). This is no longer needed as IRFC can now use the event
based interface of RCU so leave this setting at 0.

    $SHOW_EXT

Set this to 1 if you want to see the filetypes of the files in the list.


Menustructure

Create the directory you named in`$MENUDIR` and place your files in it.

Note: You can use symbolic links to create the wanted menu structure.

Hidden files (beginning with a dot) are not shown in irfc and underscores (_)
are converted to blanks.

example:

    $ ls -l menu/
    total 17
    drwxr-xr-x    2 mm       mm             35 Jan 14 12:41 CD
    drwxr-xr-x    4 mm       mm             79 Jan  5 16:50 Games
    lrwxrwxrwx    1 mm       mm             11 Dec 29 16:18 Movies -&gt; /ftp/moviez
    -rw-r--r--    1 mm       mm             34 Dec 29 21:54 Play_MP3s.sh
    -rw-r--r--    1 mm       mm             68 Jan  5 18:27 Rip_Audio_CD.sh
    -rw-r--r--    1 mm       mm             11 Dec 30 01:49 Shut_Down.sh

As you can see I used some scripts to start more complex programs. This can
be done by assigning the script extensions to their appropriate interpreter:

    %FILETYPES=( sh   => "bash",
                 pl   => "perl");

USING IT
--------

Using it is easy: Simply use the defined buttons on your remote control to
navigate throug the menu. Subdirectories are marked by []. Selecting a
directory changes to it and selecting a file runs the assigned program on it.
You can change to an upper directory by selecting the [..] entry.

FEEDBACK
--------

Yes please! Add Ideas, Bugreports and Patches in the issue tracker at
http://github.com/splitbrain/irfc/issues

THANKS
------

* Frank Schubert <frank@schokilade.de> for fixing the droprepeats function
* "Da Kourier" <dakourier@armitage.gotdns.com> for a hint how to change the fonts
* Kees Cook <kees@outflux.net> for
    - Event-based lirc (to not take 100% CPU)
    - Added "Back" command to move up in the directory tree
    - Small GTK clean-ups to reduce the size of unused space in the status window
    - Cleaned up GTK clist
    - Added file extension visibility

LICENSE
-------

irfc - IR File Chooser
Copyright (C) 2001,2002 Andreas Gohr <andi@splitbrain.org>

This program is free software; you can redistribute it and/or
modify it under the terms of the GNU General Public License as
published by the Free Software Foundation; either version 2 of
the License, or (at your option) any later version.

See COPYING for details

