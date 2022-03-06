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
Command : `openssl s_client -connect localhost -port 30001`; connecting over secure _openssl_ connection on _localhost_ on port `30001`

Password : `cluFn7wTiGryunymYOu4RcffSxQluehd`

## Level 16
Command : `nmap -sT localhost -p 31000-32000`; scanning all active ports in `[31000, 32000]` <br>
`openssl s_client -connect localhost -port PORT_NUMBER`; using for all the returned active ports from prev command to check one with _ssl_ compatibility <br>
only one responds to passed info with valid creds for next level (-port 31790)

Password : `-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----`

## Level 17
run `touch key.private`; edit the `key.private` and store the _private_key_ from last level; make it private using `chmod 600 key.private`; then login to ssh session using `-i` flag to include the key containing file 

Command : `diff password.new password.old`; compare the files line by line <br>
returns two strings; running `grep <STRING_1> password.new` will confirm which is the key for next level in _password.new_ file

Password : `kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd`

## Level 18
Command : `ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme`; since, we are getting logged out again again, running `cat` with ssh login will get the pass

Password : `IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x`

## Level 19
execute the bin file `./bandit20-do` for hint
Command : `./bandit20-do cat /etc/bandit_pass/bandit20`; _bandit_pass/bandit20_ is denied for _bandit19_, but can be accessed as _bandit20_ user

Password : `GbKksEFF4yrVs6il55v6gwY5aVje5f0j`

## Level 20
Command : `nc -l -p 10000`; on another session - `./suconnect 10000`; on original session, send the password this level, to get pass for next level <br>
`nc -l -p 10000` opens up a port with a listener, connecting to that port from another session verifies the pass and returns new pass to original session

Password : `gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr`

## Level 21
look for jobs in `/etc/cron.d` for level 22; `cronjob_bandit22.sh` in `/usr/bin/` reveals that the output is being written to some other location <br>
run `cat /usr/bin/cronjob_bandit22.sh`; and read the output file as mentioned

Password : `Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI`

## Level 22

