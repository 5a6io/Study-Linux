# Linux01

Files & media: 0701%25EC%2588%2598%25EC%2597%2585.txt, Linux_%25EC%258B%259C%25EC%258A%25A4%25ED%2585%259C_%25EA%25B4%2580%25EB%25A6%25AC.pdf, %25EB%25A6%25AC%25EB%2588%2585%25EC%258A%25A4%25EC%2588%2598%25EC%2597%2585.txt

[Linux 시스템 관리.pdf](Linux01%2057c7c6da1a54473bafe76ffcddebb699/Linux_%25EC%258B%259C%25EC%258A%25A4%25ED%2585%259C_%25EA%25B4%2580%25EB%25A6%25AC.pdf)

## 개인키를 이용한 SSH 서비스 접속

SSH 인증 시 공개키. 인증 후 공개키와 개인키 모두 사용.

### 1. 개인키/공개키 생성

```bash
hyemi@KHP18:~/.ssh$ ssh-keygen -t rsa -b 2048 -> 키 생성.
Generating public/private rsa key pair.
Enter file in which to save the key (/home/hyemi/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/hyemi/.ssh/id_rsa
Your public key has been saved in /home/hyemi/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:nGWrHCQQNxbDSDupwtRXT1xmATEwfGrqaUmdd/0UL14 hyemi@KHP18
The key's randomart image is:
+---[RSA 2048]----+
|   .++B=o=+=.    |
|  . .*o++o+      |
| . .+.. +.o      |
|o  ... * + .   . |
|...   + S . .   o|
| .   o + + . ...E|
|    o o + .  .oo |
|     =        .. |
|    .            |
+----[SHA256]-----+
hyemi@KHP18:~/.ssh$ ls -l
total 16
-rw------- 1 hyemi hyemi 1856 Jul  1 09:21 id_rsa -> 개인키
-rw-r--r-- 1 hyemi hyemi  393 Jul  1 09:21 id_rsa.pub -> 공개키
-rw------- 1 hyemi hyemi  978 Jun 28 11:19 known_hosts
-rw-r--r-- 1 hyemi hyemi  142 Jun 28 11:19 known_hosts.old
hyemi@KHP18:~/.ssh$ cp id_rsa.pub authorized_keys -> 복사
hyemi@KHP18:~/.ssh$ ls
authorized_keys  id_rsa  id_rsa.pub  known_hosts  known_hosts.old
-> authorized_keys에는 공개키가 있음.
hyemi@KHP18:~/.ssh$ cp id_rsa /mnt/c/Users/KDP/
```

![cp id_rsa /mnt/c/Users/KDP/ 명령어를 사용해 ubuntu서버에 있는 개인키를 Windows로 가져옴.](Linux01%2057c7c6da1a54473bafe76ffcddebb699/Untitled.png)

cp id_rsa /mnt/c/Users/KDP/ 명령어를 사용해 ubuntu서버에 있는 개인키를 Windows로 가져옴.

```bash
hyemi@KHP18:~/.ssh$ ip a -> ip 확인
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet 10.255.255.254/32 brd 10.255.255.254 scope global lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:15:5d:52:ad:8c brd ff:ff:ff:ff:ff:ff
    inet 172.17.184.95/20 brd 172.17.191.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::215:5dff:fe52:ad8c/64 scope link
       valid_lft forever preferred_lft forever
```

![MobaXterm으로 개인키를 사용하여 우분투에 접속.](Linux01%2057c7c6da1a54473bafe76ffcddebb699/Untitled%201.png)

MobaXterm으로 개인키를 사용하여 우분투에 접속.

### WSL

