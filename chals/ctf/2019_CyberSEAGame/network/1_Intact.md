---
layout: menu
title: "Intact [net 80]"
description: "Cyber SEA Game 2019 [Network] Intact (80 pts)"
header-img: "chals/ctf/2019_CyberSEAGame/cyber_sea_game_2019.png"
tags: [cyber sea game, cyber sea games, cyberseagame, cyberseagames, 2019, ctf, challenge, writeup, write-up, solution, network, network forensics, intact, ftp, ftp-data, ftp data, pcap, tshark]
---

# <span style="color:red">Intact (80 pts)</span>

---

## PART 1 : CHALLENGE DESCRIPTION

```
You are monitoring the traffic of a network internal to an organization. You captured communication packets like the file attached below.
Find a flag in the communication packets in the attached file.

flag format: Flag{single-byte alphanumeric characters/symbols}
```

---

## PART 2 : GIVEN FILES

[>] [Intact_8688e1a1263c414450b12985e0d5620b.pcapng](./files/Intact_8688e1a1263c414450b12985e0d5620b.pcapng)

---

## PART 3 : GETTING THE FLAG

Examining the <strong style="color:orange">.pcapng</strong> file using __`tshark`__:

```console
$ tshark -r Intact_8688e1a1263c414450b12985e0d5620b.pcapng
 
      1   0.000000 192.168.11.242 → 192.168.11.234 TCP 66 20038 → 21 [SYN] Seq=0 Win=8192 Len=0 MSS=9158 WS=1 SACK_PERM=1
      2   0.001917 192.168.11.234 → 192.168.11.242 TCP 66 21 → 20038 [SYN, ACK] Seq=0 Ack=1 Win=8192 Len=0 MSS=16074 WS=256 SACK_PERM=1
      3   0.002015 192.168.11.242 → 192.168.11.234 TCP 54 20038 → 21 [ACK] Seq=1 Ack=1 Win=8192 Len=0
      4   0.003723 192.168.11.234 → 192.168.11.242 FTP 82 Response: 220 Microsoft FTP Service
      5   0.010449 192.168.11.242 → 192.168.11.234 FTP 68 Request: OPTS UTF8 ON
      6   0.011412 192.168.11.234 → 192.168.11.242 FTP 112 Response: 200 OPTS UTF8 command successful - UTF8 encoding now ON.
      7   0.051431 192.168.11.242 → 192.168.11.234 TCP 54 20038 → 21 [ACK] Seq=15 Ack=86 Win=8107 Len=0
      8   3.619255 192.168.11.242 → 192.168.11.234 FTP 68 Request: USER ftpuser
      9   3.620210 192.168.11.234 → 192.168.11.242 FTP 78 Response: 331 Password required
     10   3.660774 192.168.11.242 → 192.168.11.234 TCP 54 20038 → 21 [ACK] Seq=29 Ack=109 Win=8084 Len=0
     11   9.812388 192.168.11.242 → 192.168.11.234 FTP 71 Request: PASS Pass#ftp18
     12   9.819608 192.168.11.234 → 192.168.11.242 FTP 76 Response: 230 User logged in.
  ...omitted...
     59  28.800933 192.168.11.234 → 192.168.11.242 FTP 78 Response: 226 Transfer complete.
     60  28.803926 192.168.11.242 → 192.168.11.234 TCP 54 20045 → 20 [FIN, ACK] Seq=1 Ack=51 Win=585984 Len=0
     61  28.804825 192.168.11.234 → 192.168.11.242 TCP 60 20 → 20045 [ACK] Seq=51 Ack=2 Win=73216 Len=0
     62  28.841454 192.168.11.242 → 192.168.11.234 TCP 54 20038 → 21 [ACK] Seq=175 Ack=474 Win=7719 Len=0
     63  31.047760 192.168.11.242 → 192.168.11.234 FTP 60 Request: QUIT
     64  31.048726 192.168.11.234 → 192.168.11.242 FTP 68 Response: 221 Goodbye.
     65  31.048912 192.168.11.234 → 192.168.11.242 TCP 60 21 → 20038 [FIN, ACK] Seq=488 Ack=181 Win=72960 Len=0
     66  31.048964 192.168.11.242 → 192.168.11.234 TCP 54 20038 → 21 [ACK] Seq=181 Ack=489 Win=7705 Len=0
     67  31.058723 192.168.11.242 → 192.168.11.234 TCP 54 20038 → 21 [FIN, ACK] Seq=181 Ack=489 Win=7705 Len=0
     68  31.059614 192.168.11.234 → 192.168.11.242 TCP 60 21 → 20038 [ACK] Seq=489 Ack=182 Win=72960 Len=0

```

It contains captured traffic over <strong style="color:orange">FTP</strong>.

Filtering the traffic to only show <strong style="color:orange">FTP</strong> and <strong style="color:orange">FTP-DATA</strong>

```console
$ tshark -2 -R "ftp or ftp-data" -n -r Intact_8688e1a1263c414450b12985e0d5620b.pcapng

      1   0.003723 192.168.11.234 → 192.168.11.242 FTP 82 Response: 220 Microsoft FTP Service
      2   0.010449 192.168.11.242 → 192.168.11.234 FTP 68 Request: OPTS UTF8 ON
      3   0.011412 192.168.11.234 → 192.168.11.242 FTP 112 Response: 200 OPTS UTF8 command successful - UTF8 encoding now ON.
      4   3.619255 192.168.11.242 → 192.168.11.234 FTP 68 Request: USER ftpuser
      5   3.620210 192.168.11.234 → 192.168.11.242 FTP 78 Response: 331 Password required
      6   9.812388 192.168.11.242 → 192.168.11.234 FTP 71 Request: PASS Pass#ftp18
      7   9.819608 192.168.11.234 → 192.168.11.242 FTP 76 Response: 230 User logged in.
      8  11.529243 192.168.11.242 → 192.168.11.234 FTP 81 Request: PORT 192,168,11,242,78,73
      9  11.530522 192.168.11.234 → 192.168.11.242 FTP 84 Response: 200 PORT command successful.
     10  11.540066 192.168.11.242 → 192.168.11.234 FTP 60 Request: LIST
     11  11.541318 192.168.11.234 → 192.168.11.242 FTP 108 Response: 125 Data connection already open; Transfer starting.
     12  11.541318 192.168.11.234 → 192.168.11.242 FTP-DATA 156 FTP Data: 102 bytes (PORT) (LIST)
     13  11.542159 192.168.11.234 → 192.168.11.242 FTP 78 Response: 226 Transfer complete.
     14  14.737202 192.168.11.242 → 192.168.11.234 FTP 62 Request: TYPE I
     15  14.738240 192.168.11.234 → 192.168.11.242 FTP 74 Response: 200 Type set to I.
     16  21.949255 192.168.11.242 → 192.168.11.234 FTP 81 Request: PORT 192,168,11,242,78,74
     17  21.950650 192.168.11.234 → 192.168.11.242 FTP 84 Response: 200 PORT command successful.
     18  21.957487 192.168.11.242 → 192.168.11.234 FTP 69 Request: RETR FLAG.zip
     19  21.958632 192.168.11.234 → 192.168.11.242 FTP 108 Response: 125 Data connection already open; Transfer starting.
     20  21.959013 192.168.11.234 → 192.168.11.242 FTP-DATA 4764 FTP Data: 4709 bytes (PORT) (RETR FLAG.zip)
     21  22.001080 192.168.11.234 → 192.168.11.242 FTP 78 Response: 226 Transfer complete.
     22  28.746590 192.168.11.242 → 192.168.11.234 FTP 81 Request: PORT 192,168,11,242,78,77
     23  28.747848 192.168.11.234 → 192.168.11.242 FTP 84 Response: 200 PORT command successful.
     24  28.758152 192.168.11.242 → 192.168.11.234 FTP 73 Request: RETR passmemo.txt
     25  28.759456 192.168.11.234 → 192.168.11.242 FTP 108 Response: 125 Data connection already open; Transfer starting.
     26  28.759529 192.168.11.234 → 192.168.11.242 FTP-DATA 104 FTP Data: 49 bytes (PORT) (RETR passmemo.txt)
     27  28.800933 192.168.11.234 → 192.168.11.242 FTP 78 Response: 226 Transfer complete.
     28  31.047760 192.168.11.242 → 192.168.11.234 FTP 60 Request: QUIT
     29  31.048726 192.168.11.234 → 192.168.11.242 FTP 68 Response: 221 Goodbye.

$ tshark -Y ftp-data -n -r  Intact_8688e1a1263c414450b12985e0d5620b.pcapng

     21  11.541318 192.168.11.234 → 192.168.11.242 FTP-DATA 156 FTP Data: 102 bytes (PORT) (LIST)
     38  21.959013 192.168.11.234 → 192.168.11.242 FTP-DATA 4764 FTP Data: 4709 bytes (PORT) (RETR FLAG.zip)
     54  28.759529 192.168.11.234 → 192.168.11.242 FTP-DATA 104 FTP Data: 49 bytes (PORT) (RETR passmemo.txt)

```

A file called __`FLAG.zip`__ and __`passmemo.txt`__ were retrieved (RETR) from the FTP server and we should still be able to carve the files from the captured network traffic.

```console
$ tshark -Y "ftp-data" -w ftp-data.pcap -F pcap -r Intact_8688e1a1263c414450b12985e0d5620b.pcapng

$ tcpflow -r ftp-data.pcap

  reportfilename: ./report.xml

$ ls

  192.168.011.234.00020-192.168.011.242.20041
  192.168.011.234.00020-192.168.011.242.20042
  192.168.011.234.00020-192.168.011.242.20045
  ftp-data.pcap
  Intact_8688e1a1263c414450b12985e0d5620b.pcapng
  report.xml

$ file 1*

  192.168.011.234.00020-192.168.011.242.20041: ASCII text, with CRLF line terminators
  192.168.011.234.00020-192.168.011.242.20042: Zip archive data, at least v2.0 to extract
  192.168.011.234.00020-192.168.011.242.20045: Generic INItialization configuration [Password]\015

$ cat 192.168.011.234.00020-192.168.011.242.20045

  [File]    [Password]
  FLAG.zip  Do_you_use_FTP?

$ unzip 192.168.011.234.00020-192.168.011.242.20042 

  Archive:  192.168.011.234.00020-192.168.011.242.20042
  [192.168.011.234.00020-192.168.011.242.20042] FLAG.GIF password: Do_you_use_FTP?
    inflating: FLAG.GIF

$ display FLAG.GIF
```

![FLAG](./files/FLAG.GIF)

---

## FLAG : __flag{It's_dangerous_to_use_FTP}__
