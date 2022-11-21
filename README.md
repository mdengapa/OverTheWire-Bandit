# OverTheWire-Bandit
Tutorial sobre OverTheWire - Wargame: bandit

bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat $(pwd)/-
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat spaces\ in\ this\ filename 
aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG
bandit2@bandit:~$ 

bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ ls inhere/ -a
.  ..  .hidden
bandit3@bandit:~$ cat inhere/.hidden 
2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe
bandit3@bandit:~$

Bandit Level 3 → Level 4
The password for the next level is stored in a hidden file in the inhere directory.

Bandit Level 4 → Level 5
The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.
bandit4@bandit:~$ time find . -name -file* | xargs file
bandit4@bandit:~$ time file inhere/*
inhere/-file00: OpenPGP Public Key
inhere/-file01: data
inhere/-file07: ASCII text
real    0m0.018s
user    0m0.009s
sys     0m0.008s
bandit4@bandit:~$ find . -name -fiel07 | xargs cat
bandit4@bandit:~$ cat $(find . -name -file07)
bandit4@bandit:~$ cat inhere/-file07
lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR

Bandit Level 5 → Level 6
The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:
  • human-readable
  • 1033 bytes in size
  • not executable
bandit5@bandit:~$ find . -type f -readable ! -executable -size 1033c | xargs cat | xargs
bandit5@bandit:~$ find . -type f -readable ! -executable -size 1033c | xargs cat | sed 's/^ *//'
bandit5@bandit:~$ find . -type f -readable ! -executable -size 1033c | xargs cat | sed '/^\s*$/d'
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU

*** Busca coincidencias que empiecen por hola y terminen en a. Muestra la linea donde se encuentra ***
cat /etc/passwd | grep “^hola$” -n
*** Muestra la primera linea ***
$ cat /etc/passwd | head -n 1
$ cat /etc/passwd | awk ‘NR==1’
*** Muestra la segunda linea ***
$ cat /etc/passwd | head -n 2
$ cat /etc/passwd | awk ‘NR==2’
*** Muestra la ultima linea ***
$ cat /etc/passwd | tail -n 1
*** Sustituye en todas las ocurrencias ***

Bandit Level 6 → Level 7
The password for the next level is stored somewhere on the server and has all of the following properties:
  • owned by user bandit7
  • owned by group bandit6
  • 33 bytes in size
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c 2>/dev/null | xargs cat
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S

Bandit Level 7 → Level 8
The password for the next level is stored in the file data.txt next to the word millionth
bandit7@bandit:~$ grep "millionth" data.txt
millionth       TESKZC0XvTetK0S9xNwm25STk5iWrBvP
bandit7@bandit:~$ awk '/millionth/' data.txt | awk '{print $1
bandit7@bandit:~$ awk '/millionth/' data.txt | awk 'NF{print $NF}'
bandit7@bandit:~$ awk '/millionth/' data.txt | rev | awk '{print $1}' | rev
 TESKZC0XvTetK0S9xNwm25STk5iWrBvP

Bandit Level 8 → Level 9
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once
bandit8@bandit:~$ cat data.txt | wc -l
1001
bandit8@bandit:~$ cat data.txt | sort | uniq -u
EN632PlfYiZbn3PhVK3XOGSlNInNE00t

Bandit Level 9 → Level 10
The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.
bandit9@bandit:~$ contador=1; strings data.txt | grep "===" | while read linea; do echo "$contador: $linea" ; let contador=+1; done | awk 'NR==4' | awk 'NF{print $NF}'
G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s

Bandit Level 10 → Level 11
The password for the next level is stored in the file data.txt, which contains base64 encoded data
bandit10@bandit:~$ cat data.txt | base64 -d | awk 'NF{print $NF}'
6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM

Bandit Level 11 → Level 12
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions
bandit11@bandit:~$ cat data.txt | tr '[G-ZA-Fg-za-f]' '[T-ZA-St-za-s]'
The password is JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
bandit11@bandit:~$ cat data.txt | tr '[G-ZA-Fg-za-f]' '[T-ZA-St-za-s]' | awk 'NF {print $NF}'
JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv

Bandit Level 12 → Level 13
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)
                           
┌──(kali㉿kali)-[~/OTW]
└─$ cat decompresor.sh
#!/bin/bash

