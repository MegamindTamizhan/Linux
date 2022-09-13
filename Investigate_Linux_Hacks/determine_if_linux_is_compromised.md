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
    tcp        0      0 127.0.0.1:2379          127.0.0.1:44999         ESTABLISHED 1897/./shell.elf
    tcp        0      0 127.0.0.1:2379          127.0.0.1:59654         ESTABLISHED 1453/ruby
    tcp        0      0 127.0.0.1:59550         127.0.0.1:2379          ESTABLISHED -
    tcp        0      0 127.0.0.1:59682         127.0.0.1:2379          ESTABLISHED -
```

Here, we will see all currently active connections. Now, we will look for a connection that should not be there.

 ```tcp        0      0 127.0.0.1:2379          127.0.0.1:44999         ESTABLISHED 1897/./shell.elf```

 Here it is, an active connection on PORT **44999** (a port which should not be open).We can see other details about the connection, such as the **PID**, and the program name it is running in the last column. In this case, the **PID** is **1897** and the malicious payload it is running is the **./shell.elf** file.

 Another command to check for the ports currently listening and active on your system is as follows:

`ubuntu@ubuntu:~$ netstat -la`

This is quite a messy output. To filter out the listening and established connections, we will use the following command:

`ubuntu@ubuntu:~$ netstat -la | grep “LISTEN” “ESTABLISHED”`

```
    ubuntu@master:~$ netstat -la | grep “LISTEN” “ESTABLISHED”
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
    tcp        0      0 127.0.0.1:10249         0.0.0.0:*               LISTEN      -
    tcp        0      0 172.16.5.17:2379        0.0.0.0:*               LISTEN      -
    tcp        0      0 127.0.0.1:2379          0.0.0.0:*               LISTEN      -
    tcp        0      0 127.0.0.1:59636         127.0.0.1:2379          ESTABLISHED -
    tcp        0      0 172.16.5.17:52188       172.16.5.17:6443        ESTABLISHED 2378/us
    tcp        0      0 127.0.0.1:2379          127.0.0.1:44999         ESTABLISHED 1897/./shell.elf
    tcp        0      0 127.0.0.1:2379          127.0.0.1:59654         ESTABLISHED 1453/ruby
    tcp        0      0 127.0.0.1:59550         127.0.0.1:2379          ESTABLISHED -
    tcp        0      0 127.0.0.1:59682         127.0.0.1:2379          ESTABLISHED -
```

This will give you only the results that matter to you, so that you can sort through these results more easily. We can see an active connection on port **44999** in the above results.

After recognizing the malicious process, you can kill the process via following commands. We will note the **PID** of the process using the netstat command, and kill the process via the following command:

`ubuntu@ubuntu:~$ kill 1897`

# ~.bash-history

Linux keeps a record of which users logged into the system, from what IP, when, and for how long.

You can access this information with the **`last`** command. The output of this command would look be as follows:

```
    sana@master:~$ last
    sana    pts/0        172.168.15.97      Fri Jul 29 18:13   still logged in
    jack    pts/0        172.168.15.97      Fri Jul 29 17:31   still logged in
    reboot   system boot  5.11.0-49-generi  Fri Jul 29 17:31   still running
    ubuntu    pts/0        172.168.15.97      Fri Jul 29 12:41 - 17:31  (04:50)
    reboot   system boot  5.11.0-49-generi  Fri Jul 29 12:40 - 17:31  (04:50)
```

The output shows the username in first column, the Terminal in the second, the source address in the third, the login time in the fourth column, and the Total session time logged in the last column. In this case, the users **sana** and **jack** are still logged in. If you see any session that is not authorized or looks malicious, then refer to the last section of this article.

The logging history is stored in **~.bash-history file**. So, the history can be removed easily by deleting the **.bash-history** file. This action is frequently performed by attackers to cover their tracks.

`ubuntu@ubuntu:~$ cat .bash_history`

This command will show the commands run on your system, with the latest command performed at the bottom of the list.

The history can be cleared via the following command:

`ubuntu@ubuntu:~$ history -c`

This command will only delete the history from the terminal you are currently using. So, there is a more correct way to do this:

`ubuntu@ubuntu:~$ cat /dev/null > ~/.bash_history`

This will clear the contents of history but keeps the file in place. So, if you are seeing only your current login after running the **last** command, this is not a good sign at all. This indicates that your system may have been compromised and that the attacker probably deleted the history.

If you suspect a malicious user or IP, log in as that user and run the command history, as follows:

`ubuntu@ubuntu:~$ su <user>`

`ubuntu@ubuntu:~$ history`






