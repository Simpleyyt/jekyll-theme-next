---
title: OverTheWire Leviathan 0번 Write-up
date: 2017-8-25 11:40:00
categories: leviathan
tags: overthewire, leviathan
---
    login as: leviathan0
     _            _       _   _
    | | _____   _(_) __ _| |_| |__   __ _ _ __
    | |/ _ \ \ / / |/ _` | __| '_ \ / _` | '_ \
    | |  __/\ V /| | (_| | |_| | | | (_| | | | |
    |_|\___| \_/ |_|\__,_|\__|_| |_|\__,_|_| |_|
    
    a http://www.overthewire.org wargame.
    
    leviathan0@leviathan.labs.overthewire.org's password:
    Welcome to Ubuntu 14.04 LTS (GNU/Linux 4.4.0-71-generic x86_64)
    
     * Documentation:  https://help.ubuntu.com/
    
    The programs included with the Ubuntu system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.
    
    Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
    applicable law.
    
    leviathan0@leviathan:~$

텍스트 아트가 반겨준다.

    leviathan0@leviathan:~$ ls
    leviathan0@leviathan:~$ ls -al
    total 28
    drwxr-xr-x  4 leviathan0 leviathan0 4096 Aug 25 14:18 .
    drwxr-xr-x 11 root       root       4096 Aug 25 14:18 ..
    drwxr-x---  2 leviathan1 leviathan0 4096 Jun 15 11:38 .backup
    -rw-r--r--  1 leviathan0 leviathan0  220 Apr  9  2014 .bash_logout
    -rw-r--r--  1 leviathan0 leviathan0 3637 Apr  9  2014 .bashrc
    drwx------  2 leviathan0 leviathan0 4096 Aug 25 14:18 .cache
    -rw-r--r--  1 leviathan0 leviathan0  675 Apr  9  2014 .profile

`ls`를 입력해보면 아무파일도 나오지 않는데, 숨겨진 파일 중에서 `.backup`이란 디렉토리는 leviathan1의 소유로 되어 있다. 

    leviathan0@leviathan:~$ ls .backup
    bookmarks.html
    leviathan0@leviathan:~$ cat .backup/bookmarks.html |head
    <!DOCTYPE NETSCAPE-Bookmark-file-1>
    <!-- This is an automatically generated file.
         It will be read and overwritten.
         DO NOT EDIT! -->
    <META HTTP-EQUIV="Content-Type" CONTENT="text/html; charset=UTF-8">
    <TITLE>Bookmarks</TITLE>
    <H1 LAST_MODIFIED="1160271046">Bookmarks</H1>
    
    <DL><p>
        <DT><H3 LAST_MODIFIED="1160249304" PERSONAL_TOOLBAR_FOLDER="true" ID="rdf:#$FvPhC3">Bookmarks Toolbar Folder</H3>

`.backup`이란 폴더 내애 있는 `bookmarks.html`파일은 매우 길어서 `|head`옵션으로 10줄만 출력해 보았는데, 저런식이라면 눈으로 플래그를 확인하는것은 불가능하다. 검색을 해야한다.

    leviathan0@leviathan:~/.backup$ cat bookmarks.html |grep leviathan
    <DT><A HREF="http://leviathan.labs.overthewire.org/passwordus.html | This will be fixed later, the password for leviathan1 is rioGegei8m" ADD_DATE="1155384634" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">password to leviathan1</A>

`cat`명령어 뒤에 `|grep`명령어를 줌으로써 `leviathan`이 들어가는 부분만 검색해보면 답이 나온다. flag등 몇가지 키워드를 검색해 보니 나왔다.

leviathan이나 bandit이나 모두 난이도는 매우 쉬운데, 서버가 너무 느려서 할 맛이 안난다.ㅡㅡ