rm data* 2>/dev/null
name_decompressed=$(7z l content.gzip | grep "Name" -A 2 | tail -n 1 | awk 'NF{print $NF}')
7z x content.gzip > /dev/null 2>&1

while true; do
        7z l $name_decompressed > /dev/null 2>&1

        if [ "$(echo $?)" == "0" ]; then
                decompressed_next=$(7z l $name_decompressed | grep "Name" -A 2 | tail -n 1 | awk 'NF{print $NF}')

                7z x $name_decompressed > /dev/null 2>&1 && name_decompressed=$decompressed_next
        else
                cat $name_decompressed; rm data* 2>/dev/null
                exit 1
        fi
done               
                                                                                                                           
┌──(kali㉿kali)-[~/OTW]
└─$ ./decompresor.sh   
The password is wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw
                                                                                                                            
Bandit Level 13 → Level 14
The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on
bandit13@bandit:~$ ls
sshkey.private
bandit13@bandit:~$ file sshkey.private 
sshkey.private: PEM RSA private key
bandit13@bandit:~$ cat sshkey.private 
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAxkkOE83W2cOT7IWhFc9aPaaQmQDdgzuXCv+ppZHa++buSkN+
gg0tcr7Fw8NLGa5+Uzec2rEg0WmeevB13AIoYp0MZyETq46t+jk9puNwZwIt9XgB
ZufGtZEwWbFWw/vVLNwOXBe4UWStGRWzgPpEeSv5Tb1VjLZIBdGphTIK22Amz6Zb
ThMsiMnyJafEwJ/T8PQO3myS91vUHEuoOMAzoUID4kN0MEZ3+XahyK0HJVq68KsV
ObefXG1vvA3GAJ29kxJaqvRfgYnqZryWN7w3CHjNU4c/2Jkp+n8L0SnxaNA+WYA7
jiPyTF0is8uzMlYQ4l1Lzh/8/MpvhCQF8r22dwIDAQABAoIBAQC6dWBjhyEOzjeA
J3j/RWmap9M5zfJ/wb2bfidNpwbB8rsJ4sZIDZQ7XuIh4LfygoAQSS+bBw3RXvzE
pvJt3SmU8hIDuLsCjL1VnBY5pY7Bju8g8aR/3FyjyNAqx/TLfzlLYfOu7i9Jet67
xAh0tONG/u8FB5I3LAI2Vp6OviwvdWeC4nOxCthldpuPKNLA8rmMMVRTKQ+7T2VS
nXmwYckKUcUgzoVSpiNZaS0zUDypdpy2+tRH3MQa5kqN1YKjvF8RC47woOYCktsD
o3FFpGNFec9Taa3Msy+DfQQhHKZFKIL3bJDONtmrVvtYK40/yeU4aZ/HA2DQzwhe
ol1AfiEhAoGBAOnVjosBkm7sblK+n4IEwPxs8sOmhPnTDUy5WGrpSCrXOmsVIBUf
laL3ZGLx3xCIwtCnEucB9DvN2HZkupc/h6hTKUYLqXuyLD8njTrbRhLgbC9QrKrS
M1F2fSTxVqPtZDlDMwjNR04xHA/fKh8bXXyTMqOHNJTHHNhbh3McdURjAoGBANkU
1hqfnw7+aXncJ9bjysr1ZWbqOE5Nd8AFgfwaKuGTTVX2NsUQnCMWdOp+wFak40JH
PKWkJNdBG+ex0H9JNQsTK3X5PBMAS8AfX0GrKeuwKWA6erytVTqjOfLYcdp5+z9s
8DtVCxDuVsM+i4X8UqIGOlvGbtKEVokHPFXP1q/dAoGAcHg5YX7WEehCgCYTzpO+
xysX8ScM2qS6xuZ3MqUWAxUWkh7NGZvhe0sGy9iOdANzwKw7mUUFViaCMR/t54W1
GC83sOs3D7n5Mj8x3NdO8xFit7dT9a245TvaoYQ7KgmqpSg/ScKCw4c3eiLava+J
3btnJeSIU+8ZXq9XjPRpKwUCgYA7z6LiOQKxNeXH3qHXcnHok855maUj5fJNpPbY
iDkyZ8ySF8GlcFsky8Yw6fWCqfG3zDrohJ5l9JmEsBh7SadkwsZhvecQcS9t4vby
9/8X4jS0P8ibfcKS4nBP+dT81kkkg5Z5MohXBORA7VWx+ACohcDEkprsQ+w32xeD
qT1EvQKBgQDKm8ws2ByvSUVs9GjTilCajFqLJ0eVYzRPaY6f++Gv/UVfAPV4c+S0
kAWpXbv5tbkkzbS0eaLPTKgLzavXtQoTtKwrjpolHKIHUz6Wu+n4abfAIRFubOdN
/+aLoRQ0yBDRbdXMsZN/jvY44eM+xRLdRVyMmdPtP8belRi2E2aEzA==
-----END RSA PRIVATE KEY-----
bandit13@bandit:~$ 

bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost

bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq


Bandit Level 14 → Level 15
The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
bandit14@bandit:~$ echo "fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq" | nc localhost 30000
Correct!
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt

bandit14@bandit:~$ telnet localhost 30000
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
Correct!
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt

Connection closed by foreign host.
bandit14@bandit:~$ 

Bandit Level 15 → Level 16
The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.
Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…

bandit15@bandit:~$ openssl s_client -connect 127.0.0.1:30001
CONNECTED(00000003)

read R BLOCK
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
Correct!
JQttfApK4SeyHwDlI9SXGR50qclOAil1

closed

Bandit Level 16 → Level 17
The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
bandit16@bandit:~$ nmap --open -T5 -v -n -p31000-32000 127.0.0.1
Starting Nmap 7.80 ( https://nmap.org ) at 2022-11-17 21:25 UTC
Initiating Ping Scan at 21:25
Scanning 127.0.0.1 [2 ports]
Completed Ping Scan at 21:25, 0.00s elapsed (1 total hosts)
Initiating Connect Scan at 21:25
Scanning 127.0.0.1 [1001 ports]
Discovered open port 31691/tcp on 127.0.0.1
Discovered open port 31960/tcp on 127.0.0.1
Discovered open port 31518/tcp on 127.0.0.1
Discovered open port 31790/tcp on 127.0.0.1
Discovered open port 31046/tcp on 127.0.0.1
Completed Connect Scan at 21:25, 0.02s elapsed (1001 total ports)
Nmap scan report for 127.0.0.1
Host is up (0.000099s latency).
Not shown: 996 closed ports
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.06 seconds
bandit16@bandit:~$ 
bandit16@bandit:~$ openssl s_client -connect 127.0.0.1:31790
---
read R BLOCK
JQttfApK4SeyHwDlI9SXGR50qclOAil1
Correct!
-----BEGIN RSA PRIVATE KEY-----
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
-----END RSA PRIVATE KEY-----

closed
bandit16@bandit:~$ mktemp -d
/tmp/tmp.mb2rvtj6Xv
bandit16@bandit:~$ 
bandit16@bandit:/tmp/tmp.mb2rvtj6Xv$ nano id_rsa
Unable to create directory /home/bandit16/.local/share/nano/: No such file or directory
It is required for saving/loading search history or cursor positions.

bandit16@bandit:/tmp/tmp.mb2rvtj6Xv$ ls -l
total 4
-rw-rw-r-- 1 bandit16 bandit16 1675 Nov 17 21:33 id_rsa
bandit16@bandit:/tmp/tmp.mb2rvtj6Xv$ chmod 600 id_rsa 
bandit16@bandit:/tmp/tmp.mb2rvtj6Xv$ ls -l
total 4
-rw------- 1 bandit16 bandit16 1675 Nov 17 21:33 id_rsa
bandit16@bandit:/tmp/tmp.mb2rvtj6Xv$ ssh -i id_rsa bandit17@localhost -p 2220
bandit17@bandit:~$ cat /etc/bandit_pass/bandit17
VwOSWtCA7lRKkTfbr2IDh6awj9RNZM5e

Bandit Level 17 → Level 18
There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new
NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19
bandit17@bandit:~$ ls
passwords.new  passwords.old
bandit17@bandit:~$ cat passwords.new | wc -l
100
bandit17@bandit:~$ cat passwords.old | wc -l
100
bandit17@bandit:~$ diff passwords.new passwords.old
42c42
< hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg
---
> 09wUIyMU4YhOzl1Lzxoz0voIBzZ2TUAf
bandit17@bandit:~$ 

Bandit Level 18 → Level 19
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.
┌──(root㉿kali)-[~]
└─# ssh bandit18@bandit.labs.overthewire.org -p 2220 "bash --norc"
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password: 
ls
readme
cat readme
awhqfNnAbc1naukrpqDYcF95h7HoMTrC

Bandit Level 19 → Level 20
To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.
bandit19@bandit:~$ ls -l
total 16
-rwsr-x--- 1 bandit20 bandit19 14872 Sep  1 06:30 bandit20-do
bandit19@bandit:~$ ./bandit20-do sh
$ whoami
bandit19
$ exit
bandit19@bandit:~$ ./bandit20-do bash -p
bash-5.1$ whoami
bandit20
bash-5.1$ cat /etc/bandit_pass/bandit20
VxCazJaVykI6W36BkBU0mJTCM8rR95XT
bash-5.1$ 


Bandit Level 20 → Level 21
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).
NOTE: Try connecting to your own network daemon to see if it works as you think

