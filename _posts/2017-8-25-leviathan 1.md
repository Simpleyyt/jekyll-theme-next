---
title: OverTheWire Leviathan 1번 Write-up
date: 2017-8-25 11:43:00
categories: leviathan
tags: overthewire, leviathan
---

    leviathan1@leviathan:~$ ls -l
    total 8
    -r-sr-x--- 1 leviathan2 leviathan1 7501 Jun 15 11:38 check

setuid가 걸려있는 실행파일이 하나 있다.

    leviathan1@leviathan:~$ ./check
    password: whatisthis
    Wrong password, Good Bye ...

아미친리버싱이다.

    leviathan1@leviathan:~$ cp check check1
    leviathan1@leviathan:~$ ls -l
    total 16
    -r-sr-x--- 1 leviathan2 leviathan1 7501 Jun 15 11:38 check
    -r-xr-x--- 1 leviathan1 leviathan1 7501 Aug 25 14:45 check1
    leviathan1@leviathan:~$ gdb check1

파일을 복사해서 소유주를 자신으로 바꾼 후 gdb로 돌린다.

    (gdb) disass main
    Dump of assembler code for function main:
       0x0804852d <+0>:     push   %ebp
       0x0804852e <+1>:     mov    %esp,%ebp
       0x08048530 <+3>:     and    $0xfffffff0,%esp
       0x08048533 <+6>:     sub    $0x30,%esp
       0x08048536 <+9>:     mov    %gs:0x14,%eax
       0x0804853c <+15>:    mov    %eax,0x2c(%esp)
       0x08048540 <+19>:    xor    %eax,%eax
       0x08048542 <+21>:    movl   $0x786573,0x18(%esp)
       0x0804854a <+29>:    movl   $0x72636573,0x25(%esp)
       0x08048552 <+37>:    movw   $0x7465,0x29(%esp)
       0x08048559 <+44>:    movb   $0x0,0x2b(%esp)
       0x0804855e <+49>:    movl   $0x646f67,0x1c(%esp)
       0x08048566 <+57>:    movl   $0x65766f6c,0x20(%esp)
       0x0804856e <+65>:    movb   $0x0,0x24(%esp)
       0x08048573 <+70>:    movl   $0x8048680,(%esp)
       0x0804857a <+77>:    call   0x80483c0 <printf@plt>
       0x0804857f <+82>:    call   0x80483d0 <getchar@plt>
       0x08048584 <+87>:    mov    %al,0x14(%esp)
       0x08048588 <+91>:    call   0x80483d0 <getchar@plt>
       0x0804858d <+96>:    mov    %al,0x15(%esp)
       0x08048591 <+100>:   call   0x80483d0 <getchar@plt>
       0x08048596 <+105>:   mov    %al,0x16(%esp)
       0x0804859a <+109>:   movb   $0x0,0x17(%esp)
       0x0804859f <+114>:   lea    0x18(%esp),%eax
       0x080485a3 <+118>:   mov    %eax,0x4(%esp)
       0x080485a7 <+122>:   lea    0x14(%esp),%eax
       0x080485ab <+126>:   mov    %eax,(%esp)
       0x080485ae <+129>:   call   0x80483b0 <strcmp@plt>
       0x080485b3 <+134>:   test   %eax,%eax
       0x080485b5 <+136>:   jne    0x80485c5 <main+152>
       0x080485b7 <+138>:   movl   $0x804868b,(%esp)
       0x080485be <+145>:   call   0x8048400 <system@plt>
       0x080485c3 <+150>:   jmp    0x80485d1 <main+164>
       0x080485c5 <+152>:   movl   $0x8048693,(%esp)
       0x080485cc <+159>:   call   0x80483f0 <puts@plt>
       0x080485d1 <+164>:   mov    $0x0,%eax
       0x080485d6 <+169>:   mov    0x2c(%esp),%edx
       0x080485da <+173>:   xor    %gs:0x14,%edx
       0x080485e1 <+180>:   je     0x80485e8 <main+187>
       0x080485e3 <+182>:   call   0x80483e0 <__stack_chk_fail@plt>
       0x080485e8 <+187>:   leave
       0x080485e9 <+188>:   ret
    End of assembler dump.

이렇게 생겼다.
`getchar`를 3번 실행함으로써 글자 3개를 받아오고, `strcmp`로 비밀번호 값과 비교하는 구조이다. 사실 gdb가 아니라 ltrace를 사용하면 훨씬 쉬운데, 풀이를 아예 보지않으려다가 삽질만 많이 했다.

    leviathan1@leviathan:~$ ltrace ./check
    __libc_start_main(0x804852d, 1, 0xffffd804, 0x80485f0 <unfinished ...>
    printf("password: ")                             = 10
    getchar(0x8048680, 47, 0x804a000, 0x8048642password: 123
    )     = 49
    getchar(0x8048680, 47, 0x804a000, 0x8048642)     = 50
    getchar(0x8048680, 47, 0x804a000, 0x8048642)     = 51
    strcmp("123", "sex")                             = -1
    puts("Wrong password, Good Bye ..."Wrong password, Good Bye ...
    )             = 29
    +++ exited (status 0) +++

저기 보면 strcmp에 내가 입력한 '123'외에 문자열이 하나 더 있는데, 그게 정답이다. 

    $ id
    uid=12001(leviathan1) gid=12001(leviathan1) euid=12002(leviathan2) groups=12002(leviathan2),12001(leviathan1)

다시 비밀번호를 입력해 주면 `check`에 setuid가 걸려 있으므로 euid가 leviathan2로 변경이 된다. uid(Realuid)는 사용자의 진짜 권한이고, euid(effectiveuid)는 사용자에게 현재 임시로 할당된 권한까지 포함된 권한이라고 보면 된다.

    $ cat /etc/leviathan_pass/leviathan2
    ougahZi8Ta

다음 레벨의 비밀번호가 나왔다.