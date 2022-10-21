# filemanager-gksudo2
### File Manager Context Menu Option to open Directories or Files as Root

Filemanager-gksudo2 is a companion script to **gksudo2**, providing a context menu option from most common file managers in both **Wayland** and **X11**.  Sudo rights, a bash script and .desktop file are required to accomplish this. If your FM easily allows custom actions to be defined with gksudo2 (Thunar, for example), or if it has "Open as Root" 	functionality, this script will be a less efficient approach. It may be slightly safer in some situations however. See the gksudo2 Readme about the significant security risks involved, as well as detailed info about gksudo2. Use at your own risk! File managers tested include: **nautilus|thunar|pcmanfm-qt|pcmanfm|dolphin|caja|konqueror|nemo|krusader|spacefm**. 

## Dependencies
**gksudo2, gksudo-su (and their dependencies), xdg-utils, glib**

The script is fairly universal, and works on most common Desktops, providing "Open-as-Root" functionality for BOTH files and directories. A warning notification will display. Common failure modes use Zenity to notify the Desktop of the error. There are some "guard rails" to prevent accidental disasters when opening files (but not deliberate misuse).  These safety features include:

- Refuses to use anything other than the default app for text/plain mimetype to open a file as root.
- Refuses to open most binary executable files at all.
- Refuses to open sockets, or block or character devices
- Refuses to follow symlinks for files and directories.
- Insists on password for root instance of filemanager, regardless of sudo credentials

Of course, if a root instance of a file manager is opened, these safety features are bypassed, and more bad consequences are possible.

In order for filemanager-gksudo2 to appear in context menus, it must advertise itself as an option for many mimetypes.  Unfortunately, it will become the default app for a mimetype if no default app is already set for the user.  The workaround for this is that the script will generally refuse to proceed if it detects this, and the user will be asked to set another default app for that mimetype. The script will then still appear as a secondary option to open the file or directory.

## Installation

Download or clone the files. Ensure that $PATH includes /usr/local/bin, unless placing executable in /bin or /usr/bin instead. From the download directory, do the following AS ROOT:

- cp filemanager-gksudo2 /usr/local/bin/filemanager-gksudo2
- chmod +x /usr/local/bin/filemanager-gksudo2

Then as regular user (NOT ROOT!):

- cp filemanager-gksudo2.desktop  ~/.local/share/applications/filemanager-gksudo2.desktop
- update-desktop-database ~/.local/share/applications

You may need to restart your desktop session.

## Notes:

The option showing in context menus will be "**gksudo2 (open as ROOT)**".  For all files, the default app for text/plain mime-types will be used. Once a directory is opened as root, the root user opens files as normal without restrictions (be careful).  The calling file manager will be used to open directories if it can be determined.  Otherwise, the script tries to guess which file manager to use, or falls back to the system default for the inode/directory mimetype. Please DO NOT install the .desktop file in the system location, where any new user will be tempted to use it (even though it requires sudo rights).  If more than one file/directory is selected, only the first will be acted on. For files, a "NOPASSWD" sudoer configuration is honored, but caching sudo credentials is not honored. For directories, "NOPASSWD" is ignored, and a password will always be required.
 
