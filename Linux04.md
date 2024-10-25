# Linux04

Files & media: guestbook.zip, 0704%25EC%2588%2598%25EC%2597%2585.txt, 0704(mail%25EC%2584%259C%25EB%25B2%2584).txt, 0704(roundcube)-1.txt

## Samba 홈디렉토리

### 우분투에서 연결

```bash
sudo -i
apt install samba-client cifs-utils
mkdir /mnt/admin
mount -t cifs //서버ip/admin /mnt/admin -o username=admin,password=**** → samba username과 password
```

### WSL ubuntu

```bash
root@KHP18:/mnt/admin# mount -t cifs //192.168.235.130/admin /mnt/admin -o username=admin,password=1234
root@KHP18:/mnt/admin# df -TH
Filesystem              Type           Size  Used Avail Use% Mounted on
none                    tmpfs          8.4G  4.1k  8.4G   1% /mnt/wsl
drivers                 9p             491G  104G  388G  22% /usr/lib/wsl/drivers
none                    tmpfs          8.4G     0  8.4G   0% /usr/lib/modules
none                    overlay        8.4G     0  8.4G   0% /usr/lib/modules/5.15.153.1-microsoft-standard-WSL2
/dev/sdc                ext4           1.1T  2.6G  1.1T   1% /
none                    tmpfs          8.4G   87k  8.4G   1% /mnt/wslg
none                    overlay        8.4G     0  8.4G   0% /usr/lib/wsl/lib
rootfs                  rootfs         8.4G  2.2M  8.3G   1% /init
none                    tmpfs          8.4G  943k  8.4G   1% /run
none                    tmpfs          8.4G     0  8.4G   0% /run/lock
none                    tmpfs          8.4G     0  8.4G   0% /run/shm
tmpfs                   tmpfs          4.2M     0  4.2M   0% /sys/fs/cgroup
none                    overlay        8.4G   78k  8.4G   1% /mnt/wslg/versions.txt
none                    overlay        8.4G   78k  8.4G   1% /mnt/wslg/doc
C:\                     9p             491G  104G  388G  22% /mnt/c
snapfuse                fuse.snapfuse   78M   78M     0 100% /snap/core22/1380
snapfuse                fuse.snapfuse  132k  132k     0 100% /snap/bare/5
snapfuse                fuse.snapfuse   78M   78M     0 100% /snap/core22/864
snapfuse                fuse.snapfuse   97M   97M     0 100% /snap/gtk-common-themes/1535
snapfuse                fuse.snapfuse   41M   41M     0 100% /snap/snapd/21759
snapfuse                fuse.snapfuse   43M   43M     0 100% /snap/snapd/20290
snapfuse                fuse.snapfuse  138M  138M     0 100% /snap/ubuntu-desktop-installer/1276
snapfuse                fuse.snapfuse  139M  139M     0 100% /snap/ubuntu-desktop-installer/1286
tmpfs                   tmpfs          1.7G  4.1k  1.7G   1% /run/user/0
//192.168.235.130/admin cifs            28G  3.8G   25G  14% /mnt/admin
root@KHP18:/mnt/admin# cd /mnt/admin
root@KHP18:/mnt/admin# ls
tardir  workdir
root@KHP18:/mnt/admin# touch a b c d
root@KHP18:~# umount /mnt/admin  → 연결 해제
root@KHP18:~# df -Th
Filesystem     Type           Size  Used Avail Use% Mounted on
none           tmpfs          7.8G  4.0K  7.8G   1% /mnt/wsl
drivers        9p             457G   97G  361G  22% /usr/lib/wsl/drivers
none           tmpfs          7.8G     0  7.8G   0% /usr/lib/modules
none           overlay        7.8G     0  7.8G   0% /usr/lib/modules/5.15.153.1-microsoft-standard-WSL2
/dev/sdc       ext4          1007G  2.4G  954G   1% /
none           tmpfs          7.8G   84K  7.8G   1% /mnt/wslg
none           overlay        7.8G     0  7.8G   0% /usr/lib/wsl/lib
rootfs         rootfs         7.8G  2.1M  7.8G   1% /init
none           tmpfs          7.8G  920K  7.8G   1% /run
none           tmpfs          7.8G     0  7.8G   0% /run/lock
none           tmpfs          7.8G     0  7.8G   0% /run/shm
tmpfs          tmpfs          4.0M     0  4.0M   0% /sys/fs/cgroup
none           overlay        7.8G   76K  7.8G   1% /mnt/wslg/versions.txt
none           overlay        7.8G   76K  7.8G   1% /mnt/wslg/doc
C:\            9p             457G   97G  361G  22% /mnt/c
snapfuse       fuse.snapfuse   75M   75M     0 100% /snap/core22/1380
snapfuse       fuse.snapfuse  128K  128K     0 100% /snap/bare/5
snapfuse       fuse.snapfuse   74M   74M     0 100% /snap/core22/864
snapfuse       fuse.snapfuse   92M   92M     0 100% /snap/gtk-common-themes/1535
snapfuse       fuse.snapfuse   39M   39M     0 100% /snap/snapd/21759
snapfuse       fuse.snapfuse   41M   41M     0 100% /snap/snapd/20290
snapfuse       fuse.snapfuse  132M  132M     0 100% /snap/ubuntu-desktop-installer/1276
snapfuse       fuse.snapfuse  132M  132M     0 100% /snap/ubuntu-desktop-installer/1286
tmpfs          tmpfs          1.6G  4.0K  1.6G   1% /run/user/0

```