=== Primera conexion: ===
bandit20@bandit:~$ ls
suconnect
bandit20@bandit:~$ ./suconnect 
Usage: ./suconnect <portnumber>
This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.
bandit20@bandit:~$ ./suconnect 5757
Read: VxCazJaVykI6W36BkBU0mJTCM8rR95XT
Password matches, sending next password
bandit20@bandit:~$ 

=== Segunda conexion: ===
bandit20@bandit:~$ nc -nlvp 5757
Listening on 0.0.0.0 5757
Connection received on 127.0.0.1 33154
VxCazJaVykI6W36BkBU0mJTCM8rR95XT
NvEJF7oVjkddltPSrdKEFOllh9V1IBcq
bandit20@bandit:~$ 

Bandit Level 21 → Level 22
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
bandit21@bandit:~$ ls -l /etc/cron.d
total 36
-rw-r--r-- 1 root root  62 Sep  1 06:30 cronjob_bandit15_root
-rw-r--r-- 1 root root  62 Sep  1 06:30 cronjob_bandit17_root
-rw-r--r-- 1 root root 120 Sep  1 06:30 cronjob_bandit22
-rw-r--r-- 1 root root 122 Sep  1 06:30 cronjob_bandit23
-rw-r--r-- 1 root root 120 Sep  1 06:30 cronjob_bandit24
-rw-r--r-- 1 root root  62 Sep  1 06:30 cronjob_bandit25_root
-rw-r--r-- 1 root root 201 Jan  8  2022 e2scrub_all
-rwx------ 1 root root  52 Sep  1 06:30 otw-tmp-dir
-rw-r--r-- 1 root root 396 Feb  2  2021 sysstat
bandit21@bandit:~$ cat /etc/cron.d/cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
bandit21@bandit:~$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:~$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff
bandit21@bandit:~$ 

Bandit Level 22 → Level 23
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.
bandit22@bandit:~$ ls /etc/cron.d
cronjob_bandit15_root  cronjob_bandit22  cronjob_bandit24       e2scrub_all  sysstat
cronjob_bandit17_root  cronjob_bandit23  cronjob_bandit25_root  otw-tmp-dir
bandit22@bandit:~$ cat /etc/cron.d/cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:~$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
bandit22@bandit:~$ mytarget=$(echo I am user bandit23 | md5sum | cut -d ' ' -f 1)
bandit22@bandit:~$ echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"
Copying passwordfile /etc/bandit_pass/ to /tmp/8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:~$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G
bandit22@bandit:~$ 

Bandit Level 23 â†’ Level 24ls
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!
NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy aroundâ€¦

andit23@bandit:~$ ls /etc/cron.d
cronjob_bandit15_root  cronjob_bandit22  cronjob_bandit24       e2scrub_all  sysstat
cronjob_bandit17_root  cronjob_bandit23  cronjob_bandit25_root  otw-tmp-dir
bandit23@bandit:~$ cat /etc/cron.d/cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit23@bandit:~$ cat /usr/bin/cronjob_bandit24.sh 
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done

