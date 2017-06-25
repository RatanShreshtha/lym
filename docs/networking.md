## Networking commands

In this chapter we will learn about a few basic networking commands,
which will help us in our daily Linux usage.

```.. index:: ip
```
### Finding the IP address

*ip* command can be used to find IP address of the system.

```
$ ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1454 qdisc fq_codel state UP group default qlen 1000
    link/ether fa:16:3e:3c:ed:27 brd ff:ff:ff:ff:ff:ff
    inet 172.25.97.253/20 brd 172.25.111.255 scope global dynamic eth0
       valid_lft 57021sec preferred_lft 57021sec
    inet6 fe80::f816:3eff:fe3c:ed27/64 scope link 
       valid_lft forever preferred_lft forever
```

Here *lo* is a special device which points to the same system (also known as *localhost*). The IP *127.0.0.1* always points to the the *localhost*. *eth0* is our ethernet device which connects with the network.

```.. index:: ping
```
### ping command

*ping* is simple way to find if you are connected to Internet or not. We can also ping any particular computer to find if the computer is connected to the network or not. Press *Ctrl+c* to stop the loop.

```
$ ping google.com
PING google.com (216.58.201.142) 56(84) bytes of data.
64 bytes from mad06s25-in-f142.1e100.net (216.58.201.142): icmp_seq=1 ttl=44 time=157 ms
64 bytes from mad06s25-in-f142.1e100.net (216.58.201.142): icmp_seq=2 ttl=44 time=156 ms
64 bytes from mad06s25-in-f142.1e100.net (216.58.201.142): icmp_seq=3 ttl=44 time=156 ms
^C
--- google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2000ms
rtt min/avg/max/mdev = 156.373/156.811/157.566/0.704 ms
```

```.. index:: dns
```

### Short story about DNS

DNS or Domain Name System is a decentralized naming system for systems which are connected to Internet (can be for private networks too). This is way the computer knows which other computer to connect, when we type google.com in our browser, or in the ping command. There are servers known as dns servers, for every domain name the client system generally connect to these dns servers, and find out the IP address of the domain name.

### /etc/resolv.conf

*/etc/resolv.conf* is the configuration file for DNS. It contains the DNS server address to use for DNS queries.

```
$ cat /etc/resolv.conf 
# Generated by NetworkManager
nameserver 8.8.8.8
```

The *8.8.8.8* is the DNS server hosted by Google.

```.. index:: dig
```

### dig command

*dig* command can tell us DNS records, MX details (used to send emails) and other information for a given domain name.

```
$ dig kushaldas.in

; <<>> DiG 9.10.4-P8-RedHat-9.10.4-5.P8.fc25 <<>> kushaldas.in
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 50750
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;kushaldas.in.			IN	A

;; ANSWER SECTION:
kushaldas.in.		5528	IN	A	208.113.152.208

;; Query time: 66 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Sun Jun 25 11:37:00 IST 2017
;; MSG SIZE  rcvd: 57
```

If you want to specify the DNS server to use, you can do that with the address specified at the end of the command along with a @ sign.

```
$ dig rtnpro.com @208.67.222.222

; <<>> DiG 9.10.4-P8-RedHat-9.10.4-5.P8.fc25 <<>> rtnpro.com @208.67.222.222
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 27312
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;rtnpro.com.			IN	A

;; AUTHORITY SECTION:
rtnpro.com.		3600	IN	SOA	dns1.bigrock.in. rtnpro.gmail.com. 2017021401 7200 7200 172800 38400

;; Query time: 899 msec
;; SERVER: 208.67.222.222#53(208.67.222.222)
;; WHEN: Sun Jun 25 11:40:01 IST 2017
;; MSG SIZE  rcvd: 106
```

```.. index:: ssh
```
### Remote login to a computer using ssh tool

We use *ssh* command to login to the remote computers. The remote
computer must have *sshd* service running, and should also allow
clients to connect to this service. Let us try to connect to the
localhost itself. Remember to start the *sshd* service before this
step.

```
$ ssh kdas@localhost
kdas@localhost's password: 
Last login: Wed Jun 21 08:44:40 2017 from 192.168.1.101
$
```

As you can see, the command syntax is ssh followed by user@hostname. If your remote system's user name is same as your current one, then you can omit the username and just use the hostname(IP address or domain name).

```
$ ssh localhost
kdas@localhost's password: 
$
```

### ssh key generation

ssh keys are used in daily life of Linux user or developer. In simple terms, it helps us to securely login to other computers. In the following example, we will create a new key for our user. 

```
$ ssh-keygen -t rsa -b 4096 -C "kushaldas@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/fedora/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/fedora/.ssh/id_rsa.
Your public key has been saved in /home/fedora/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:O6Rxir7lpFBQsBnvs+NJRU8Ih01ffVBvLTE8s5TpxLQ kushaldas@gmail.com
The key's randomart image is:
+---[RSA 4096]----+
|  o.o+o   ...*=o |
|   *.o.o .  . @=.|
|  + . o o    =E++|
|   o . o      oo |
|    + o S        |
|   . = * .       |
|  . = = o        |
|   = B   .       |
|    *..          |
+----[SHA256]-----+
```

As you can see in the command output, the key has been saved in the *~/.ssh* directory. You can also find out that these files are only
readable by the owner.

```
$ ls -l .ssh
total 12
-rw-------. 1 fedora fedora 3326 Jun 25 06:25 id_rsa
-rw-r--r--. 1 fedora fedora  745 Jun 25 06:25 id_rsa.pub
```

Each key has two parts. The *id_rsa.pub* is the public key and *id_rsa* is the private part of the key. One can safely upload or use the public key anywhere. But, the private key should be kept in a safe manner, as if people get access to your private key, they can also access all of your information from any system using that key.

In other words, do not give the private key to anyone, or do not randomly copy the *.ssh* directory to USB drive and then forget about it.

```.. index:: ssh-copy-id
```

### ssh-copy-id 

*ssh-copy-id* command copies the keys to a given remote system. After
this step one can use the ssh key to login to the box instead of the
usual password method.

```
$ ssh-copy-id fedora@209.132.184.117
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 2 key(s) remain to be installed -- if you are prompted now it is to install the new keys

fedora@209.132.184.117's password:

Number of key(s) added: 2

Now try logging into the machine, with:   "ssh 'fedora@209.132.184.117'"
and check to make sure that only the key(s) you wanted were added.
```

### Stop and disable the sshd service

If you don't need ssh access to your computer (say in your laptop), you should always stop and disable the *sshd* service in the computer.

### Disable password based login for ssh

Remember, this step can be harmful, unless you are very much sure that you can access a computer either to login physically or using your ssh key (and you have a backup of the key somewhere), you should not do this step.

By disabling password based login in sshd service, you can make sure only the person with the right private key can login to the computer. This will help in case people trying to break into the system by guessing the password. This is really helpful in case your computer is connected to some network, and you still need to access it over ssh.

We will use vim to open the */etc/ssh/sshd_config* file, which is the configuration file for *sshd* service.

```
$ sudo vim /etc/ssh/sshd_config
```

Search for the term *PasswordAuthentication*, and change the value to no. Below I added a new line for the same, you can also understand that the lines starting with *#* are comments in this configuration file. This configuration will disable password based authentication for the sshd service. You should remember to restart the sshd service after this step for the change to go live.

![](/img/passwordauthno.png)