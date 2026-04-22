# Assignment 6 Write-up

## What I did

**New inode type** — Added `T_SYMLINK 4` to `stat.h`.

**New syscall** — `symlink(target, linkpath)` creates a `T_SYMLINK` inode at
`linkpath` and writes the target string into it using `writei()`

**Resolution** — In `sys_open`, after looking up the path, I added a loop that
follows symlinks by reading the target with `readi()` and looking it up again.
The loop stops after 10 hops to handle cycles

`testsymlink` checks three things:
- Creating a symlink succeeds
- Reading through a symlink returns the target file's contents
- A cycle causes `open` to fail