bandit23@bandit:~$ 
andit23@bandit:~$ ls -l /var/spool/
total 12
dr-xr-x--- 3 bandit24 bandit23 4096 Sep  1 06:30 bandit24
drwxr-xr-x 3 root     root     4096 Aug 10 12:21 cron
lrwxrwxrwx 1 root     root        7 Aug 10 12:21 mail -> ../mail
drwx------ 2 syslog   adm      4096 Dec 30  2021 rsyslog
bandit23@bandit:~$ mktemp -d
/tmp/tmp.Y5nI8QP62y
bandit23@bandit:~$ cd /tmp/tmp.Y5nI8QP62y
andit23@bandit:/tmp/tmp.Y5nI8QP62y$ chmod o+rwx ../tmp.Y5nI8QP62y
bandit23@bandit:/tmp/tmp.Y5nI8QP62y$ touch script.sh
bandit23@bandit:/tmp/tmp.Y5nI8QP62y$ chmod +x script.sh 
bandit23@bandit:/tmp/tmp.Y5nI8QP62y$ nano script.sh 
		#!/bin/bash
		cat /etc/bandit_pass/bandit24 >/tmp/tmp.Y5nI8QP62y/mypass24.txt

bandit23@bandit:/tmp/tmp.Y5nI8QP62y$ cp script.sh /var/spool/bandit24/foo/script.sh
bandit23@bandit:/tmp/tmp.Y5nI8QP62y$ watch -n 1 ls -l
bandit23@bandit:/tmp/tmp.Y5nI8QP62y$ ls
mypass24.txt  script.sh
bandit23@bandit:/tmp/tmp.Y5nI8QP62y$ cat mypass24.txt 
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar


Bandit Level 24 → Level 25
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
    1  ls /etc/cron.d/cronjob_bandit
    2  mktemp -d
    3  cd /tmp/tmp.L6q1V5PzZP
    4  touch script,sh
    5  touch script.sh
    6  ls
    7  rm script,sh 
    8  ls
    9  chmod +x script.sh 
   10  nano script.sh
   11  ./script.sh 
   12  ./script.sh > dictionay.txt
   13  cat dictionay.txt | wc -l
   14  cat dictionay.txt | nc localhost 30002
   15  cat dictionay.txt | nc localhost 30002 | grep -v -E "Wrong|Please"

bandit24@bandit:/tmp/tmp.L6q1V5PzZP$ cat dictionay.txt | nc localhost 30002 | grep -v -E "Wrong|Please"
Correct!
The password of user bandit25 is p7TaowMYrmu23Ol8hiZh9UvD0O9hpx8d

Exiting.

Bandit Level 25 → Level 26
Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.
bandit25@bandit:~$ cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
bandit25@bandit:~$ cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
bandit25@bandit:~$ 
Pulsar v
:e /etc/bandit_pass/bandit26
c7GvcKlw9mC7aUQaPx7nwFstuAIBw1o1
:set shell=/bin/bash
:shell                                                                                                                           
bandit26@bandit:~$ 
 

Bandit Level 26 → Level 27
Good job getting a shell! Now hurry and grab the password for bandit27!
bandit26@bandit:~$ ./bandit27-do  
Run a command as another user. 
  Example: ./bandit27-do id 
bandit26@bandit:~$ ./bandit27-do whoami 
bandit27 
bandit26@bandit:~$ ./bandit27-do bash -p 
bash-5.1$ whoami 
bandit27 
bash-5.1$ cat /etc/bandit_pass/bandit27 
YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS 
bash-5.1$ 
 

Bandit Level 27 → Level 28
There is a git repository at ssh://bandit27-git@localhost/home/bandit27-git/repo. The password for the user bandit27-git is the same as for the user bandit27.
Clone the repository and find the password for the next level.
bandit27@bandit:~$ mktemp -d 
/tmp/tmp.TUn4ORywwD 
bandit27@bandit:~$ cd /tmp/tmp.TUn4ORywwD
 bandit27@bandit:/tmp/tmp.TUn4ORywwD$ git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
 bandit27@bandit:/tmp/tmp.TUn4ORywwD$ ls 
repo 
bandit27@bandit:/tmp/tmp.TUn4ORywwD$ cd repo 
bandit27@bandit:/tmp/tmp.TUn4ORywwD/repo$ ls 
README 
bandit27@bandit:/tmp/tmp.TUn4ORywwD/repo$ cat README  
The password to the next level is: AVanL161y9rsbcJIsFHuw35rjaOM19nR
 

Bandit Level 28 → Level 29
There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo. The password for the user bandit28-git is the same as for the user bandit28.
Clone the repository and find the password for the next level.
bandit28@bandit:~$ mktemp -d 
/tmp/tmp.WesdYQ4J1w 
bandit28@bandit:~$ cd /tmp/tmp.WesdYQ4J1w 
bandit28@bandit:/tmp/tmp.WesdYQ4J1w$ git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
 bandit28@bandit:/tmp/tmp.WesdYQ4J1w$ ls 
repo 
bandit28@bandit:/tmp/tmp.WesdYQ4J1w$ cd repo 
bandit28@bandit:/tmp/tmp.WesdYQ4J1w/repo$ ls 
README.md 
bandit28@bandit:/tmp/tmp.WesdYQ4J1w/repo$ cat README.md  
# Bandit Notes 
Some notes for level29 of bandit. 
 
## credentials 
 
- username: bandit29 
- password: xxxxxxxxxx 
 
bandit28@bandit:/tmp/tmp.WesdYQ4J1w/repo$ git log -p 
commit 43032edb2fb868dea2ceda9cb3882b2c336c09ec (HEAD -> master, origin/master, origin/HEAD) 
Author: Morla Porla <morla@overthewire.org> 
Date:   Thu Sep 1 06:30:25 2022 +0000 
 
    fix info leak 
 
diff --git a/README.md b/README.md 
index b302105..5c6457b 100644 
--- a/README.md 
+++ b/README.md 
@@ -4,5 +4,5 @@ Some notes for level29 of bandit. 
 ## credentials 
  
 - username: bandit29 
-- password: tQKvmcwNYcFS6vmPHIUSI3ShmsrQZK8S 
+- password: xxxxxxxxxx 
  
 
commit bdf3099fb1fb05faa29e80ea79d9db1e29d6c9b9 
Author: Morla Porla <morla@overthewire.org> 
Date:   Thu Sep 1 06:30:25 2022 +0000 
 
    add missing data 
 
diff --git a/README.md b/README.md 
index 7ba2d2f..b302105 100644 
--- a/README.md 
+++ b/README.md 
@@ -4,5 +4,5 @@ Some notes for level29 of bandit. 
 ## credentials 
  
 - username: bandit29 
-- password: <TBD> 
:
 

Bandit Level 29 → Level 30
There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo. The password for the user bandit29-git is the same as for the user bandit29.
Clone the repository and find the password for the next level.

bandit29@bandit:/tmp/tmp.kPQC9GVg42$ ls 
repo 
bandit29@bandit:/tmp/tmp.kPQC9GVg42$ cd repo 
bandit29@bandit:/tmp/tmp.kPQC9GVg42/repo$ ls 
README.md 
bandit29@bandit:/tmp/tmp.kPQC9GVg42/repo$ cat README.md  
# Bandit Notes 
Some notes for bandit30 of bandit. 
 
## credentials 
 
- username: bandit30 
- password: <no passwords in production!> 
 
bandit29@bandit:/tmp/tmp.kPQC9GVg42/repo$ git log -p 
commit 1748acec99ba66676acd551c2932fb9fc14a98a3 (HEAD -> master, origin/master, origin/HEAD) 
Author: Ben Dover <noone@overthewire.org> 
Date:   Thu Sep 1 06:30:26 2022 +0000 
 
    fix username 
 
diff --git a/README.md b/README.md 
index 2da2f39..1af21d3 100644 
--- a/README.md 
+++ b/README.md 
@@ -3,6 +3,6 @@ Some notes for bandit30 of bandit. 
  
 ## credentials 
  
-- username: bandit29 
+- username: bandit30 
 - password: <no passwords in production!> 
  
 
commit c27fff763003bb1d57d311e6763211110b94cc87 
Author: Ben Dover <noone@overthewire.org> 
Date:   Thu Sep 1 06:30:26 2022 +0000 
 
    initial commit of README.md 
 
diff --git a/README.md b/README.md 
new file mode 100644 
index 0000000..2da2f39 
--- /dev/null 
+++ b/README.md 
@@ -0,0 +1,8 @@ 
+# Bandit Notes 
+Some notes for bandit30 of bandit. 
+ 
+## credentials 
+ 
+- username: bandit29 
+- password: <no passwords in production!> 
+ 
bandit29@bandit:/tmp/tmp.kPQC9GVg42/repo$ git branch -r 
  origin/HEAD -> origin/master 
  origin/dev 
  origin/master 
  origin/sploits-dev 
bandit29@bandit:/tmp/tmp.kPQC9GVg42/repo$ git checkout dev 
Branch 'dev' set up to track remote branch 'dev' from 'origin'. 
Switched to a new branch 'dev' 
bandit29@bandit:/tmp/tmp.kPQC9GVg42/repo$ git branch -r 
  origin/HEAD -> origin/master 
  origin/dev 
  origin/master 
  origin/sploits-dev 