```bash
hyemi@KHP18:~/.ssh$ cp /mnt/c/Users/KDP/yhm-keypair.pem .
hyemi@KHP18:~/.ssh$ ls
authorized_keys  id_rsa  id_rsa.pub  known_hosts  known_hosts.old  yhm-keypair.pem*
hyemi@KHP18:~/.ssh$ ssh-keygen -t rsa -b 2048
Generating public/private rsa key pair.
Enter file in which to save the key (/home/hyemi/.ssh/id_rsa): id_rsa3
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa3
Your public key has been saved in id_rsa3.pub
The key fingerprint is:
SHA256:pet81+RmTws/Ip3WmejdmEQDyXiEGrw6l5O+eA7VQds hyemi@KHP18
The key's randomart image is:
+---[RSA 2048]----+
|       .  ...    |
|        o..* .   |
|         += E    |
|        o+ o .   |
|       .So.   o  |
|      o.=.   ... |
|      .+..  .== +|
|       =o ...BOOo|
|      .o=o .+++=+|
+----[SHA256]-----+
hyemi@KHP18:~/.ssh$ ls
authorized_keys  id_rsa  id_rsa.pub  id_rsa3  id_rsa3.pub  known_hosts  known_hosts.old  y
hyemi@KHP18:~/.ssh$ chmod 400 yhm-keypair.pem
hyemi@KHP18:~/.ssh$ scp -i yhm-keypair.pem id_rsa3.pub ubuntu@43.202.46.184:/home/ubuntu  /.ssh
The authenticity of host '43.202.46.184 (43.202.46.184)' can't be established.
ED25519 key fingerprint is SHA256:O7V6FI8nEolK2fV3xmEYVu3YVacjh/rwkqEW8pmqNAY.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '43.202.46.184' (ED25519) to the list of known hosts.
id_rsa3.pub                                           100%  393    59.5KB/s   00:00
hyemi@KHP18:~/.ssh$ cp id_rsa3 /mnt/c/Users/KDP/ -> 개인키를 windows에 복사

```

### AWS ubuntu

```bash
ubuntu@ip-172-31-11-35:~/.ssh$ ls
authorized_keys  id_rsa  id_rsa.pub
ubuntu@ip-172-31-11-35:~/.ssh$ mv id_rsa id_rsa2 -> id_rsa를 id_rsa2로 이름 변경.
ubuntu@ip-172-31-11-35:~/.ssh$ cat id_rsa.pub >> authorized_keys -> id_rsa2의 공개키 내용을 authorized_keys로 append.
ubuntu@ip-172-31-11-35:~/.ssh$ ls
authorized_keys  id_rsa  id_rsa.pub
ubuntu@ip-172-31-11-35:~/.ssh$ ls
authorized_keys  id_rsa.pub  id_rsa2  id_rsa3.pub
ubuntu@ip-172-31-11-35:~/.ssh$ cat id_rsa3.pub >> authorized_keys ->id_rsa3.pub 내용을 authorized_key에 append.
ubuntu@ip-172-31-11-35:~/.ssh$ cat authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQClQBqMSgjcolsctma9A/zJ/4H7RWhCPK0f3wmALB0o3bn0PUl/YMRWrv5MVsY3Hys/lm/maa9iniCA4HNWSREAd/uhqD3ldRquLC9HZc841M6Y3dumVsKwwjXOJ4wh8Nqly1HPKvQ2Q0jfsL7OPDLs8h0WjrLxNV+PKmUYNRvVX1AFD4ixknpPhVJIzEDB1sq663qtotrT1haTurSF4Dqr7t5FCQccWCcNLfavWbjSxdMvA8qhyu3KTkgHE0cs5A5/EdZlu+wF0M7oz5FH5LUt7uQVc4HwvqLpdjmHffhsp0Esm6I5H1VoBUcj6ZQgLqvDdDFWhtzpSdWrl98fS1Kj yhm-keypair -> aws에서 서버 생성 당시 사용한 키
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAAgQC4xQGSNWALiIFPSBdz5Kw+57a3+mowzCmoE9MMRvGZghL4isGV03F/dQMonby4gWnjSNVVQO7XC+/jqcUTwPO4h7/aOM648CpLmkYUwewEFx0OoxldIKoPyyYbxFIbM6dz1mWrRDCMyIJFKJOHJbutKtzgirK4AkG7KyQx60/UPQ== ubuntu@ip-172-31-11-35 -> aws에서 ssh를 이용하여 만든 rsa2의 공개키
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDSAmgRxwcxdVs4Xn1uYH90XwWZDE0e2VSN/5Zryczf8GGHE5ckDZwkG0B5k5AROeBEa4noWJiVY8yxuJlsQYiKyHf9z5c5YD0B2EUHUX/evrppMv0iwvM7Xpq7VqvIkGqgZKW1HnZZ0OTMfw4gp9EfOrUYXDWio3fh9Waj0rblql8xlMbWToqGONhqgjQd3rr0HYvHy2IAHI9MCD8Ray5iXK/LyTAnIn+rsw/A6IWMhyijhaKPYNF6gkWdI4g37Ct+OyvexITgRVlXa77+P1R4iOGeAODlMIt4bngj3M0I3E9qMR8YnjLVxVLb+XRWjzApWL/G6t2lsBgxVyBeK+1P hyemi@KHP18-> wsl에서 ssh를 이용하여 만든 rsa3의 공개키
```

