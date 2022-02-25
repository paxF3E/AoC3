# Wargames :: Bandit

## Level 0
Command : `ssh bandit0@bandit.labs.overthewire.org -p 2220` using password `bandit0` to ssh into level0

Usage : `ssh <USERNAME>@<HOST_IP/DOMAIN>` with a `-p` flag here to specify the port number 

`ls` to list the available files, `cat` to read the _readme_ file

Password : `boJ9jbbUNNfktd78OOpsqOltutMc3MY1`

## Level 1
Command : `cat <PATH>/<FILE_NAME>` since, filename was `-`, have to distinguish it from flag, thus along with PATH

Password : `CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9`

## Level 2
Command : `cat "FILE_NAME"` since file name had space characters, enclosing in `" "` will do 

Password : `UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK`

## Level 3
Command : `ls -a` to list all the contents of the directory, since the file was a hidden one `.<FILE_NAME>`, the period prefix denotes a hidden file/dir

Password : `pIwrPrtPN36QITSp3EQaw936yaFoFgAB`

## Level 4
Command : there are 10 files in `inhere/` directory, `cat <FILENAME>` to check for the UTF-8 encoded, human readable characters for the password to next level

Password : `koReBOKuIDDepwhWk7jZC0RTdopnAYKh`

## Level 5
Command : there are too many directories, one of which would have an unexecutable file of size 1033 bytes <br>
thus using `find . -size 1033c`; `.` for specifying the path where to look; `1033c` to specify the 1033 bytes of file size

Password : `DXjZPULLxYr17uwoI01bNLQbtFemEgo7`

## Level 6
Command : `find / -type f -size 33c -group bandit6 -user bandit7`; `/` for searching on complete server; `-group` to define the owner group; `-user` to define the owner of the file

Password : `HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs`

## Level 7