bandit29@bandit:/tmp/tmp.kPQC9GVg42/repo$ git log -p 
commit 2b1395f00cfb986163082c50100be5be8f249f64 (HEAD -> dev, origin/dev) 
Author: Morla Porla <morla@overthewire.org> 
Date:   Thu Sep 1 06:30:26 2022 +0000 
 
    add data needed for development 
 
diff --git a/README.md b/README.md 
index 1af21d3..a4b1cf1 100644 
--- a/README.md 
+++ b/README.md 
@@ -4,5 +4,5 @@ Some notes for bandit30 of bandit. 
 ## credentials 
  
 - username: bandit30 
-- password: <no passwords in production!> 
+- password: xbhV3HpNGlTIdnjUrdAlPzc2L6y9EOnS 
  
 
commit 989df8073e16b5f7ec337f51bc1f60bd2f6b7e0b 
Author: Ben Dover <noone@overthewire.org> 
Date:   Thu Sep 1 06:30:26 2022 +0000 
 
    add gif2ascii 
 
diff --git a/code/gif2ascii.py b/code/gif2ascii.py 
new file mode 100644 
index 0000000..8b13789 
--- /dev/null 
+++ b/code/gif2ascii.py 
@@ -0,0 +1 @@ 
+ 
 
commit 1748acec99ba66676acd551c2932fb9fc14a98a3 (origin/master, origin/HEAD, master) 
Author: Ben Dover <noone@overthewire.org> 
Date:   Thu Sep 1 06:30:26 2022 +0000 
 
    fix username 
 
diff --git a/README.md b/README.md 
index 2da2f39..1af21d3 100644 
--- a/README.md 
+++ b/README.md 
@@ -3,6 +3,6 @@ Some notes for bandit30 of bandit. 
  
 ## credentials 
  
:
 
Bandit Level 30 → Level 31
There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo. The password for the user bandit30-git is the same as for the user bandit30.
Clone the repository and find the password for the next level.
bandit30@bandit:/tmp/tmp.4jaF9pq8l9$ git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
 bandit30@bandit:/tmp/tmp.4jaF9pq8l9/repo$ git log -p 
commit a325f29e1cc26b0f0dc5f89b4348e389b408cc87 (HEAD -> master, origin/master, origin/HEAD) 
Author: Ben Dover <noone@overthewire.org> 
Date:   Thu Sep 1 06:30:28 2022 +0000 
 
    initial commit of README.md 
 
diff --git a/README.md b/README.md 
new file mode 100644 
index 0000000..029ba42 
--- /dev/null 
+++ b/README.md 
@@ -0,0 +1 @@ 
+just an epmty file... muahaha 
bandit30@bandit:/tmp/tmp.4jaF9pq8l9/repo$ git branch -r 
  origin/HEAD -> origin/master 
  origin/master 
bandit30@bandit:/tmp/tmp.4jaF9pq8l9/repo$ git tag 
secret 
bandit30@bandit:/tmp/tmp.4jaF9pq8l9/repo$ cat secret 
cat: secret: No such file or directory 
bandit30@bandit:/tmp/tmp.4jaF9pq8l9/repo$ git show secret 
OoffzGDlzhAlerFJ2cAiz1D41JW1Mhmt 
bandit30@bandit:/tmp/tmp.4jaF9pq8l9/repo$ 
 
Bandit Level 31 → Level 32
There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo. The password for the user bandit31-git is the same as for the user bandit31.
Clone the repository and find the password for the next level.
 
bandit31@bandit:/tmp/tmp.3yIjortLM9/repo$ cat README.md  
This time your task is to push a file to the remote repository. 
 
Details: 
    File name: key.txt 
    Content: 'May I come in?' 
    Branch: master 
 
bandit31@bandit:/tmp/tmp.3yIjortLM9/repo$ git log -p 
commit a37d1efc4f663779c495f1253b7d0cfb3734346a (HEAD -> master, origin/master, origin/HEAD) 
Author: Ben Dover <noone@overthewire.org> 
Date:   Thu Sep 1 06:30:29 2022 +0000 
 
    initial commit 
 