### Rocky에서 파일 확인

```bash
[root@rocky admin]# ls → 마운트 전
 tardir   workdir
[root@rocky admin]# ls  → 마운트 
 a   b   c   d   tardir   workdir
```

## 홈 디렉토리가 아닌 별도의 디렉토리를 공유하는 경우

### Rocky

```bash
[root@rocky admin]# vi /etc/samba/smb.conf
[root@rocky admin]# systemctl restart smb
[root@rocky admin]# useradd -s /sbin/nologin share
[root@rocky admin]# smbpasswd -a share
New SMB password:
Retype new SMB password:
Added user share.
[root@rocky admin]# mkdir /var/samba
[root@rocky admin]#
```

### WSL ubuntu

```bash
root@KHP18:~# mount -t cifs //192.168.235.130/share /mnt/share-dir -o username=share,password=1358
root@KHP18:~#
```

### NFS(Network File System)

네트워크를 통해 다른 시스템(NFS서버)이 공유하고 있는 디렉토리를 파일 시스템처럼 연결(마운트)해서 사용하는 것.

### Rocky

```bash
[root@rocky home]# dnf install nfs-utils
[root@rocky ~]# vi /etc/exports → 공유할 디렉토리 설정
[root@rocky ~]# mkdir /var/nfs-share → 공유할 디렉토리 생성
[root@rocky ~]# systemctl start nfs-server -> NFS 서비스 시
[root@rocky ~]# systemctl enable nfs-server
Created symlink /etc/systemd/system/multi-user.target.wants/nfs-server.service → /usr/lib/systemd/system/nfs-server.service.
```

### Rocky vi /etc/exports 내용

 공유할 디렉토리      클라이언트 주소/네트워크(NFS옵션)
/var/nfs-share           *(rw, insecure)

### WSL ubuntu

```bash

root@KHP18:~# apt-get install nfs-common → 클라이언트에서 NFS서버에서 공유하고 있는 디렉토리 마운트
root@KHP18:~# mount -t nfs 192.168.235.130:/var/nfs-share /mnt/nfs-dir
root@KHP18:~# df -Th
Filesystem                     Type           Size  Used Avail Use% Mounted on
none                           tmpfs          7.8G  4.0K  7.8G   1% /mnt/wsl
drivers                        9p             457G   97G  361G  22% /usr/lib/wsl/drivers
none                           tmpfs          7.8G     0  7.8G   0% /usr/lib/modules
none                           overlay        7.8G     0  7.8G   0% /usr/lib/modules/5.15.153.1-microsoft-standard-WSL2
/dev/sdc                       ext4          1007G  2.4G  954G   1% /
none                           tmpfs          7.8G   92K  7.8G   1% /mnt/wslg
none                           overlay        7.8G     0  7.8G   0% /usr/lib/wsl/lib
rootfs                         rootfs         7.8G  2.1M  7.8G   1% /init
none                           tmpfs          7.8G  936K  7.8G   1% /run
none                           tmpfs          7.8G     0  7.8G   0% /run/lock
none                           tmpfs          7.8G     0  7.8G   0% /run/shm
tmpfs                          tmpfs          4.0M     0  4.0M   0% /sys/fs/cgroup
none                           overlay        7.8G   76K  7.8G   1% /mnt/wslg/versions.txt
none                           overlay        7.8G   76K  7.8G   1% /mnt/wslg/doc
C:\                            9p             457G   97G  361G  22% /mnt/c
snapfuse                       fuse.snapfuse   75M   75M     0 100% /snap/core22/1380
snapfuse                       fuse.snapfuse  128K  128K     0 100% /snap/bare/5
snapfuse                       fuse.snapfuse   74M   74M     0 100% /snap/core22/864
snapfuse                       fuse.snapfuse   92M   92M     0 100% /snap/gtk-common-themes/1535
snapfuse                       fuse.snapfuse   39M   39M     0 100% /snap/snapd/21759
snapfuse                       fuse.snapfuse   41M   41M     0 100% /snap/snapd/20290
snapfuse                       fuse.snapfuse  132M  132M     0 100% /snap/ubuntu-desktop-installer/1276
snapfuse                       fuse.snapfuse  132M  132M     0 100% /snap/ubuntu-desktop-installer/1286
tmpfs                          tmpfs          1.6G  4.0K  1.6G   1% /run/user/0
192.168.235.130:/var/nfs-share nfs4            26G  3.6G   23G  14% /mnt/nfs-dir

```

