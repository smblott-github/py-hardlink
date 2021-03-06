py-hardlink
===========

A python script to hard-link together duplicate files.  In other words, file system de-duplication.

**Problem**:
a large file system (often containing videos or music) with multiple copies of
the same file.

**Solution**:
replace duplicate copies with hard links to a single primary copy.

Many scripts for solving this problem already exist.  However, I've had bad
experiences with some of them.  They might be slow -- which is not too
bad, really. But they might also delete the wrong files leaving you with no
backup.

So I decided to write my own de-duplicator.  At least, that way, I only have
myself to blame.

## Approach

Group files by:

* size
* the file system mount point they're on
* MD5 hash
* SHA256 hash

Only consider files of at least 1024 bytes.

If a pair of files pass those tests, then it is vanishingly improbable that
they aren't the same file.

Of candidate files, the one chosen to survive is that one with the earliest
modification time.  So any resulting file has the attributes (permissions,
timestamps, and so on) of that file.

**Optimisation**: within a single file system, `py-hardlink` only considers a
single exemplar for each inode.  This considerably speeds things up,
particularly if the directory hierarchy already contains many files which are
hard linked together.

## Requirements

Python 2.6 or 2.7 seem to work.  Python 3 does not (syntax error).  I haven't
looked into why.

