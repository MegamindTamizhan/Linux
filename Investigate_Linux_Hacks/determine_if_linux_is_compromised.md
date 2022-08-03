# Investigate If your Linux Server has been Hacked or Not

There are many reasons why a hacker would worm his or her way into  your system and cause you serious troubles. Years ago, maybe it was to show off one’s skills, but nowadays, the intentions behind such activities can be much more complicated with much more wide-reaching consequences to the victim. This may sound obvious, but just because “everything seems fine,” this does not mean that everything is fine. Hackers could penetrate your system without letting you know and infect it with malware to take full control, and even for lateral movement among systems. The malware can be hidden in the system and serves as a backdoor or a Command & Control system for hackers to carry out malicious activities on your system.It is better to be safe than sorry. You may not immediately realize that your system has been hacked, but there are some ways in which you can determine whether your system is compromised. This article will discuss how to determine if your Linux system has been compromised by an unauthorized person or a bot is logging into your system to carry out malicious activities.

# Netstat

Netstat is an important command-line TCP/IP networking utility that provides information and statistics about protocols in use and active network connections.

We will use **netstat** on an example victim machine to check for something suspicious in the active network connections via the following command:

`ubuntu@ubuntu:~$ netstat -antp`

Here, we will see all currently active connections. Now, we will look for a connection that should not be there.

```
    ubuntu@master:~$ netstat -antp
    (Not all processes could be identified, non-owned process info
    will not be shown, you would have to be root to see it all.)
    Active Internet connections (servers and established)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
    tcp        0      0 127.0.0.1:10249         0.0.0.0:*               LISTEN      -
    tcp        0      0 172.16.5.17:2379        0.0.0.0:*               LISTEN      -
    tcp        0      0 127.0.0.1:2379          0.0.0.0:*               LISTEN      -
    tcp        0      0 127.0.0.1:59636         127.0.0.1:2379          ESTABLISHED -
    tcp        0      0 172.16.5.17:52188       172.16.5.17:6443        ESTABLISHED 2378/us
    tcp        0      0 127.0.0.1:2379          127.0.0.1:59604         ESTABLISHED 1897/./shell.elf
    tcp        0      0 127.0.0.1:2379          127.0.0.1:59654         ESTABLISHED 1453/ruby
    tcp        0      0 127.0.0.1:59550         127.0.0.1:2379          ESTABLISHED -
    tcp        0      0 127.0.0.1:59682         127.0.0.1:2379          ESTABLISHED -
```

Here, we will see all currently active connections. Now, we will look for a connection that should not be there.

 `tcp        0      0 127.0.0.1:2379          127.0.0.1:59604         ESTABLISHED 1897/./shell.elf`