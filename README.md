## Installation
1. Copy the EXEC members into a library in your SYSPROC or SYSEXEC allocation.
2. Copy the PANELS members into a library in your ISPPLIB allocation.

## Dependencies
 - This ISPF dialog utilizes the STEMEDIT utility which can be found on the [CBTTape](https://www.cbttape.org/) FILE 183.
 - Some elements of the dialog require the user to have SuperUser capability.
 - The zLSOF option on the menu requires access to SYS1.SBPXEXEC where IBM ships this exec.

## Overview
zFSTools is an ISPF dialog that combines home grown and IBM dialogs to
assist and simplify the management of zFS datasets. With this dialog you
can backup a zFS dataset to a unloaded sequential dataset, restore a zFS
dataset from a backup, allocate a new zFS, grow the size of a zFS, access
many of the zfsadm functions (grow is one of them), mount and unmount
filesystems, and even rename a zFS dataset. All with comparitive ease thanks
to the ISPF menus and panels.

![image](https://github.com/lbdyck/zfstools/assets/42328411/cd62fe87-901b-4e35-a193-5a95f30bfda0)

Some of the functions require that the user have the ability to become
a SuperUser and if you have that ability then those functions (e.g. mount,
unmount, grow) will work very well. If not, then find someone who can do
it for you.

The backup and restore functions work with any unmounted zFS dataset and
support using a GDG for the backup dataset if desired using relative GDG
numbers.

The dialog is accessed by starting with the ISPF Panel ZFSMENU.

To simplfy use you can edit the ZFSTOOLS exec to point to the install
datasets (exec and panels) and then install just the ZFSTOOLS exec into
an allocated library in your SYSPROC or SYSEXEC concatenation.

Some of the REXX exec's included are:
* ZFSADM   - Perform several of the zfsadm functions
* ZFSALLOC - To allocate and format a new zFS (as a type 1.5)
* ZFSBACK  - To backup a zFS to a sequential dataset
  * uses IDCAMS REPRO
  * make backup to a gdg
  * Note that if the zFS to be backed up is mounted then the code, using the sysdsn() function, gets a dataset not found result. Strange but it does prevent an attempt to backup an active zFS.
* ZFSCOPY  - Generate ADRDSSU JCL to copy a zFS
* ZFSGROW  - Increase the size of a mounted zFS (requires SU authority)
* ZFSIBM   - Exec to access some of the IBM zFS dialogs
  * Mount Table by File System
  * Mount Table by Mount Point
    * not supported prior to z/OS 2.2
* ZFSMENU  - Simple exec to display the ZFSTools Menu Panel
* ZFSMNT   - Mount a file system using the IBM Mount dialog
  * If z/OS 2.1 then use custom dialog
* ZFSRENAM - Rename the zFS base and data datasets
* ZFSREST  - To restore a zFS unloaded dataset
  * uses IDCAMS REPRO
  * can create a new zFS
  * can delete a current zFS before allocating a new zFS and reloading
  * can restore from a gdg
* ZFSRMNT  - Remount a zFS changing from RO to RW and vice versa
* ZFSSHRK  - Decreases the size of a mounted zFS
* ZFSSIZER - Exec to prompt to size a zFS from a directory or by MB's
* ZFSTOOLS - Exec to altlib and libdef for this application if you don't install in ISPF allocated libraries
* ZFSUMNT  - UnMount using the IBM Mount Table by File System
  * If z/OS 2.1 then use custom dialog
* ZFSUSE   - Display information about zFS usage

## Acknowledgements
- Ed Jaffe for how to use bpxwunix
- Paul from IBM z/OS USS Shell and Utilities for how to set SuperUser mode using syscall
- Peter Giles for several usability updates
- Patrick Loftus added zfsadm shrink option
