# The `truncate` command

The truncate command is used to shrink or extend the size of a file to the given size. The truncate command cannot remove the file whereas removes the contents of the file and set size of file is zero byte. The meaning of truncate is reducing. While reducing the size of the file is the specified size is less than actual size then extra data will be lost.

## Syntax

```
truncate OPTION... FILE...
```

## Options

|**Option**|**Description**|
|:--|:--|
|`-c`, `--no-create`|Do not create any files|
|`-o`, `--io-blocks`|Manage size as number of IO blocks rather than bytes|
|`-r`, `--reference=RFILE`|Set size of file as reference file|
|`-s`, `--size=SIZE`|Set size of the file to SIZE bytes|
|`--help`|Display this help and exit|
|`--version`|Output version information and exit|

When using `truncate` command, the `SIZE` argument has to be an integer and optional unit (example: 10K is 10*1024). The Units as used are `K`, `M`, `G`, `T`, `P`, `E`, `Z`, `Y` (powers of 1024) or `KB`, `MB`,...(as powers of 1000).

The `SIZE` argument may also be prefixed by one of the following modifying characters: `'+'` extend by, `'-'` reduce by, `'<'` at most, `'>'` at least, `'/'` round down to multiple of, `'%'` round up to multiple of.

## Examples:

### Clear contents of a file with truncate

To set the size of the file from actual size to zero and remove all contents of the file, we use the truncate command as shown in below.

```
truncate -s 0 file.txt
```

This is useful e.g for log clearing files. This is better than manually deleting a file and maybe doing a touch for the new one. The truncation process basically removes all the contents of the file. It does not remove the file itself, but it leaves it on the disk as a zero byte file. As an example, let's clear our `/var/log/syslog` to 0 bytes using truncate.

```
[user@home ~]# du -sh /var/log/syslog
4.5M	/var/log/syslog

[user@home ~]# truncate -s 0 /var/log/syslog
```

If we do a check again for the file size, it should be 0 bytes.

```
[user@home ~]# du -sh /var/log/syslog
0 /var/log/syslog
```

Note that `truncate` command will retain file permissions and ownership. You can confirm by using `ls -lh` commands:

```
[user@home ~]# ls -lh /var/log/syslog
-rw-r----- 1 syslog adm 0 Oct 9 18:34 /var/log/syslog
```

### Truncate a file to a specific size

The example below will truncate a file to 100 bytes in size.

```
# create 0 bytes file
[user@home ~]$ touch file.txt

# check the file
[user@home ~]$ ls -lh file.txt 
-rw-r--r-- 1 user user 0 Oct 9 18:39 file.txt

# truncate the file to 100 bytes
[user@home ~]$ truncate -s 100 file.txt
```

Now check the file size again:

```
[user@home ~]$ ls -lh file.txt
-rw-r--r-- 1 user user 100 Oct 9 18:40 file.txt
```

To truncate a file to 100 KB:

```
[user@home ~]$ truncate -s 100K file.txt

[user@home ~]$ ls -lh file.txt 
-rw-r--r-- 1 user user 100K Oct 9 18:41 file.txt
```

The `M`, `G`, `T`, `P`, `E`, `Z`, and `Y` may be used in place of `'K'` as required.

### Extend file size with truncate

You can as well extend the size of the file from current to the desired state. Use the option `-s` with a `+` in size.

```
[user@home ~]$ touch file.txt

[user@home ~]$ truncate -s 100K file.txt

[user@home ~]$ ls -lh file.txt 
-rw-r--r-- 1 user user 100K Oct 10 13:12 file.txt

[user@home ~]$ truncate -s +200K file.txt

[user@home ~]$ ls -lh file.txt 
-rw-r--r-- 1 user user 300K Oct 10 13:12 file.txt
```

This will extend the file size from 100K to 300K by adding extra 200K.

### Reduce file size with truncate

From step above, now we have a `300K` file and would like to shrink it to `250K`. You'll use the `-s` option with a - in size specified. E.g

```
[user@home ~]$ truncate -s -50K file.txt

[user@home ~]$ ls -lh file.txt 
-rw-r--r-- 1 jmutai wheel 250K Oct 10 13:15 file.txt
```

You can see the current size changed to `250K`.

## Conclusion

`Truncate` command is a very useful for removing the content of a file while not deleting the file. You can also change the size of the file to the size you want it to be. We have learned how to truncate the content of a file, as well as how to shrink or extend the files in this topic.