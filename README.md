# simple-batch-backup
Super simple file backup script for Windows.

## Requirements
Windows Server 2003, Vista, Server 2008, 7, Server 2003 R2, 2008 R2, Server 2000, Server 2012, 8, or 10

## Setup

### Destination
Out of the box the script backs up to ```C:\Backup```, which is on your hard drive. Change the ```destination``` variable to where ever you want without a trailing slash. (Don't worry about quoation marks as they're added when the command is called.)

For example, if you wanted to back up directly to the D: drive:
```set desination=D:```

Or to E:\Backup files:
```set destination=E:\Backup files```

### Directories
The script works on pseduo-arrays created as a variable. The ```sour_directories[x]``` pseudo-array is the source path and the ```dest_directories[x]``` is the destination folder (where _x_ is a number generated by the script). Manipulating these correctly, you can add directories to the backup.

### Personal folders
The script backs up the Documents, Downloads, Pictures, Music, and Videos folders of the current user. (Using the ``%USERPROFILE%`` environment variable.)

If you need to back up additional folders in the user directory, edit the ```user_directories``` variable. It is space separated. If your folder has a space, surround it with quotes.

For example, let's add ```AppData\Mozilla Firefox``` to the list:
```set user_directories=Documents Downloads Pictures Music Videos "AppData\Mozilla Firefox"```

If you need to back up additional users, you will need to copy the block of code and edit it in the script. The script will need to be ran by a user with file permissions or as an administrator.

For example, let's add the user John to our list by pasting this a line or two above ```:: to add additional directories:```:
```
for %%a in (%user_directories%) do (
	set dest_directories[!n!]=%%a
	set sour_directories[!n!]=C:\Users\John\%%a
	set /a n = n + 1
)
```
_Note_: You'll need to specify the path correctly!

### Additional folders
You have the option of backing up other non-profile folders. You'll just need to add a couple of lines into the script. 

To backup the source folder ```C:\Program Files\Google Chrome``` into the folder ```Chrome``` on the destination drive, add the following into script after ```::  3. increase the n variable by 1```:
```
set dest_directories[!n!]=Chrome
set sour_directories[!n!]=C:\Program Files\Google Chrome
set /a n = n + 1
```

The last line is very important as it increments the "element" for the pseudo-array. Not incrementing it means you're overwriting the last entry over and over.

## Usage
Just double-click the .bat file to run the backup. If you've added other directories needing special permisison, you can run as an administrator. 
