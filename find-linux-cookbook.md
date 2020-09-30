# Linux/Unix find tool cookbook

[Locate a file by a part of its name regardless of the case](#ee1)  
[Find all files that belong to a user and run an ls on them](#ee2)  
[Find all files that belong to a group an dignore all errors](#ee3)  
[Find regular files that are larger than 100 Mb, 100 Kb](#ee4)  
[Find regular files that are smaller than 100 Mb](#ee5)  
[Find files created/modified within the past 24 hours](#ee5)  
[Find files modified between 24 and 48 hours ago](#ee6)  
[Find files modified between 3 and 9 minutes ago](#ee7)  
[Find executable files](#ee8)



## Locate a file by a part of its name regardless of the case
```bash
find / -iname "file*"
```
Note the `" "` around the file globbing pattern - it is needed to prevent the pattern being read by the bash instead of the `find` itself.

## Find all files that belong to a user and run ls on them
We can search by username as well as by user id.   Here I search by the user id of 0, i.e. root-privileged user(s). Also, I add `-type f` to find only regular files, as opposed to device, block etc. files.   

```bash
find / -type f -uid 0 -exec ls -l {} \;
```
Here:  
- `-exec` tells find what command/utility to run on the found files.  
- `ls -l {} \;` tells find to run `ls -l` on each found file. Each found filename is represented by `{}`, in older Unix shells it may be necessary to wrap in escape sequence like that: `\{\}`.   
- `;` ends the command exercution, precede by `\` to prevent bash interpretation.

## Find all files that belong to a group and ignore all errors
```bash
find /  -type f -group apache 2>/dev/null
```


## Find executable files
There few ways to do so.  
```bash
find .  -type f -executable
```
Here we don't care for whom the execute bit is set (owner, group, others) - only whether it is set or not.

We can also limit the directory depth:  
```bash
find . -maxdepth 1 -type f -executable
```

Using familiar octal or symbolic notation for executable file:  

```bash
find . -perm 755
find . -perm /u+x
find . -perm /u+x,g+x
```
