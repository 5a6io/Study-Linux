# Linux03

## 240702 추가

1. 파일시스템
- 새로운 하드디스크를 추가한 경우
1) 파티션 생성 : 파티션은 파일시스템을 생성하기 위한 구역
               fdisk `장치파일` : fdisk `/dev/sda` 
2) 파일시스템 : 파일시스템은 파일을 저장하기 위한 체계. OS마다 다름.
               mkfs -t `파일시스템종류` `파티션` : mkfs -t `ext4` `/dev/sda1`
3) 생성된 파일 시스템을 리눅스에서 사용가능하도록 mount함. → mount는 장치를 리눅스에서 사용가능하도록 연결하는 것.
               mount `연결할 파티션` `연결 지점` : mount /dev/sda1 /mnt/mpoint
               umount `연결 지점` or `연결된 파티션` : umount /mnt/mpoint
4) 시스템 리부팅 후에도 지속적으로 mount된 상태로 사용하기 위해 /etc/fstab 파일에 등록해야 함.
                                                             ↳file system table
               파티션 or 장치        연결 지점       파일 시스템      옵션    dump여부  fsck 순서
                /dev/sda1        /mnt/mpoint        ext4       defaults     0        0

```bash
[root@rocky ~]# ls /mnt/mpoint
lost+found
[root@rocky ~]# df -Th
Filesystem          Type      Size  Used Avail Use% Mounted on
devtmpfs            devtmpfs  1.8G     0  1.8G   0% /dev
tmpfs               tmpfs     1.8G     0  1.8G   0% /dev/shm
tmpfs               tmpfs     1.8G  8.9M  1.8G   1% /run
tmpfs               tmpfs     1.8G     0  1.8G   0% /sys/fs/cgroup
/dev/mapper/rl-root xfs        26G  3.4G   23G  14% /
/dev/nvme0n1p1      xfs      1014M  199M  816M  20% /boot
tmpfs               tmpfs     364M     0  364M   0% /run/user/0
/dev/sda1           ext4      9.8G   24K  9.3G   1% /mnt/mpoint
[root@rocky ~]# umount /mnt/mpoint
[root@rocky ~]# ls /mnt/mpoint
xfs_admin  xfs_db        xfs_fsr     xfs_io         xfs_metadump  xfs_quota   xfs_spaceman
xfs_bmap   xfs_estimate  xfs_growfs  xfs_logprint   xfs_mkfile    xfs_repair  xtables-monitor
xfs_copy   xfs_freeze    xfs_info    xfs_mdrestore  xfs_ncheck    xfs_rtcp    xtables-nft-multi
[root@rocky ~]# fdisk /dev/sda

Welcome to fdisk (util-linux 2.32.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Command (m for help): p
Disk /dev/sda: 10 GiB, 10737418240 bytes, 20971520 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x8f5952c6

Device     Boot Start      End  Sectors Size Id Type
/dev/sda1        2048 20971519 20969472  10G 83 Linux

Command (m for help): d
Selected partition 1
Partition 1 has been deleted.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): o
Value out of range.
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-20971519, default 2048):   → Enter키 눌러서 기본값으로 생성
Last sector, +sectors or +size{K,M,G,T,P} (2048-20971519, default 20971519): +5G

Created a new partition 1 of type 'Linux' and of size 5 GiB.
Partition #1 contains a ext4 signature.

Do you want to remove the signature? [Y]es/[N]o: Y

The signature will be removed by a write command.

Command (m for help): p
Disk /dev/sda: 10 GiB, 10737418240 bytes, 20971520 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x8f5952c6

Device     Boot Start      End  Sectors Size Id Type
/dev/sda1        2048 10487807 10485760   5G 83 Linux

Filesystem/RAID signature on partition 1 will be wiped.

Command (m for help): n
Partition type
   p   primary (1 primary, 0 extended, 3 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (2-4, default 2): 3
First sector (10487808-20971519, default 10487808):
Last sector, +sectors or +size{K,M,G,T,P} (10487808-20971519, default 20971519):

Created a new partition 3 of type 'Linux' and of size 5 GiB.

Command (m for help): p
Disk /dev/sda: 10 GiB, 10737418240 bytes, 20971520 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x8f5952c6

Device     Boot    Start      End  Sectors Size Id Type
/dev/sda1           2048 10487807 10485760   5G 83 Linux
/dev/sda3       10487808 20971519 10483712   5G 83 Linux

Filesystem/RAID signature on partition 1 will be wiped.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
[root@rocky ~]# mkfs -t ext4 /dev/sda1
mke2fs 1.45.6 (20-Mar-2020)
Creating filesystem with 1310720 4k blocks and 327680 inodes
Filesystem UUID: 6b23cf1e-f30e-4066-b610-fc3c66bee928
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done

[root@rocky ~]# mkfs -t xfs /dev/sda3
meta-data=/dev/sda3              isize=512    agcount=4, agsize=327616 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=0 inobtcount=0
data     =                       bsize=4096   blocks=1310464, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
[root@rocky ~]# mkdir /mnt/mpoint2
[root@rocky ~]# mv /mnt/mpoint /mnt/mpoint1
[root@rocky ~]# ls /mnt
mpoint1  mpoint2
[root@rocky ~]# mount -t ext4 /dev/sda1 /mnt/mpoint1
[root@rocky ~]# mount -t xfs /dev/sda3 /mnt/mpoint2
[root@rocky ~]# df -Th
Filesystem          Type      Size  Used Avail Use% Mounted on
devtmpfs            devtmpfs  1.8G     0  1.8G   0% /dev
tmpfs               tmpfs     1.8G     0  1.8G   0% /dev/shm
tmpfs               tmpfs     1.8G  8.9M  1.8G   1% /run
tmpfs               tmpfs     1.8G     0  1.8G   0% /sys/fs/cgroup
/dev/mapper/rl-root xfs        26G  3.4G   23G  14% /
/dev/nvme0n1p1      xfs      1014M  199M  816M  20% /boot
tmpfs               tmpfs     364M     0  364M   0% /run/user/0
/dev/sda1           ext4      4.9G   24K  4.6G   1% /mnt/mpoint1
/dev/sda3           xfs       5.0G   68M  5.0G   2% /mnt/mpoint2
[root@rocky ~]# ls /mnt/mpoint1
lost+found                     → ext는 lost+found 파일이 생성됨. xfs는 생성이 되지 않음.
[root@rocky ~]# ls /mnt/mpoint2
[root@rocky ~]# vi /etc/fstab
			fstab에 추가할 내용.
			15 /dev/sda1               /mnt/mpoint1            ext4    defaults        0 0
			16 /dev/sda3               /mnt/mpoint2            xfs     defaults        0 0
```