### Windows

```powershell
PS C:\Users\KDP> ssh -i id_rsa3 ubuntu@43.202.46.184
Enter passphrase for key 'id_rsa3':
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 6.5.0-1017-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

  System information as of Mon Jul  1 01:54:29 UTC 2024

  System load:  0.0               Processes:             102
  Usage of /:   20.8% of 7.57GB   Users logged in:       1
  Memory usage: 22%               IPv4 address for eth0: 172.31.11.35
  Swap usage:   0%

Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status

The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Mon Jul  1 01:30:00 2024 from 115.88.240.250
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@ip-172-31-11-35:~$
```

authorized_keys에 id_rsa3의 공개키 내용이 있어서 windows에서 id_rsa3의 개인키로 접속이 가능.

### Rocky

```bash
PS C:\Users\KDP> ssh -i id_rsa3 root@192.168.235.130
The authenticity of host '192.168.235.130 (192.168.235.130)' can't be established.
ED25519 key fingerprint is SHA256:4DxrfuqWwmTzYQjXd+nIeu2ejj5IgWMNB3l3qyve7Iw.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.235.130' (ED25519) to the list of known hosts.
Enter passphrase for key 'id_rsa3':
Last login: Mon Jul  1 01:16:33 2024 from 192.168.235.1
[root@localhost ~]# hostnamectl set-hostname rocky
[root@localhost ~]# exit
logout
Connection to 192.168.235.130 closed.
PS C:\Users\KDP> ssh -i id_rsa3 root@192.168.235.130
Enter passphrase for key 'id_rsa3':
Last login: Mon Jul  1 01:29:53 2024 from 192.168.235.1
[root@rocky ~]#
```

키를 잃어버린 경우

## 파일 및 디렉토리 관련 명령어

1. ls(list): 디렉토리의 파일 목록 출력
-l: 파일의 자세한 정보 출력
-a: 모든 파일 출력 (숨김파일 포함)
-h: 파일크기를 읽기 쉽게 출력
ls `디렉토리명`: 디렉토리 내용을 볼 수 있음.

```bash
hyemi@KHP18:~$ ls --help
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      with -l, scale sizes by SIZE when printing them;
                               e.g., '--block-size=M'; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first
  -C                         list entries by columns
      --color[=WHEN]         colorize the output; WHEN can be 'always' (default
                               if omitted), 'auto', or 'never'; more info below
  -d, --directory            list directories themselves, not their contents
  -D, --dired                generate output designed for Emacs' dired mode
  -f                         do not sort, enable -aU, disable -ls --color
  -F, --classify             append indicator (one of */=>@|) to entries
      --file-type            likewise, except do not append '*'
      --format=WORD          across -x, commas -m, horizontal -x, long -l,
                               single-column -1, verbose -l, vertical -C
      --full-time            like -l --time-style=full-iso
  -g                         like -l, but do not list owner
      --group-directories-first
                             group directories before files;
                               can be augmented with a --sort option, but any
                               use of --sort=none (-U) disables grouping
  -G, --no-group             in a long listing, don't print group names
  -h, --human-readable       with -l and -s, print sizes like 1K 234M 2G etc.
      --si                   likewise, but use powers of 1000 not 1024
  -H, --dereference-command-line
                             follow symbolic links listed on the command line
      --dereference-command-line-symlink-to-dir
                             follow each command line symbolic link
                               that points to a directory
      --hide=PATTERN         do not list implied entries matching shell PATTERN
                               (overridden by -a or -A)
      --hyperlink[=WHEN]     hyperlink file names; WHEN can be 'always'
                               (default if omitted), 'auto', or 'never'
      --indicator-style=WORD  append indicator with style WORD to entry names:
                               none (default), slash (-p),
                               file-type (--file-type), classify (-F)
  -i, --inode                print the index number of each file
  -I, --ignore=PATTERN       do not list implied entries matching shell PATTERN
  -k, --kibibytes            default to 1024-byte blocks for disk usage;
                               used only with -s and per directory totals
  -l                         use a long listing format
  -L, --dereference          when showing file information for a symbolic
                               link, show information for the file the link
                               references rather than for the link itself
  -m                         fill width with a comma separated list of entries
  -n, --numeric-uid-gid      like -l, but list numeric user and group IDs
  -N, --literal              print entry names without quoting
  -o                         like -l, but do not list group information
  -p, --indicator-style=slash
                             append / indicator to directories
  -q, --hide-control-chars   print ? instead of nongraphic characters
      --show-control-chars   show nongraphic characters as-is (the default,
                               unless program is 'ls' and output is a terminal)
  -Q, --quote-name           enclose entry names in double quotes
      --quoting-style=WORD   use quoting style WORD for entry names:
                               literal, locale, shell, shell-always,
                               shell-escape, shell-escape-always, c, escape
                               (overrides QUOTING_STYLE environment variable)
  -r, --reverse              reverse order while sorting
  -R, --recursive            list subdirectories recursively
  -s, --size                 print the allocated size of each file, in blocks
  -S                         sort by file size, largest first
      --sort=WORD            sort by WORD instead of name: none (-U), size (-S),
                               time (-t), version (-v), extension (-X)
      --time=WORD            change the default of using modification times;
                               access time (-u): atime, access, use;
                               change time (-c): ctime, status;
                               birth time: birth, creation;
                             with -l, WORD determines which time to show;
                             with --sort=time, sort by WORD (newest first)
      --time-style=TIME_STYLE  time/date format with -l; see TIME_STYLE below
  -t                         sort by time, newest first; see --time
  -T, --tabsize=COLS         assume tab stops at each COLS instead of 8
  -u                         with -lt: sort by, and show, access time;
                               with -l: show access time and sort by name;
                               otherwise: sort by access time, newest first
  -U                         do not sort; list entries in directory order
  -v                         natural sort of (version) numbers within text
  -w, --width=COLS           set output width to COLS.  0 means no limit
  -x                         list entries by lines instead of by columns
  -X                         sort alphabetically by entry extension
  -Z, --context              print any security context of each file
  -1                         list one file per line.  Avoid '\n' with -q or -b
      --help     display this help and exit
      --version  output version information and exit
```