```bash
WSL ubuntu>
root@KHP18:~# cd /mnt/nfs-dir
root@KHP18:/mnt/nfs-dir# ls
root@KHP18:/mnt/nfs-dir# touch a b c → other 권한 설정 전
touch: cannot touch 'a': Permission denied
touch: cannot touch 'b': Permission denied
touch: cannot touch 'c': Permission denied
root@KHP18:/mnt/nfs-dir# touch a b c → other 권한 설정 후

Rocky>
[root@rocky ~]# ls -ld /var/nfs-share
drwxr-xr-x. 2 root root 6  7월  4 10:23 /var/nfs-share
[root@rocky ~]# chmod o+w /var/nfs-share
[root@rocky ~]# ls -ld /var/nfs-share
drwxr-xrwx. 2 root root 33  7월  4 10:57 /var/nfs-share
[root@rocky ~]# ls /var/nfs-share
a  b  c
[root@rocky ~]# ls -l /var/nfs-share
합계 0
-rw-r--r--. 1 nobody nobody 0  7월  4 10:57 a
-rw-r--r--. 1 nobody nobody 0  7월  4 10:57 b
-rw-r--r--. 1 nobody nobody 0  7월  4 10:57 c

```

### Rocky vi /etc/exports 변경 내용

 공유할 디렉토리      클라이언트 주소/네트워크(NFS옵션)
/var/nfs-share    *(rw, insecure, no_root_squash)

```bash
Rocky>
[root@rocky ~]# vi /etc/exports
[root@rocky ~]# systemctl restart nfs-server

WSL ubuntu>
root@KHP18:/mnt/nfs-dir# cd ..
root@KHP18:/mnt# umount nfs-dir
root@KHP18:/mnt# mount -t nfs 192.168.235.130:/var/nfs-share /mnt/nfs-dir
root@KHP18:/mnt# cd nfs-dir
root@KHP18:/mnt/nfs-dir# touch x y z
root@KHP18:/mnt/nfs-dir# ls -l
total 0
-rw-r--r-- 1 nobody nogroup 0 Jul  4 10:57 a
-rw-r--r-- 1 nobody nogroup 0 Jul  4 10:57 b
-rw-r--r-- 1 nobody nogroup 0 Jul  4 10:57 c
-rw-r--r-- 1 root   root    0 Jul  4 11:01 x
-rw-r--r-- 1 root   root    0 Jul  4 11:01 y
-rw-r--r-- 1 root   root    0 Jul  4 11:01 z

```

## 메일 서비스

메일 서비스 : SMTP(TCP 25번)프로토콜을 이용해서 메시지를 송수신할 수 있는 서비스

            메일 서비스를 위해서는 DNS서비스 필수. (MX resource record 필요)

```bash
[root@rocky named]# vi yhm.zone
[root@rocky named]# systemctl restart named
```

### vi /var/named/yhm.zone 수정

$TTL 86400
@       IN SOA ns   admin (
                    20240703 ; serial
                    1D       ; refresh
                    1H       ; retry
                    1W       ; expire
                    3H )     ; minimum

        IN NS  ns
        IN MX 10 mail

