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
Command : `grep <PHRASE> <FILE_NAME>`

Password : `cvX2JJa4CFALtqS87jk27qwqGhBM9plV`

## Level 8
Command : `sort <FILE_NAME> | uniq -u` <br>
`uniq` returns unique strings in succession, `sort` is required to place all duplicates together, then perform `uniq` to get the single unique keyphrase

Password : `UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR`

## Level 9
Command : `strings data.txt | grep "="` <br>
`strings` prints the strings for printable characters, then searching for those strings with `=` in them

Password : `truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk`

## Level 10
Command : `base64 -d <FILE_NAME>`; `-d` flag to specify the decode option

Password : `IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR`

## Level 11
Command : `cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'`; reading the content of file, and pipelining it to ROT13 decode command 

Password : `5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu`

## Level 12
Repeatedly compressed password file; <br>
Trick is: 
  - copy to a location, with access to make files `cp <SOURCE_PATH> <DEST_PATH>`
  - reverse the hexdump to a extension less file `xxd -r <FILE_NAME> > <NEW_FILE>`
  - check for the type using `file <NEW_FILE>`; returns the file was a _gzip_ compressed data
  - rename it to give a _.gz_ extension using `mv data data.gz`
  - decompress it to get a new file
  - run `file <RESULT_FILE>` to get its type, change the extension as per need, decompress it again using suitable commands untill the result file is a `ASCII` one

Password : `8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL`

## Level 13
Command : `ssh bandit14@localhost -i sshkey.private`; accessing _bandit14_ over _localhost_ with _sshkey.private_ auth using `-i` flag

Password : `4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e`

## Level 14
Command : `nc -l 30000`; to listen on `port 30000`; <br>
`echo "4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e" | nc localhost 30000`; sending data to _localhost_ on port number `30000` using _netcat nc_ tool

Password : `BfMYroe26WYalil77FoDi9qh59eK5xNr`

## Level 15
Command : 
