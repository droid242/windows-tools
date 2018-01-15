# ponderworthy-tools

Some applets for Windows, courtesy of Ponderworthy folks and friends.  Original site is https://notes.ponderworthy.com.
PowerShell 3.0 and later are supported.

## RUNMOST.CMD:  download, verify by hash, and run most of the below

[RUNMOST is a .CMD](https://raw.githubusercontent.com/jebofponderworthy/ponderworthy-tools/master/RUNMOST.CMD) which, if run as administrator, will download, verify integrity by hash, and run OWTAS, OVSS, and then CATE.  It does not run TOSC.ps1, because some enterprises will be using Offline Files.  The result is a distinct performance hike on any current Windows machine.

For compatibility, hashing is done using the command-line CERTUTIL tool (capturing text output to PowerShell code run within CMD), instead of Get-FileHash.  SHA256 is in use.

## RUNALL.CMD:  download, verify by hash, and run all of the below

[RUNALL is a .CMD](https://raw.githubusercontent.com/jebofponderworthy/ponderworthy-tools/master/RUNALL.CMD) which, if run as administrator, will download, verify integrity by hash, and run OWTAS first, OVSS, TOSC, and then CATE.  The result is a distinct performance hike on any current Windows machine.

For compatibility, hashing is done using the command-line CERTUTIL tool (capturing text output to PowerShell code run within CMD), instead of Get-FileHash.  SHA256 is in use.


## CATE: (C)lean (A)ll system and user profile (T)emp folders, (E)tcetera

For quite a while I had been curious as to why a simple method to do this was not available. CCLEANER and others do not reach into every user profile, and on many machines this is crucial, e.g., terminal servers. CATE was originated as a .VBS by the excellent David Barrett ( http://www.cedit.biz ) and has been rewritten thoroughly by yours truly (JEB of Ponderworthy). The current VBS is [here.](https://raw.githubusercontent.com/jebofponderworthy/ponderworthy-tools/master/CATE.vbs)  But [the most recent version](https://raw.githubusercontent.com/jebofponderworthy/ponderworthy-tools/master/CATE.ps1) is a PowerShell script, which adds removal of Ask Partner Network folders from user profiles, and a good bit of speed and clean running; future development will be in PowerShell.

One thing discovered along the way, is even in XP there was a user profile called the “System Profile” — XP had it in C:\WINDOWS\System32\config\systemprofile — and some malware dumps junk into it, and sometimes many gigs of unwanted files can be found in its temporary storage. CATE cleans all user profiles including those, as well as the Windows Error Reporting cache, and the .NET caches, and the system TEMP folders, and in recent versions, many Windows log files which are often found in many thousands of fragments.

The tool is designed for Windows 10 down through XP. As of 2017-10-10, it is self-elevating if run non-administratively.

## OWTAS: Optimize Service Work Items and Additional/Delayed Worker Threads

This tool sets a number of additional critical and delayed worker threads, plus service work items. The changes are autocalculated according to a combination of RAM and OS bit-width (32 vs. 64). Performance will increase, more so with more RAM.  Available as [VBS](https://github.com/jebofponderworthy/ponderworthy-tools/raw/master/OWTAS.VBS) and as [PowerShell](https://github.com/jebofponderworthy/ponderworthy-tools/raw/master/OWTAS.ps1).  Future development will be in PowerShell.

The tool is designed for Windows 10 down through XP. As of 2017-10-10, it is self-elevating if run non-administratively.

## TOSC: Turn Off Share Caching

By default in Windows since XP, if a folder is shared to the network via SMB, so-called "caching" is turned on.  This actually means that the Offline Files service on *other* machines accessing the share, are allowed to retrieve and store copies of files and folders on the machine acting as server.  Turning this off for all shares gives a speed bump for the server machine, and also improves reliability overall, dependence on Offline Files can lead to all sorts of issues including data loss when the server is not available or suddenly becomes available et cetera.

## OVSS:  Optimize VSS

By default, on Windows client OS systems, VSS is active on all VSS-aware volumes, but it is not optimized, which in this case means, there is an "association" or preallocation, of zero space.  On Windows server OS systems, VSS is likewise active, but there is no association/preallocation, at all, on any VSS-aware volumes.  Many different Windows backup tools make the same recommendation concerning this, stating that every volume to be backed up should have 20% of its space "associated" or preallocated for VSS.  OVSS does this, and also, removes all orphan shadows, shadow snapshots existing because of old aborted backups, taking space and drive bandwidth resources but not usable.
