# Bandit Writeup

## Level 0

**Level Goal:**  
Log into the game using SSH.

**Connection Details:**  
- Host: `bandit.labs.overthewire.org`  
- Port: `2220`  
- Username: `bandit0`  
- Password: `bandit0`

**Command to connect:**
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

**Explanation:**
- `bandit0@` → the username.
- `bandit.labs.overthewire.org` → the host we are connecting to.
- `-p 2220` → specifies the port for SSH (2220).

Once connected, we get a shell:
```bash
bandit0@bandit:~$
```

List the files in the home directory:
```bash
ls
```
Output:
```
readme
```

We found a file named `readme`. To see its content, we use `cat`:
```bash
cat readme
```
Output:
```
Congratulations on your first steps into the bandit game!!
Please make sure you have read the rules at https://overthewire.org/rules/
...
The password you are looking for is: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

---

## Level 1 → 2

**Level Goal:**  
Use the password from Level 0 to log into Level 1 via SSH.

**Connect using SSH:**
```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
```
Password: `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

Once logged in:
```bash
bandit1@bandit:~$ ls
-
```

### Handling a file named `-`

The file in this directory is literally named `-`, which is a **special character** in Linux. Many commands like:
```bash
cat -
cat \-
cat '-'
```
may **fail or hang**, because `-` is commonly interpreted as a placeholder for **stdin** or **stdout**, not as a filename.

**Correct way to read the file:**
```bash
cat ./-
```
- `./` specifies the current directory and ensures the shell treats `-` as a **literal filename**.

Output:
```
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

This reveals the **password for Level 2**.

---

## Level 2 → 3

**Level Goal:**  
The password for the next level is stored in a file called `--spaces in this filename--` located in the home directory.

```bash
bandit2@bandit:~$ ls
--spaces in this filename--
```

To read it, we must handle a file starting with `--` and containing spaces. Linux interprets `--` at the start of a filename as a long option, which is why naive commands fail.

**Correct ways to read the file:**
```bash
cat ./--spaces\ in\ this\ filename--
```
or
```bash
cat -- --spaces\ in\ this\ filename--
```

Output:
```
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

**Tip for future levels:**
- Files starting with `-` or `--` may be treated as command options. Use `./` to indicate the current directory, or use `--` before the filename to stop option parsing.
- Escape spaces with `\ ` or use quotes to handle filenames with spaces.

This reveals the **password for Level 3**.

## Level 3 → 4

**Level Goal**
The password for the next level is stored in a hidden file in the inhere directory.

bandit3@bandit:~$ ls
inhere

we found a folder called inhere so we go to it

bandit3@bandit:~$ cd inhere

bandit3@bandit:~/inhere$ ls

ls returned no output because hidden files are not shown by default.
We use ls -la to list all files (including hidden ones) along with their permissions, owner, group, and timestamps. and we found the file called ...Hiding-From-You

bandit3@bandit:~/inhere$ ls -la
total 12
drwxr-xr-x 2 root    root    4096 Oct 14 09:26 .
drwxr-xr-x 3 root    root    4096 Oct 14 09:26 ..
-rw-r----- 1 bandit4 bandit3   33 Oct 14 09:26 ...Hiding-From-You
so we try to read it to get the password for the next level
bandit3@bandit:~/inhere$ cat ...Hiding-From-You

Output:

```2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ```

## Level 4 -> 5

**Level Goal**
The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.

so we connect and find the inhere dir and the ls all the content of it and found multiple directories so we use file command. File command returns the type of data that is found in the file
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
bandit4@bandit:~/inhere$ file ./*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
The * here means search all files in the directory. For more information on file globbing refer the attached reference resources

``bandit4@bandit:~/inhere$ cat ./-file07``
```4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw```

## Level 5 -> 6

**Level Goal**
The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

    human-readable
    1033 bytes in size
    not executable

bandit5@bandit:~/inhere$ ls
maybehere00  maybehere02  maybehere04  maybehere06  maybehere08  maybehere10  maybehere12  maybehere14  maybehere16  maybehere18
maybehere01  maybehere03  maybehere05  maybehere07  maybehere09  maybehere11  maybehere13  maybehere15  maybehere17  maybehere19

bandit5@bandit:~/inhere$ find . -type f -size 1033c ! -executable -readable

. → start searching in the current directory (~/inhere)
-type f → only look for files
-size 1033c → files that are exactly 1033 bytes (c = bytes)
! -executable → not executable
-readable → the file has read permissions for the current user

./maybehere07/.file2
bandit5@bandit:~/inhere$ cat maybehere07/.file2
```HWasnPhtq9AVKe0dmk45nxy20cvUa6EG```
                                  
## Level 6 -> 7

**Level Goal**
The password for the next level is stored somewhere on the server and has all of the following properties:

    owned by user bandit7
    owned by group bandit6
    33 bytes in size

bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c -type f 2>/dev/null

find / -user bandit7 ...
-type f → regular files only
-size 33c → 33 bytes (c = bytes)
-user bandit7 -group bandit6 → owned by user bandit7 and group bandit6

2>/dev/null → hides all permission denied errors
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
```morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj```

## Level 7 -> 8

**Level Goal**

The password for the next level is stored in the file data.txt next to the word millionth

bandit7@bandit:~$ ls
data.txt
bandit7@bandit:~$ grep "millionth" data.txt
millionth       ```dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc```

## Level 8 -> 9

**Level Goal**
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

bandit8@bandit:~$ sort data.txt | uniq -u
```4CKMh1JI91bUIZZPXDqGanal4xvAg0JM```
uniq is a command that filters input and writes to the output. Specifically, it filters based on identical lines.

## Level 9 -> 10

**Level Goal**
The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

bandit9@bandit:~$ strings data.txt | grep ===
========== the
========== password
E========== is
5========== ```FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey```

The strings command finds human readable strings in files and then next we want to filter the output of the strings to see the lines that includes more than one '=' so we use grep and put the pattern we want to grep since the description said several = so atleast we put 3 equals and we got the password

## Level 10 -> 11

**Level Goal**
The password for the next level is stored in the file data.txt, which contains base64 encoded data

bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==

so we use the base64 command with the flag -d to decode the file and we got the password

bandit10@bandit:~$ base64 -d data.txt 
The password is ```dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr```