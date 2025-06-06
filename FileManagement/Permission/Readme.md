```
ls -l
```
![image](https://github.com/user-attachments/assets/82e68b2e-da19-4d8f-89e5-db94149d7f12)

### Umask vs chnmod
- umask Sets the default permission mask that determines the permissions of files and directories when they are created. It does not affect existing files and directories.
- chmod (change mode) is used to change the permissions of existing files and directories. It does not affect the permissions of files and directories that will be created in the future

--------------------------------------------------------------------------------------------------
### umask
- umask subtracts from the default system permissions, which are usually 666 for files (rw-rw-rw-) and 777 for directories (rwxrwxrwx). Thus, umask 022 causes new files to have permissions of 644 (rw-r–r–) and new directories to have permissions of 755 (rwxr-xr-x).
- Changes to umask only affect the current shell session and do not persist after exit unless you include commands to set these permissions in a system startup file such as /etc/bash.bashrc or .profile.
![image](https://github.com/user-attachments/assets/35d20ecc-09ce-431e-b4f7-b3de105ec615)

change umask permanetly:
- default 022
```
echo 'umask 027' >> ~/.bashrc
source ~/.bashrc
```
change umask temorary :
```
umask [-p] [-S] 
```
example:
```
umask 000
umask -S u=rwx,g=rwx,o=rwx

umask 111
umask -S u=rw,g=rw,o=rw

umask 222
umask -S u=rx,g=rx,o=rx

umask 444
umask -S u=wx,g=wx,o=wx
```

show 
- default umask (0022 OR u=rwx,g=rx,o=rx)
```
umask
umask -S
```


### Chmod
sysntex:
```
chmod 1-7,1-7,1-7
chmod u=rwx,g=rwx,o=rwx
```
```
chmod 777 new_file.txt
chmod u=rwx,og=r new_file.txt
chmod a+x new_script.sh
```
Recursively Change
```
chmod +x -R ./testdir
```
----------------------------------------------------------------------------------------------
- u: User, meaning the owner of the file.
- g: Group, meaning members of the group the file belongs to.
- o: Others, meaning people not governed by the u and g permissions.
- a: All, meaning all of the above.

-----------------------------------------------------------------------------------------------
-  -: Minus sign. Removes the permission.
-  +: Plus sign. Grants the permission. The permission is added to the existing permissions. If you want to have this permission and only this permission set, use the = option, described below.
-  =: Equals sign. Set a permission and remove others.

-----------------------------------------------------------------------------------------------

- r: The read permission.
- w: The write permission.
- x: The execute permission.

-----------------------------------------------------------------------------------------------

- 0: (000) No permission.
- 1: (001) Execute permission.
- 2: (010) Write permission.
- 3: (011) Write and execute permissions.
- 4: (100) Read permission.
- 5: (101) Read and execute permissions.
- 6: (110) Read and write permissions.
- 7: (111) Read, write, and execute permissions.



### Chown
```
chown [OPTIONS] USER[:GROUP] FILE(s)
```

```
sudo chown :root example.txt
sudo chown root: example.txt
sudo chown root:root example.txt
```

Recursively Change

```
sudo chown -R root:root ./testdir
```