1. cd(change directory): 작업 디렉토리 변경
cd `변경할 디렉토리`
cd - : 현재 디렉토리로 오기 전 디렉토리로 이동
cd    : 홈디렉토리 ⇒ /home/사용자명
cd ~: 홈디렉토리
cd . : 현재 디렉토리로 이동
cd ..:상위 디렉토리로 이동
- 절대경로: ‘/’부터 전체 경로를 모두 표시. ex) /home/hyemi/lab
                      /var/log/messages
- 상대경로: 현재 위치를 기준으로 표시 .  ex) ../home/hyemi/lab
                     #pwd
                        /home/ubuntu
                     #ls .ssh
                     # pwd
                        /home
                     # ls .ssh ⇒ 오류발생
                     #ls ubuntu/.ssh
2. cp(copy): 파일복사
cp 원본파일 복사할 디렉토리 or 복사 파일
-r : 하위 디렉토리까지 모두 복사
       cp -r /etc/* /tmp → 반복해서 밑에 있는 모든 디렉토리 복사

```bash
hyemi@KHP18:~/lab$ cp /etc/profile . -> etc디렉토리에 있는 profile를 현재 위치로 복사
hyemi@KHP18:~/lab$ ls
profile
hyemi@KHP18:~/lab$ cp /etc/profile ./profile2 -> etc디렉토리에 있는 profile을 profile2라는 이름으로 변경 후 복사
hyemi@KHP18:~/lab$ ls
profile  profile2
```

1. mv(move): 파일/디렉토리 이동, 파일명 변경
mv `옮길 디렉토리` `옮겨갈 디렉토리`
ex) mv /lab /home/hyemi ⇒ 디렉토리를 옮김.
      mv /lab /home/hyemi/lab2 ⇒ 디렉토리명을 변경하고 옮김.
      mv test test2 ⇒ 파일명 변경.

```bash
hyemi@KHP18:~$ mv lab lab2
hyemi@KHP18:~$ ls
downloaded-client-config.ovpn                  keys  yhm-keypair.pem
downloaded-client-config.ovpn:Zone.Identifier  lab2  yhm-keypair.pem:Zone.Identifier
hyemi@KHP18:~$ cd lab2
hyemi@KHP18:~/lab2$ ls
lab
hyemi@KHP18:~/lab2$ mv lab ../
hyemi@KHP18:~/lab2$ ls
hyemi@KHP18:~/lab2$ cd ..
hyemi@KHP18:~$ ls
downloaded-client-config.ovpn                  keys  lab2             yhm-keypair.pem:Zone.Identifier
downloaded-client-config.ovpn:Zone.Identifier  lab   yhm-keypair.pem
hyemi@KHP18:~$ mv lab lab3
hyemi@KHP18:~$ ls
downloaded-client-config.ovpn                  keys  lab3             yhm-keypair.pem:Zone.Identifier
downloaded-client-config.ovpn:Zone.Identifier  lab2  yhm-keypair.pem
hyemi@KHP18:~$

```

1. mkdir: 디렉토리 생성
mkdir /`디렉토리 명` → 절대 경로. root 디렉토리에 파일 생성. 관리자 권한 필요.
mkdir `생성할 디렉토리명` → 현재 위치에 디렉토리 생성
mkdir -p a/b/c/d → 상위에 없는 디렉토리를 생성 후 d 디렉토리를 생성.

```bash
hyemi@KHP18:~$ mkdir a/b/c/d
mkdir: cannot create directory ‘a/b/c/d’: No such file or directory
hyemi@KHP18:~$ mkdir -p a/b/c/d
hyemi@KHP18:~$ ls
a                              downloaded-client-config.ovpn:Zone.Identifier  lab2  yhm-keypair.pem
downloaded-client-config.ovpn  keys                                           lab3  yhm-keypair.pem:Zone.Identifier
hyemi@KHP18:~$ cd a
hyemi@KHP18:~/a$ ls
b
hyemi@KHP18:~/a$ cd b
hyemi@KHP18:~/a/b$ ls
c
hyemi@KHP18:~/a/b$ cd c
hyemi@KHP18:~/a/b/c$ ls
d
```

1. rmdir(remove directory): 비어있는 경우 디렉토리 삭제.

```bash
hyemi@KHP18:~$ tree a
a
└── b
    └── c
        └── d

3 directories, 0 files
hyemi@KHP18:~$ rmdir a/b/c/d
hyemi@KHP18:~$ tree a
a
└── b
    └── c

2 directories, 0 files
hyemi@KHP18:~$ rmdir a/b/c -> c 디렉토리만 지워짐.

```

1. rm(remove): 파일 삭제
-r: 디렉토리까지 삭제

```bash
hyemi@KHP18:~$ rm -r a
hyemi@KHP18:~$ ls
downloaded-client-config.ovpn                  keys  lab3             yhm-keypair.pem:Zone.Identifier
downloaded-client-config.ovpn:Zone.Identifier  lab2  yhm-keypair.pem
```

## 포트 번호 변경 후 접속

`WSL`

```bash
sudo vi sshd_config
PasswordAuthentication을 no에서 yes로 변경.
port 22를 2222로 변경
service sshd restart
```

`Windows`

```bash
ssh -i id_rsa3 hyemi@172.17.184.95 -p 2222 으로 접속.
```

리눅스 확장자 개념X.

## John the Ripper 다운

```bash
Rocky>
dnf install wget
wget https://www.openwall.com/john/k/john-1.9.0-jumbo-1.tar.xz
xz -d john-1.9.0-jumbo-1.tar.xz
dnf install tar
tar xf john-1.9.0-jumbo-1.tar
cd john-1.9.0-jumbo-1
dnf install gcc openssl openssl-devel
cd src
./configure
dnf install make
make -s clean && make -sj4

ls /etc/shadow -> 사용자들의 password가 담겨 있음.
./john /etc/shadow -> run파일 위치에서 원래 값들을 찾아냄.
```

## vi editor 명령어

`h, j ,k, l` : `우, 하, 상, 좌` 움직일 수 있음.

`o` :새로운 줄 아래로 삽입

`O` : 새로운 줄 위로 삽입

`i, a, o` : 입력 키

`u` : undo

`U` : 모두 다 undo

`x` : 한 자씩 지움.

`dw` : 한 단어를 지움.

`cw` : 한 단어를 지우고 입력 모드로 바뀜.

`p` : paste

`yy` :  한 줄씩 copy. 앞에 숫자를 붙이면 숫자만큼 copy.

`dd` : 잘라내기

`/` : 찾기. `n`을 누르면 그 다음 단어를 찾음.

`%s/찾을 단어/바꿀 단어` : 찾아서 바꾸기. g를 붙이면 전체 다 바꾸기(?)

% → 전체 행

1, 10 → 숫자로 지정하는 경우, 1에서 10행 사이

`set number` : 행 번호 보이기.

`set nonu` : 행 번호 숨기기.

`G` :  맨 마지막 행으로 이동.

`gg` : 첫 행으로 이동.

`q!` : 변경된 사항이 있으나 그냥 나감.

`w` : 저장.

`wq` : 저장하고 나가기

vi 환경설정 파일 .virc

vim 환경설정 파일 .vimrc