diff --git a/.gitignore b/.gitignore 
new file mode 100644 
index 0000000..2211df6 
--- /dev/null 
+++ b/.gitignore 
@@ -0,0 +1 @@ 
+*.txt 
diff --git a/README.md b/README.md 
new file mode 100644 
index 0000000..0edecc0 
--- /dev/null 
+++ b/README.md 
@@ -0,0 +1,7 @@ 
+This time your task is to push a file to the remote repository. 
+ 
+Details: 
+    File name: key.txt 
+    Content: 'May I come in?' 
+    Branch: master 
+ 
bandit31@bandit:/tmp/tmp.3yIjortLM9/repo$ touch key.txt 
bandit31@bandit:/tmp/tmp.3yIjortLM9/repo$ echo "May I come in?" 
May I come in? 
bandit31@bandit:/tmp/tmp.3yIjortLM9/repo$ echo "May I come in?" > key.txt 
bandit31@bandit:/tmp/tmp.3yIjortLM9/repo$ git branch -r 
  origin/HEAD -> origin/master 
  origin/master 
bandit31@bandit:/tmp/tmp.3yIjortLM9/repo$ git add key.txt 
The following paths are ignored by one of your .gitignore files: 
key.txt 
hint: Use -f if you really want to add them. 
hint: Turn this message off by running 
hint: "git config advice.addIgnoredFile false" 
bandit31@bandit:/tmp/tmp.3yIjortLM9/repo$ rm .gitignore 
bandit31@bandit:/tmp/tmp.3yIjortLM9/repo$ git add key.txt 
bandit31@bandit:/tmp/tmp.3yIjortLM9/repo$ git commit -m "Add a new key" 
[master 17fa546] Add a new key 
 1 file changed, 1 insertion(+) 
 create mode 100644 key.txt 
bandit31@bandit:/tmp/tmp.3yIjortLM9/repo$ git push -u origin master 
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established. 
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY. 
This key is not known by any other names 
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes 
Could not create directory '/home/bandit31/.ssh' (Permission denied). 
Failed to add the host to the list of known hosts (/home/bandit31/.ssh/known_hosts). 
                         _                     _ _ _    
                        | |__   __ _ _ __   __| (_) |_  
                        | '_ \ / _` | '_ \ / _` | | __| 
                        | |_) | (_| | | | | (_| | | |_  
                        |_.__/ \__,_|_| |_|\__,_|_|\__| 
                                                        
 
                      This is an OverTheWire game server.  
            More information on http://www.overthewire.org/wargames 
 
bandit31-git@localhost's password:  
Enumerating objects: 4, done. 
Counting objects: 100% (4/4), done. 
Delta compression using up to 2 threads 
Compressing objects: 100% (2/2), done. 
Writing objects: 100% (3/3), 325 bytes | 325.00 KiB/s, done. 
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 
remote: ### Attempting to validate files... #### 
remote:  
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo. 
remote:  
remote: Well done! Here is the password for the next level: 
remote: rmCBvG56y58BXzv98yZGdO7ATVL5dW8y  
remote:  
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo. 
remote:  
To ssh://localhost:2220/home/bandit31-git/repo 
 ! [remote rejected] master -> master (pre-receive hook declined) 
error: failed to push some refs to 'ssh://localhost:2220/home/bandit31-git/repo'
 

Bandit Level 32 → Level 33
After all this git stuff its time for another escape. Good luck!
>> $0 
$ whoami 
bandit33 
$ cat /etc/bandit_pass/bandit33 
odHo63fHiFqcWWJG9rLiLDtPm45KzUKy 
$ 
 
Bandit Level 33 → Level 34
At this moment, level 34 does not exist yet.

 bandit33@bandit:~$ ls 
README.txt 
bandit33@bandit:~$ cat README.txt  
Congratulations on solving the last level of this game! 
 
At this moment, there are no more levels to play in this game. However, we are constantly working 
on new levels and will most likely expand this game with more levels soon. 
Keep an eye out for an announcement on our usual communication channels! 
In the meantime, you could play some of our other wargames. 
 
If you have an idea for an awesome new level, please let us know! 
bandit33@bandit:~$ 


bandit5@bandit:~$ cat /etc/passwd | head -n 1 | sed 's/root/noroot/g'
noroot:x:0:0:noroot:/noroot:/bin/bash
bandit5@bandit:~$ cat /etc/passwd | grep '^root'
root:x:0:0:root:/root:/bin/bash
bandit5@bandit:~$ cat /etc/passwd | tr ‘r’ ‘o’ 
ooot:x:0:0:ooot:/ooot:/bin/bash