[Amazon Elastic Block Store란 무엇인가요? - Amazon EBS](https://docs.aws.amazon.com/ko_kr/ebs/latest/userguide/what-is-ebs.html)

LVM(Logical Volume Manager)↴
RAID                      → 용량 확장.장애 대비(백업)
Linux에서는 기본적으로 LVM을 사용.

## DNS

- bind 설치 : `dnf install bind-chroot`

   `/var/named/chroot/var/named` 

- 설정 파일 : /etc/named.conf
- Caching Name Server : 자신이 관리하는 도메인은 없고, 클라이언트의 요청에 응답만 하는 Name Server
- Zone : 도메인을 가리키며 zone file에 도메인의 호스트 정보를 등록함.
- /etc/named.rfc1912.zones : 도메인을 등록하는 파일이고 도메인 등록 시 zone 파일의 위치를 지정함.
- zone 파일이 저장되는 위치 : /var/named
- Resource Record :  NS(name server), A(IPv4주소), AAAA(IPv6주소),  MX(메일서버), CNAME(canonical name), PTR(pointer)

### Rocky

```bash
[root@rocky ~]# dnf install bind-chroot
[root@rocky ~]# cd /var/named
[root@rocky ~]# ls
chroot  data  dynamic  named.ca  named.empty  named.localhost  named.loopback  slaves
[root@rocky named]# cd chroot
[root@rocky chroot]# ls
dev  etc  proc  run  usr  var
[root@rocky chroot]# cd var
[root@rocky var]# ls
log  named  run  tmp
[root@rocky var]# cd named
[root@rocky named]# vi /etc/named.conf
[root@rocky named]# systemctl start named
[root@rocky named]# cd /etc
[root@rocky etc]# systemctl enable named
Created symlink /etc/systemd/system/multi-user.target.wants/named.service → /usr/lib/systemd/system/named.service.
[root@rocky etc]# systemctl stop firewalld
[root@rocky etc]# less named.conf
[root@rocky etc]# vi named.rfc1912.zones
[root@rocky etc]# vi yhm.zone
[root@rocky etc]# systemctl restart named
```

### WSL

```bash
hyemi@KHP18:~$ nslookup
> server 192.168.235.130
Default server: 192.168.235.130
Address: 192.168.235.130#53
> aws.yhm.com
Server:         192.168.235.130
Address:        192.168.235.130#53

Name:   aws.yhm.com
Address: 15.165.76.45
> www.yhm.com
Server:         192.168.235.130
Address:        192.168.235.130#53

Name:   www.yhm.com
Address: 192.168.235.130

```

![named.rfc1912.zones 맨 마지막에 추가한 내용.](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled.png)

named.rfc1912.zones 맨 마지막에 추가한 내용.

![86400 유효기간 하루.
@ → yhm.com  도메인
SOA → Start Of Authority. 권한의 시작.
NS → name server
A → 호스트 이름에 해당하는 IP 지정. IPv4
name server의 호스트명은 ns.
IN NS ns → ns 뒤에 `.yhm.com.`이 생략되어 있음. 끝에 `.`을 안 찍을 때는 호스트명만. 도메인을 적은 경우 `.`까지 입력.
앞에 공백이 들어가면 안 됨.](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%201.png)

86400 유효기간 하루.
@ → yhm.com  도메인
SOA → Start Of Authority. 권한의 시작.
NS → name server
A → 호스트 이름에 해당하는 IP 지정. IPv4
name server의 호스트명은 ns.
IN NS ns → ns 뒤에 `.yhm.com.`이 생략되어 있음. 끝에 `.`을 안 찍을 때는 호스트명만. 도메인을 적은 경우 `.`까지 입력.
앞에 공백이 들어가면 안 됨.

### AWS Ubuntu

```bash
ubuntu@ip-172-31-11-35:~$ sudo apt install nginx
ubuntu@ip-172-31-11-35:~$ sudo apt install net-tools
```

![Untitled](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%202.png)

![Untitled](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%203.png)

![DNS 서버 주소를 Rocky의 ip로 바꾼 후 DNS로 접속.](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%204.png)

DNS 서버 주소를 Rocky의 ip로 바꾼 후 DNS로 접속.

```bash
[root@rocky etc]# vi named.rfc1912.zones
[root@rocky etc]# cd /var/named
[root@rocky named]# cp yhm.zone naver.zone
[root@rocky named]# cat naver.zone
$TTL 86400
@               IN SOA ns       admin (
                                        20240703 ; serial
                                        1D               ; refresh
                                        1H               ; retry
                                        1W               ; expire
                                        3H )     ; minimum

                IN NS  ns

ns              IN A   192.168.235.130
www             IN A   192.168.235.130
aws             IN A   15.165.76.45
[root@rocky named]# systemctl restart named
[root@rocky named]# vi naver.zone
[root@rocky named]# vi yhm.zone
[root@rocky named]# systemctl restart named
[root@rocky named]# cat yhm.zone
$TTL 86400
@               IN SOA ns       admin (
                                        20240703 ; serial
                                        1D               ; refresh
                                        1H               ; retry
                                        1W               ; expire
                                        3H )     ; minimum

                IN NS  ns

ns              IN A   192.168.235.130
www             IN A   192.168.235.130
aws             IN A   15.165.76.45
www.a.b IN A   192.168.235.100 → 이 naver.zone에서 CNAME으로 이 주소를 찾아가도록 함.
```

![Untitled](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%205.png)

![Untitled](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%206.png)

![Untitled](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%207.png)

### hosts 수정 후 이름 주소로 접근.

```bash
[root@rocky etc]# cat hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
[root@rocky etc]# vi hosts
[root@rocky etc]# cat hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
172.17.184.95   wsl             wsl-ubuntu
15.165.76.45    aws             aws-ubuntu
[root@rocky etc]# ping wsl
PING wsl (172.17.184.95) 56(84) bytes of data.
64 bytes from wsl (172.17.184.95): icmp_seq=1 ttl=128 time=2.06 ms
64 bytes from wsl (172.17.184.95): icmp_seq=2 ttl=128 time=3.15 ms
64 bytes from wsl (172.17.184.95): icmp_seq=3 ttl=128 time=2.45 ms
64 bytes from wsl (172.17.184.95): icmp_seq=4 ttl=128 time=2.61 ms
64 bytes from wsl (172.17.184.95): icmp_seq=5 ttl=128 time=2.43 ms
64 bytes from wsl (172.17.184.95): icmp_seq=6 ttl=128 time=2.35 ms
64 bytes from wsl (172.17.184.95): icmp_seq=7 ttl=128 time=2.46 ms
64 bytes from wsl (172.17.184.95): icmp_seq=8 ttl=128 time=2.07 ms
64 bytes from wsl (172.17.184.95): icmp_seq=9 ttl=128 time=2.04 ms
^C
--- wsl ping statistics ---
9 packets transmitted, 9 received, 0% packet loss, time 8016ms
rtt min/avg/max/mdev = 2.041/2.401/3.146/0.329 ms
[root@rocky etc]# ssh root@aws
The authenticity of host 'aws (15.165.76.45)' can't be established.
ECDSA key fingerprint is SHA256:xP4aJYSCFsUc0dUUzzeicMbeanUGNltEPJCn4WW1rSg.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'aws,15.165.76.45' (ECDSA) to the list of known hosts.
Password:
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 6.5.0-1017-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

  System information as of Wed Jul  3 03:46:17 UTC 2024

  System load:  0.0               Processes:             109
  Usage of /:   26.8% of 7.57GB   Users logged in:       1
  Memory usage: 25%               IPv4 address for eth0: 172.31.11.35
  Swap usage:   0%

 * Ubuntu Pro delivers the most comprehensive open source security and
   compliance features.

   https://ubuntu.com/aws/pro

Expanded Security Maintenance for Applications is not enabled.

57 updates can be applied immediately.
42 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status

Last login: Tue Jul  2 06:28:30 2024 from 115.88.240.250
root@ip-172-31-11-35:~#

```

Windows에도 hosts가 있음.

`C:\Windows\System32\drivers\etc`

### 역방향 zone 설정

### Rocky

```bash
[root@rocky named]# cat /etc/named.rfc1912.zones
// named.rfc1912.zones:
//
// Provided by Red Hat caching-nameserver package
//
// ISC BIND named zone configuration for zones recommended by
// RFC 1912 section 4.1 : localhost TLDs and address zones
// and https://tools.ietf.org/html/rfc6303
// (c)2007 R W Franks
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//
// Note: empty-zones-enable yes; option is default.
// If private ranges should be forwarded, add
// disable-empty-zone "."; into options
//

zone "localhost.localdomain" IN {
        type master;
        file "named.localhost";
        allow-update { none; };
};

zone "localhost" IN {
        type master;
        file "named.localhost";
        allow-update { none; };
};

zone "1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.ip6.arpa" IN {
        type master;
        file "named.loopback";
        allow-update { none; };
};

zone "1.0.0.127.in-addr.arpa" IN {
        type master;
        file "named.loopback";
        allow-update { none; };
};

zone "0.in-addr.arpa" IN {
        type master;
        file "named.empty";
        allow-update { none; };
};

zone "yhm.com" IN {
        type master;
        file "yhm.zone";
};

zone "naver.com" IN {
        type master;
        file "naver.zone";
};

zone "235.168.192.in-addr.arpa" IN {
        type master;
        file "yhm.reverse";
};
[root@rocky named]# cat yhm.zone
$TTL 86400
@               IN SOA ns       admin (
                                        20240703 ; serial
                                        1D               ; refresh
                                        1H               ; retry
                                        1W               ; expire
                                        3H )     ; minimum

                IN NS  ns

ns              IN A    192.168.235.130
aws             IN A    15.165.76.45
www.a.b         IN A    192.168.235.100
www             IN A    192.168.235.10
ftp             IN A    192.168.235.20
db              IN A    192.168.235.30
[root@rocky named]# cat yhm.reverse
$TTL 86400
@               IN SOA ns       admin (
                                        20240703 ; serial
                                        1D               ; refresh
                                        1H               ; retry
                                        1W               ; expire
                                        3H )     ; minimum

                IN NS  ns

ns              IN A   192.168.235.130

10              IN PTR www.yhm.com.
20              IN PTR ftp.yhm.com.
30              IN PTR db.yhm.com.
[root@rocky named]# systemctl restart named
```

### WSL

```bash
hyemi@KHP18:~$ nslookup
> server 192.168.235.130
Default server: 192.168.235.130
Address: 192.168.235.130#53
> 192.168.235.10
10.235.168.192.in-addr.arpa     name = www.yhm.com.
> 192.168.235.20
20.235.168.192.in-addr.arpa     name = ftp.yhm.com.
> 192.168.235.30
30.235.168.192.in-addr.arpa     name = db.yhm.com.

```

## 파일 공유 서비스 : FTP, Samba, NFS

1) FTP : FTP서버에 파일을 저장하고 클라이언트에서 파일을 다운로드하거나 업로드하는 서비스. VSFTP 서버 프로그램을 사용하여 FTP서비스(TCP 21(session), 데이터포트(20, 또는 임의의 포트).

- FTP 모드
  Active
           Client                     Server
         임의의 포트 ———————————————————→ 21
                    클라이언트가 서버에게
                  접속할 데이터 포트 알려줌

          Client가 ←——————————————————— 20
         알려준 포트        데이터
  Passive
          Client                     Server
        임의의 포트 ———————————————————→ 21
                    서버가 클라이언트에게
                  접속할 데이터 포트 알려줌

        임의의 포트 ——————————————————→ 서버가
                        데이터       알려준 포트

  - chroot : local_enable=YES 설정 시 사용자가 로그인 했을 경우 해당 사용자의 홈 디렉토리를  ‘/’로 인식하도록 설정.
        vi /etc/vsftpd.conf
        chroot_local_user=YES
        chroot_list_enable=YES
        chroot_list_file=/etc/vsftpd.chroot_list
        allow_writeable_chroot=YES

  - passive 모드 사용 시 포트 지정
        pasv_min_port=5001
        pasv_max_port=5002
        pasv_address=XX.XX.XX.XX

### Rocky

```bash
[root@rocky named]# dnf install vsftpd
[root@rocky named]# netstat -lntp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 192.168.235.130:53      0.0.0.0:*               LISTEN      2891/named
tcp        0      0 127.0.0.1:53            0.0.0.0:*               LISTEN      2891/named
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      954/sshd
tcp        0      0 127.0.0.1:953           0.0.0.0:*               LISTEN      2891/named
tcp6       0      0 :::21                   :::*                    LISTEN      3030/vsftpd
tcp6       0      0 :::22                   :::*                    LISTEN      954/sshd
tcp6       0      0 ::1:953                 :::*                    LISTEN      2891/named

```

### Windows

```bash
PS C:\Users\KDP> ftp 192.168.235.130
192.168.235.130에 연결되었습니다.
220 (vsFTPd 3.0.3)
200 Always in UTF8 mode.
사용자(192.168.235.130:(none)): admin
331 Please specify the password.
암호:

230 Login successful.
ftp>
```

![파일 탐색기로 접속.
netstat -ntlp : n(numeric), t(tcp), l(listening), p(pid)](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%208.png)

파일 탐색기로 접속.
netstat -ntlp : n(numeric), t(tcp), l(listening), p(pid)

### 익명으로 ftp 접속.

```powershell
[root@rocky named]# cd /etc/vsftpd
[root@rocky vsftpd]# ls
ftpusers  user_list  vsftpd.conf  vsftpd_conf_migrate.sh
[root@rocky vsftpd]# vi vsftpd.conf
[root@rocky vsftpd]# systemctl restart vsftpd
```

![anonymous_enable=YES → 익명 사용자로 로그인 가능. 패스워드X.
local_enable=YES → 서버 사용자로 로그인 가능.](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%209.png)

anonymous_enable=YES → 익명 사용자로 로그인 가능. 패스워드X.
local_enable=YES → 서버 사용자로 로그인 가능.

```powershell
Rocky>
PS C:\Users\KDP> ftp 192.168.235.130
192.168.235.130에 연결되었습니다.
220 (vsFTPd 3.0.3)
200 Always in UTF8 mode.
사용자(192.168.235.130:(none)): anonymous
331 Please specify the password.
암호:

230 Login successful.
ftp>

AWS ubuntu>
PS C:\Users\KDP> ftp 15.165.76.45
15.165.76.45에 연결되었습니다.
220 (vsFTPd 3.0.5)
200 Always in UTF8 mode.
사용자(15.165.76.45:(none)): ubuntu
331 Please specify the password.
암호:

230 Login successful.
ftp>
```

```powershell
ubuntu@ip-172-31-11-35:/etc$ cat /etc/vsftpd.conf
# Example config file /etc/vsftpd.conf
#
# The default compiled in settings are fairly paranoid. This sample file
# loosens things up a bit, to make the ftp daemon more usable.
# Please see vsftpd.conf.5 for all compiled in defaults.
#
# READ THIS: This example file is NOT an exhaustive list of vsftpd options.
# Please read the vsftpd.conf.5 manual page to get a full idea of vsftpd's
# capabilities.
#
#
# Run standalone?  vsftpd can run either from an inetd or as a standalone
# daemon started from an initscript.
listen=YES → IPv4로만 접근
#
# This directive enables listening on IPv6 sockets. By default, listening
# on the IPv6 "any" address (::) will accept connections from both IPv6
# and IPv4 clients. It is not necessary to listen on *both* IPv4 and IPv6
# sockets. If you want that (perhaps because you want to listen on specific
# addresses) then you must run two copies of vsftpd with two configuration
# files.
listen_ipv6=NO → IPv4, IPv6 접근
#
# Allow anonymous FTP? (Disabled by default).
anonymous_enable=NO
#
# Uncomment this to allow local users to log in.
local_enable=YES
#
# Uncomment this to enable any form of FTP write command.
#write_enable=YES
#
# Default umask for local users is 077. You may wish to change this to 022,
# if your users expect that (022 is used by most other ftpd's)
#local_umask=022
#
# Uncomment this to allow the anonymous FTP user to upload files. This only
# has an effect if the above global write enable is activated. Also, you will
# obviously need to create a directory writable by the FTP user.
#anon_upload_enable=YES
#
# Uncomment this if you want the anonymous FTP user to be able to create
# new directories.
#anon_mkdir_write_enable=YES
#
# Activate directory messages - messages given to remote users when they
# go into a certain directory.
dirmessage_enable=YES
#
# If enabled, vsftpd will display directory listings with the time
# in  your  local  time  zone.  The default is to display GMT. The
# times returned by the MDTM FTP command are also affected by this
# option.
use_localtime=YES
#
# Activate logging of uploads/downloads.
xferlog_enable=YES
#
# Make sure PORT transfer connections originate from port 20 (ftp-data).
connect_from_port_20=YES
#
# If you want, you can arrange for uploaded anonymous files to be owned by
# a different user. Note! Using "root" for uploaded files is not
# recommended!
#chown_uploads=YES
#chown_username=whoever
#
# You may override where the log file goes if you like. The default is shown
# below.
#xferlog_file=/var/log/vsftpd.log
#
# If you want, you can have your log file in standard ftpd xferlog format.
# Note that the default log file location is /var/log/xferlog in this case.
#xferlog_std_format=YES
#
# You may change the default value for timing out an idle session.
#idle_session_timeout=600
#
# You may change the default value for timing out a data connection.
#data_connection_timeout=120
#
# It is recommended that you define on your system a unique user which the
# ftp server can use as a totally isolated and unprivileged user.
#nopriv_user=ftpsecure
#
# Enable this and the server will recognise asynchronous ABOR requests. Not
# recommended for security (the code is non-trivial). Not enabling it,
# however, may confuse older FTP clients.
#async_abor_enable=YES
#
# By default the server will pretend to allow ASCII mode but in fact ignore
# the request. Turn on the below options to have the server actually do ASCII
# mangling on files when in ASCII mode.
# Beware that on some FTP servers, ASCII support allows a denial of service
# attack (DoS) via the command "SIZE /big/file" in ASCII mode. vsftpd
# predicted this attack and has always been safe, reporting the size of the
# raw file.
# ASCII mangling is a horrible feature of the protocol.
#ascii_upload_enable=YES
#ascii_download_enable=YES
#
# You may fully customise the login banner string:
#ftpd_banner=Welcome to blah FTP service.
#
# You may specify a file of disallowed anonymous e-mail addresses. Apparently
# useful for combatting certain DoS attacks.
#deny_email_enable=YES
# (default follows)
#banned_email_file=/etc/vsftpd.banned_emails
#
# You may restrict local users to their home directories.  See the FAQ for
# the possible risks in this before using chroot_local_user or
# chroot_list_enable below.
#chroot_local_user=YES
#
# You may specify an explicit list of local users to chroot() to their home
# directory. If chroot_local_user is YES, then this list becomes a list of
# users to NOT chroot().
# (Warning! chroot'ing can be very dangerous. If using chroot, make sure that
# the user does not have write access to the top level directory within the
# chroot)
#chroot_local_user=YES
#chroot_list_enable=YES
# (default follows)
#chroot_list_file=/etc/vsftpd.chroot_list
#
# You may activate the "-R" option to the builtin ls. This is disabled by
# default to avoid remote users being able to cause excessive I/O on large
# sites. However, some broken FTP clients such as "ncftp" and "mirror" assume
# the presence of the "-R" option, so there is a strong case for enabling it.
#ls_recurse_enable=YES
#
# Customization
#
# Some of vsftpd's settings don't fit the filesystem layout by
# default.
#
# This option should be the name of a directory which is empty.  Also, the
# directory should not be writable by the ftp user. This directory is used
# as a secure chroot() jail at times vsftpd does not require filesystem
# access.
secure_chroot_dir=/var/run/vsftpd/empty
#
# This string is the name of the PAM service vsftpd will use.
pam_service_name=vsftpd
#
# This option specifies the location of the RSA certificate to use for SSL
# encrypted connections.
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=NO

#
# Uncomment this to indicate that vsftpd use a utf8 filesystem.
#utf8_filesystem=YES
pasv_min_port=5001
pasv_max_port=5002
pasv_address=15.165.76.45 → 서버가 private주소를 가지고 있는 경우 공인주소를 설정함.
ubuntu@ip-172-31-11-35:/etc$ sudo systemctl restart vsftpd
```

인바운드 규칙에 TCP 5001, 5002 추가 후 알드라이브 접속 시도.

![Untitled](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%2010.png)

![Untitled](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%2011.png)

```powershell
ubuntu@ip-172-31-11-35:~$ sudo vi /etc/vsftpd.conf
120 # the user does not have write access to the top level directory within the
121 # chroot)
122 chroot_local_user=YES
123 chroot_list_enable=YES
124 # (default follows)
125 chroot_list_file=/etc/vsftpd.chroot_list
126 allow_writeable_chroot=YES
```

![ubuntu 사용자가 chroot_list에 없기 때문에 본인의 홈디렉토리를 ‘/’로 인식.](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%2012.png)

ubuntu 사용자가 chroot_list에 없기 때문에 본인의 홈디렉토리를 ‘/’로 인식.

![chroot_list에 admin을 추가하였을 때 모든 디렉토리 탐색 가능.](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%2013.png)

chroot_list에 admin을 추가하였을 때 모든 디렉토리 탐색 가능.

echo는 빌트인 명령어로 setuid가 걸려있지 않아 sudo를 붙여도 쓸 수가 없음. 아래와 같이 실행하여 추가.

```powershell
ubuntu@ip-172-31-11-35:/etc$ echo "admin2" | sudo tee -a vsftpd.chroot_list
admin2
ubuntu@ip-172-31-11-35:/etc$ cat vsftpd.chroot_list
admin
admin2
```

2) Samba : Windows와 Linux 사이의 파일 및 장치 간 공유서비스.
           smb, cifs 프로토콜을 사용하여 공유.
           윈도우에서는 폴더 공유하면 리눅스에서 해당 폴더에 접근 가능.
           리눅스에서는 Samba 패키지(서버) 설치, Samba-client 패키지 설치.
           네트워크 드라이브로 연결해서 직접 연결된 드라이브처럼 사용 가능.
           TCP 137, 139, 445번 포트 사용.
   - dnf install samba ⇒ samba 서버 패키지 설치
     systemctl start smbd
     systemctl start nmbd ⇒ ip 대신 컴퓨터 이름으로 접근 가능.

     /etc/samba/smb.conf ⇒ samba 설정 파일. 기본값은 로컬 사용자의 홈디렉토리를 공유함.
     - samba 서비스를 사용하기 위해서는 samba 사용자로 등록해야 함.
       sambapasswd -a 사용자아이디 : `smbpasswd -a admin`

