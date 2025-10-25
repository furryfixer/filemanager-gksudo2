# filemanager-gksudo2
### File Manager Context Menu Option to open Directories or Files as Root

Filemanager-gksudo2 is a companion script to **gksudo2**, providing a context menu option from most common file managers in both **Wayland** and **X11**.  Sudo rights, a bash script and .desktop file are required to accomplish this. If your FM(file manager) has "Open as Root" functionality, this script may be a less desirable approach. It may be slightly safer in some situations however. See the gksudo2 Readme about the significant security risks involved, as well as detailed info about gksudo2. Use at your own risk! File managers tested include: **nautilus, thunar, pcmanfm-qt, pcmanfm, dolphin, caja, konqueror, nemo, krusader, spacefm**. See the **CHANGELOG-10-2025** file, as the script has been modified and improved.

## Dependencies
**gksudo2 (and it's dependencies), xdg-utils, glib**   
   
gksudo2 can be found here:   https://github.com/furryfixer/gksudo2

The script is fairly universal, and works on most common Desktops, providing "Open-as-Root" functionality for BOTH files and directories. A warning notification will display. Common failure modes use Zenity to notify the Desktop of the error. There are some "guard rails" to prevent accidental disasters when opening files (but not deliberate misuse).  These safety features include:

- Refuses to use anything other than the default app for text/plain mimetype to open a file as root.
- Refuses to open binary executable files at all.
- Refuses to open sockets, or block or character devices
- Refuses to follow symlinks for files and directories.
- Insists on password for root instance of filemanager, regardless of sudo credentials

Of course, once a root instance of a file manager is opened, these safety features are bypassed, and more bad consequences are possible.

## Installation

Download or clone the files. Ensure that $PATH includes /usr/local/bin, unless placing executable in /bin or /usr/bin instead. From the download directory, do the following AS ROOT:

- cp filemanager-gksudo2 /usr/local/bin/filemanager-gksudo2
- chmod +x /usr/local/bin/filemanager-gksudo2
- chmod +x fmgks2-user-setup

Then within the download directory, as regular user (NOT ROOT!):

- ./fmgks2-user-setup

Dolphin users may require a symlink for proper integration of mimetype defaults:
  
  **sudo ln -s /etc/xdg/menus/plasma-applications.menu  /etc/xdg/menus/applications.menu**
  
Reboot, or at least restart your desktop session.

## Notes:

The option showing in context menus will be "**gksudo2 (open as ROOT)**".  For all files, the default app for text/plain mime-types will be used. Once a directory is opened as root, the root user opens files as normal without restrictions (be careful).  

In order for filemanager-gksudo2 to appear in context menus, it must advertise itself as an option for many mimetypes. The goal is to have gksudo appear as an "open with" or context menu option, but prevent it from being be the default for opening any file or directory. If no default exists for a file association advertised by filemanager-gksudo2.desktop, the default app for the text/plain mimetype will be set as default to accomplish this. If, despite this, the script detects that it is a default app, it will warn the user and refuse to run until the default is changed.

The calling file manager will be used to open directories if it can be determined.  Otherwise, the script tries to use the most recent file manager opened if more than one, or falls back to the system default for the inode/directory mimetype. Please DO NOT install the .desktop file in the system location, where any new user will be tempted to use it (even though it requires sudo rights).  If more than one file/directory is selected, only the first will be acted on. For files, a "NOPASSWD" sudoer configuration is honored, but cached sudo credentials are not honored. For directories, "NOPASSWD" is ignored, and a password will always be required.
 
 