ns      IN A    192.168.235.130
aws     IN A    15.165.76.45
www     IN A    192.168.235.130
mail    IN A    192.168.235.130

![Untitled](Linux04%2066792004c64342ee89e65cd2b5e5663c/Untitled.png)

```powershell
DNS 서버 주소를 Rocky의 DNS로 바꾼 후 확인
C:\Users\KDP>nslookup mail.yhm.com
서버:    UnKnown
Address:  192.168.235.130

이름:    mail.yhm.com
Address:  192.168.235.130
```

```bash
[root@rocky named]# rpm -qa | grep postfix
[root@rocky named]# rpm -qa | grep dovecot
[root@rocky named]# dnf install postfix dovecot
[root@rocky named]# postconf -e 'myhostname = mail.yhm.com'
[root@rocky named]# postconf -e 'mydomain = yhm.com'
[root@rocky named]# postconf -e 'myorigin = $mydomain'
[root@rocky named]# postconf -e 'inet_interfaces = all'
[root@rocky named]# postconf -e 'inet_protocols = ipv4'
[root@rocky named]# postconf -e 'mydestination = $mydomain,locahost,$mydomain,/etc/postfix/local-host-names'
[root@rocky named]# touch /etc/postfix/local-host-names
[root@rocky named]# postconf -e 'smtpd_sasl_type = dovecot'
[root@rocky named]# postconf -e 'smtpd_sasl_path = private/auth'
[root@rocky named]# postconf -e 'smtpd_sasl_auth_enable = yes'
[root@rocky named]# postconf -e 'smtpd_sasl_security_options = noanonymous'
[root@rocky named]# postconf -e 'smtpd_sasl_local_domain = $myhostname'
[root@rocky named]# postconf -e 'smtpd_client_restrictions = permit_mynetworks,permit'
[root@rocky named]# postconf -e 'smtpd_recipient_restrictions = permit_mynetworks,permit_auth_destination,permit_sasl_authenticated,reject'
[root@rocky named]# dnf install dovecot
[root@rocky named]# vi /etc/dovecot/dovecot.conf
[root@rocky named]# vi /etc/dovecot/conf.d/10-auth.conf
[root@rocky named]# vi /etc/dovecot/conf.d/10-mail.conf
[root@rocky named]# vi /etc/dovecot/conf.d/10-master.conf
[root@rocky named]# vi /etc/dovecot/conf.d/10-ssl.conf
[root@rocky named]# systemctl restart postfix dovecot
```

### vi /etc/dovecot/dovecot.conf

- 프로토콜 설정
#protocols = imap pop3 lmtp => protocols = imap pop3
- 리스닝 인터페이스 설정
#listen = *, :: => listen = *

### 비밀번호 인증 설정

vi /etc/dovecot/conf.d/10-auth.conf
#disable_plaintext_auth = yes => disable_plaintext_auth = no

auth_mechanisms = plain => auth_mechanisms = plain login

### 메일박스 위치 변경

vi /etc/dovecot/conf.d/10-mail.conf
#mail_location => mail_location = mbox:/var/empty:INBOX=/var/spool/mail/%u:INDEX=MEMORY

### SMTP 인증 설정

vi /etc/dovecot/conf.d/10-master.conf

#Postfix smtp-auth
unix_listener /var/spool/postfix/private/auth {
mode = 0666
user = postfix
group = postfix
}

vi /etc/dovecot/conf.d/10-ssl.conf

ssl = no

```bash
[root@rocky ~]# telnet localhost 110
Trying ::1...
telnet: connect to address ::1: Connection refused
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
+OK Dovecot ready.
user admin
+OK
pass 1358
+OK Logged in.
-ERR [SYS/TEMP] Internal error occurred. Refer to server log for more informatio
Connection closed by foreign host.
[root@rocky ~]# telnet localhost 25
Trying ::1...
telnet: connect to address ::1: Connection refused
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
220 mail.yhm.com ESMTP Postfix
ehlo yhm.com
250-mail.yhm.com
250-PIPELINING
250-SIZE 10240000
250-VRFY
250-ETRN
250-STARTTLS
250-AUTH PLAIN LOGIN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250-DSN
250 SMTPUTF8
auth login
334 VXNlcm5hbWU6
YWRtaW4=
334 UGFzc3dvcmQ6
MTM1OA==
235 2.7.0 Authentication successful
quit
221 2.0.0 Bye

```