![Untitled](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%2014.png)

```bash
[root@rocky home]# dnf install samba
[root@rocky home]# cd /etc/samba
[root@rocky samba]# ls
lmhosts  smb.conf  smb.conf.example
[root@rocky samba]# systemctl start smb
[root@rocky samba]# netstat -ntlp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:139             0.0.0.0:*               LISTEN      4081/smbd
tcp        0      0 192.168.235.130:53      0.0.0.0:*               LISTEN      2891/named
tcp        0      0 127.0.0.1:53            0.0.0.0:*               LISTEN      2891/named
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      954/sshd
tcp        0      0 127.0.0.1:953           0.0.0.0:*               LISTEN      2891/named
tcp        0      0 0.0.0.0:445             0.0.0.0:*               LISTEN      4081/smbd
tcp6       0      0 :::139                  :::*                    LISTEN      4081/smbd
tcp6       0      0 :::21                   :::*                    LISTEN      3339/vsftpd
tcp6       0      0 :::22                   :::*                    LISTEN      954/sshd
tcp6       0      0 ::1:953                 :::*                    LISTEN      2891/named
tcp6       0      0 :::445                  :::*                    LISTEN      4081/smbd
[root@rocky samba]# systemctl start nmb
[root@rocky samba]# netstat -ntlp
[root@rocky samba]# smbpasswd -a admin
New SMB password:
Retype new SMB password:
Added user admin.
[root@rocky samba]# setenforce 0
[root@rocky samba]# getenforce
Permissive
```

![윈도우에서 연결하여 사용. ⇒ 인증 후 연결](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%2015.png)

윈도우에서 연결하여 사용. ⇒ 인증 후 연결

![Windows 방화벽을 내리면 숨겨진 파일을 볼 수 있음.](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%2016.png)

Windows 방화벽을 내리면 숨겨진 파일을 볼 수 있음.

![Untitled](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%2017.png)

![Drive처럼 사용 가능.](Linux03%20575c929534844a7eaea5f2f4a9c99362/Untitled%2018.png)

Drive처럼 사용 가능.