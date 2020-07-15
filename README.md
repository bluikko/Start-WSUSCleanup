# Start-WSUSCleanup
WSUS periodic maintenance script.

WSUS requires periodic maintenance. If the maintenance is not done, the database tables grow too large and WSUS will stop working with various errors.
Cleaning the database becames difficult if the database is left without maintenance.

This script does the following steps:
* Decline expired updates (executing stored procedure `spDeclineExpiredUpdates`).
* Decline superseded updates (executing stored procedure `spDeclineSupersededUpdates`).
* Delete obsolete updates (executing stored procedure `spGetObsoleteUpdatesToCleanup` and then executing `spDeleteUpdate` for each of the resulting update IDs).
* Delete obsolete computers (executing stored procedure `spCleanupObsoleteComputers`).
* Restart WSUS (stop IIS, restart WsusService, start IIS).
* Delete unneeded content files (with PowerShell `Invoke-WsusServerCleanup -CleanupUnneededContentFiles`).

A million such scripts exist already. The purpose of this script is to pick the best parts of such scripts and make it work for me (and hopefully to others).

Story time!
```
There was once a good WSUS maintenance script. Many people, including Microsoft
employees with insider information about how WSUS works and detailed knowledge
of the SUSDB database, helped to improve the script. Over a long period of time
the script became the ultimate WSUS maintenance script. Then the maintainer of
the script decided to start charging monthly fee for access to the script and
removed it from everywhere. The maintainer even went so far as to issue
DMCA/copyright takedown notices for publishing the script, it is said that
lawsuit threats were also made.

It is understandable that the maintainer wants to get something in return for
the script. But in the end he was only just that - a maintainer - and the
script was created mostly by other people. Naively these other people just
donated their "IP" to the script without clear license.

These days, almost all searches for WSUS problems or maintenance will result
in references to the script. But the script itself has been removed.
And, obnoxiously, the maintainer of the script replies to any WSUS problem with
an advertisement for the paid script.
```

## TODO
* Add switches to check/create additional indexes in SUSDB to make large/unmaintained WSUS not unbelievably slow.
* Rebuild fragmented indices.
* Add switch for test mode/WhatIf.
* Delete old history entries.
