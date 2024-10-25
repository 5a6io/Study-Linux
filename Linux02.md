# Linux02

## apt update 중 Hash Sum Mismatch 오류 발생시

```bash
sudo rm -rf /var/lib/apt/lits/*
sudo apt update -o Acquire::CompressionTypes::Order::=gz
sudo apt update
```

## 기본 명령어

1. `less`, `more` : 긴 문서 파일 보기
less 파일명
less는 뒤로 가기 가능
v키를 입력하면 편집 가능.
2. `cat` : 짧은 문서파일 보기, 짧은 문서 작성
cat 파일명 : 문서 보기
cat > 파일명 : 화면에 입력한 내용을 파일에 저장. `ctrl + d` 를 입력해서 저장하고 종료.
cat >> 파일명 : 기존 파일 내용에 추가.

```bash
[root@rocky ~]# cat > test
ls
spring
summer
autumn
winter
[root@rocky ~]# cat test
ls
spring
summer
autumn
winter
[root@rocky ~]# cat >> test
melon
[root@rocky ~]# cat test
ls
spring
summer
autumn
winter
melon

```

1. 표준파일 입력(0)•출력(1), 표준 에러(2), Redirection(표준 입출력, 에러의 방향을 바꿈)
`0` 과 `1`은 생략 가능. `2`는 생략 불가.

```bash
표준 입출력
[root@rocky ~]# cat > test2 < test -> test1에 있는 내용을 test2에 입력.
[root@rocky ~]# cat test2
ls
spring
summer
autumn
winter
melon
```

```bash
[admin@rocky tmp]$ touch test
[admin@rocky tmp]$ date
2024. 07. 01. (월) 22:48:05 EDT
[admin@rocky tmp]$ date > test
[admin@rocky tmp]$ cat test
2024. 07. 01. (월) 22:48:19 EDT
```

```bash
표준 에러
[root@rocky ~]# a 2> error.txt -> a에 대한 오류 내용을 error.txt에 입력.  
[root@rocky ~]# cat error.txt
-bash: a: command not found
[root@rocky ~]# a 2> /dev/null
```

1. 소유권(Ownership), 허가권(Permission)
- 소유권(Ownership): 파일이나 디렉토리의 소유자, 그룹을 표시, 소유자도 그룹도 아닌 사용자 그외(Other)
                 해당 사용자가 파일에 접근하고자 할 때 소유권에 따라 접근 권한이 달라짐.
- 소유권을 확인하는 명령 : ls -l
   -rw-r--r--. 1 root root   37 Jul  1 20:55 test2
                소유자 그룹
- 소유자(user),  그룹(group), 그외(other) 별로 권한을 가짐.
- 허가권(Permission): 파일이나 디렉토리에 대한 권한(Read, Write, eXecute)
  파일 : r(읽기), w(쓰기), x(실행)
  디렉토리: r(목록 읽기), w(디렉토리에 파일 쓰기), x(cd로 접근하기)

  허가권은 user, group, other별로 부여됨.
   rwx   rwx   rwx Permission을 숫자로 표시.  7     7     7
  user  group other                       user  gruop other
  ex) 644 = rw- r— r— : user는 읽기, 쓰기 가능. group과 other는 읽기 가능.
- 소유권 변경: `chown new_user.new_group 파일명`
             `chgrp new_group 파일명` → 그룹만 변경.
- 허가권 변경:`chmod 숫자방식 or 문자방식 파일명`
            숫자 방식은 모든 사용자별로 모두 작성 : 755, 640, 400
            문자 방식은 변경되는 사용자만 작성: u+x, o-w, ug+x, a=rw
            +, -는 기존에서 추가하고 빼는 것.
            = 는 기존에 상관없이 권한 변경.
- 사용자를 특정 그룹에 포함시킬 경우
  usermod -ag 그룹명 사용자 주그룹 변경.
  usermod -aG 그룹명 사용자 부그룹 변경.
- 특수퍼미션: setuid, setgid, stickybit
  setuid, setgid: 실행파일을 실행할 때 실행파일의 소유자 권한이 아닌 실행하는 사용자의 권한으로 실행이 되는 것이 일반적인 상황. 소유자, 혹은 소유그룹의 권한으로 실행하는 것이 필요한 경우 setuid, setgid 퍼미션을 부여해야 함.
  stickybit: 디렉토리에 부여하는 특수퍼미션으로 해당 퍼미션이 부여된 디렉토리에는 모든 사용자가 쓰기 가능하지만 삭제는 소유자만 가능.
- 4755, 2644, 1755 → 맨 앞에 붙는 것이 특수 퍼미션.

### usermod -aG docker hyemi:WSL

```bash
id 확인
hyemi@KHP18:~$ id
uid=1000(hyemi) gid=1000(hyemi) groups=1000(hyemi),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),116(netdev)
hyemi@KHP18:~$ sudo usermod -aG docker hyemi
hyemi@KHP18:~$ id
uid=1000(hyemi) gid=1000(hyemi) groups=1000(hyemi),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),116(netdev),999(docker)
```

### usermod -aG docker admin:Rocky

```bash
[admin@rocky ~]$ id
uid=1000(admin) gid=1000(admin) groups=1000(admin),10(wheel) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
[admin@rocky ~]$ sudo usermod -aG docker admin -> docker그룹에 admin을추가함으로써 sudo 명령어 없이 사용이 가능해짐.
재로그인
[admin@rocky ~]$ id
uid=1000(admin) gid=1000(admin) groups=1000(admin),10(wheel),992(docker) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```

### setuid, setgid

```bash
[admin@rocky ~]$ ls -l /usr/bin/passwd
-rwsr-xr-x. 1 root root 33424  4월 19  2022 /usr/bin/passwd
   ↳setuid 특수퍼미션이 있음.
   
[admin@rocky ~]$ cp /usr/bin/vi .
[admin@rocky ~]$ ls
vi
[admin@rocky ~]$ ls -l /usr/bin/vi
-rwxr-xr-x. 1 root root 1180368  8월  2  2022 /usr/bin/vi
[admin@rocky ~]$ ls -l
합계 1156
-rwxr-xr-x. 1 admin admin 1180368  7월  1 22:40 vi
[admin@rocky ~]$ sudo chown root.root vi
[sudo] admin의 암호:
[admin@rocky ~]$ ls -l
합계 1156
-rwxr-xr-x. 1 root root 1180368  7월  1 22:40 vi
[admin@rocky ~]$ ./ vi /etc/shadow
-bash: ./: Is a directory
[admin@rocky ~]$ ./vi /etc/shadow
[admin@rocky ~]$ sudo chmod 4755 vi
[admin@rocky ~]$ ls -l
합계 1156
-rwsr-xr-x. 1 root root 1180368  7월  1 22:40 vi
[admin@rocky ~]$ ./vi /etc/shadow
[admin@rocky ~]$ ls -l /tmp
합계 8
-rwx------. 1 root root 291  7월  1 07:43 ks-script-3ijqdtsg
-rwx------. 1 root root 701  7월  1 07:43 ks-script-9cpfsmek
drwx------. 2 root root   6  7월  1 07:45 mkdumprd.wt1cKt
drwx------. 3 root root  17  7월  1 20:06 systemd-private-770f28a9c85a4ac3aad6d23c439e1f09-chronyd.service-UH5ZJd
[admin@rocky ~]$ ls -ld /tmp → -ld 옵션을 이용하여 /tmp 자체의 권한을 확인할 수 있음.
drwxrwxrwt. 9 root root 4096  7월  1 22:16 /tmp  → stickybit가 걸려 있음.
[admin@rocky ~]$ ls -l
합계 1156
-rwsr-xr-x. 1 root root 1180368  7월  1 22:40 vi
[admin@rocky ~]$ sudo chmod 4644 vi
[admin@rocky ~]$ ls -l
합계 1156
-rwSr--r--. 1 root root 1180368  7월  1 22:40 vi → 실행권한이 없는데 권한을 부여하여 'S'로 나타남.
[admin@rocky tmp]$ ls -l test
-rw-rw-r--. 1 admin admin 39  7월  1 22:49 test
[admin@rocky tmp]$ chmod o+w test
[admin@rocky tmp]$ ls -l test
-rw-rw-rw-. 1 admin admin 39  7월  1 22:49 test
[admin@rocky tmp]$ su - user1
암호:
[user1@rocky ~]$ cd /tmp
[user1@rocky tmp]$ ls -l test
-rw-rw-rw-. 1 admin admin 39  7월  1 22:49 test
[user1@rocky tmp]$ rm test
rm: cannot remove 'test': Operation not permitted → 실행 권한이 없어 삭제할 수는 없음.
[user1@rocky tmp]$ date >> test → 쓰기 권한이 있어 쓰기는 가능.
```

1. 프로세스와 서비스
프로세스: 프로그래밍 실행되어 메모리에서 동작하고 있는 상태
서비스: 데몬이라고도 하며 백그라운드에서 동작하면서 요청이 있을 경우 응답하는 프로세스
ps: 현재 터미널에서 동작시킨 프로세스
`ps -ef` : 현재 시스템에서 동작인 모든 프로세스
`ps aux` :               “               . -ef와는 나타내
         는 정보가 다름.
`kill -l` : 모든 시그널 출력
          SIGHUP(1) : 프로세스를 일시중지했다가 다시 실행. 
                      설정 파일을 다시 읽어들임.
                      프로세스 ID가 안 바뀜.
          SIGINT(2) : ctrl + c에 의해 발생.
          SIGKILL(9) : 강제 종료
          SIGTERM(15): 기본 시그널. 정상 종료
          SIGSTP(20) : 일시정지
grep: 패턴 검색.
서비스(데몬) : 서비스를 제어하는 명령 - systemctl, service
             systemctl 명령 서비스명.service or .socket
                                    ↳.service 생략 가능
                      start : 서비스 시작
                      stop : 서비스 종료
                      restart : 서비스 재시작
                      status : 서비스 상태 확인
                      enable : 부팅시 서비스 시작
                      disable : 부팅시 서비스 시작 안 함
             ex) systemctl start docker
                 = service docker start
                 systemctl start telnet.socket
                 = service telnet.socket start

---

Rocky Linux는 SELINUX가 활성화 돼 있음.
비활성화하고 사용할 경우 설정
# vi /etc/sysconfig/selinux
SELINUX=enforcing ⇒ disabled로 변경하고 저장 후 리부팅. 영구적으로 적용.
# setenforce 0 ⇒ 일시적으로 해제.

### ps -ef/ps aux

```bash
[admin@rocky tmp]$ ps
    PID TTY          TIME CMD
   4809 pts/0    00:00:00 bash
   4922 pts/0    00:00:00 bash
   5074 pts/0    00:00:00 ps
[admin@rocky tmp]$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 20:06 ?        00:00:11 /usr/lib/systemd/systemd --s
root           2       0  0 20:06 ?        00:00:00 [kthreadd]
root           3       2  0 20:06 ?        00:00:00 [rcu_gp]
root           4       2  0 20:06 ?        00:00:00 [rcu_par_gp]
root           5       2  0 20:06 ?        00:00:00 [slub_flushwq]
root           7       2  0 20:06 ?        00:00:00 [kworker/0:0H-events_highpri
root          10       2  0 20:06 ?        00:00:00 [mm_percpu_wq]
root          11       2  0 20:06 ?        00:00:00 [rcu_tasks_rude_]
root          12       2  0 20:06 ?        00:00:00 [rcu_tasks_trace]
p]
root         157       2  0 20:06 ?        00:00:00 [irq/53-pciehp]
root         158       2  0 20:06 ?        00:00:00 [irq/54-pciehp]
                                .
                                .
                                .
admin       4922    4918  0 22:21 pts/0    00:00:00 -bash
root        4955       2  0 22:32 ?        00:00:00 [kworker/0:1-xfs-sync/nvme0n
root        5018       2  0 22:50 ?        00:00:00 [kworker/0:0-xfs-sync/nvme0n
root        5057       2  0 22:56 ?        00:00:01 [kworker/1:3-events]
root        5059       2  0 23:00 ?        00:00:00 [kworker/0:2-events]
root        5073       2  0 23:03 ?        00:00:00 [kworker/1:0]
admin       5075    4922  0 23:04 pts/0    00:00:00 ps -ef
[admin@rocky tmp]$ ps -ef | head → 맨 위 10개
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 20:06 ?        00:00:11 /usr/lib/systemd/systemd --switched-root --system --deserialize 17
root           2       0  0 20:06 ?        00:00:00 [kthreadd]
root           3       2  0 20:06 ?        00:00:00 [rcu_gp]
root           4       2  0 20:06 ?        00:00:00 [rcu_par_gp]
root           5       2  0 20:06 ?        00:00:00 [slub_flushwq]
root           7       2  0 20:06 ?        00:00:00 [kworker/0:0H-events_highpri]
root          10       2  0 20:06 ?        00:00:00 [mm_percpu_wq]
root          11       2  0 20:06 ?        00:00:00 [rcu_tasks_rude_]
root          12       2  0 20:06 ?        00:00:00 [rcu_tasks_trace]
[admin@rocky tmp]$ ps -ef | tail → 맨  10개
root        4846    4809  0 22:17 pts/0    00:00:00 sudo docker pull nginx
root        4852    4846  0 22:18 pts/0    00:00:00 docker pull nginx
root        4918    4809  0 22:21 pts/0    00:00:00 su - admin
admin       4922    4918  0 22:21 pts/0    00:00:00 -bash
root        4955       2  0 22:32 ?        00:00:00 [kworker/0:1-xfs-sync/nvme0n1p1]
root        5057       2  0 22:56 ?        00:00:01 [kworker/1:3-events]
root        5059       2  0 23:00 ?        00:00:00 [kworker/0:2-events]
root        5073       2  0 23:03 ?        00:00:00 [kworker/1:0]
admin       5079    4922  0 23:05 pts/0    00:00:00 ps -ef
admin       5080    4922  0 23:05 pts/0    00:00:00 tail
[admin@rocky tmp]$ ps aux | head → aux를 이용하면 언제 시작했는지 알 수 있음.
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.1  0.4 177816 16068 ?        Ss   20:06   0:11 /usr/lib/systemd/systemd --switched-root --system --deserialize 17
root           2  0.0  0.0      0     0 ?        S    20:06   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   20:06   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   20:06   0:00 [rcu_par_gp]
root           5  0.0  0.0      0     0 ?        I<   20:06   0:00 [slub_flushwq]
root           7  0.0  0.0      0     0 ?        I<   20:06   0:00 [kworker/0:0H-events_highpri]
root          10  0.0  0.0      0     0 ?        I<   20:06   0:00 [mm_percpu_wq]
root          11  0.0  0.0      0     0 ?        S    20:06   0:00 [rcu_tasks_rude_]
root          12  0.0  0.0      0     0 ?        S    20:06   0:00 [rcu_tasks_trace]
[admin@rocky tmp]$ ps aux | tail
root        4852  0.0  0.7 1780696 27340 pts/0   Tl   22:18   0:00 docker pull nginx
root        4918  0.0  0.1 316904  6120 pts/0    S    22:21   0:00 su - admin
admin       4922  0.0  0.1 224992  3928 pts/0    S    22:21   0:00 -bash
root        4955  0.0  0.0      0     0 ?        I    22:32   0:00 [kworker/0:1-xfs-sync/nvme0n1p1]
root        5057  0.2  0.0      0     0 ?        I    22:56   0:01 [kworker/1:3-events]
root        5059  0.0  0.0      0     0 ?        I    23:00   0:00 [kworker/0:2-events]
root        5073  0.0  0.0      0     0 ?        I    23:03   0:00 [kworker/1:0]
root        5081  0.0  0.0      0     0 ?        I    23:08   0:00 [kworker/0:0-events]
admin       5085  0.0  0.1 257508  3876 pts/0    R+   23:09   0:00 ps aux
admin       5086  0.0  0.0 217252   872 pts/0    S+   23:09   0:00 tail
```

### kill/sleep

```bash
[admin@rocky tmp]$ kill -l
 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
[admin@rocky tmp]$ sleep 1000
^Z
[1]+  Stopped                 sleep 1000
[admin@rocky tmp]$ ^C
[admin@rocky tmp]$ ps
    PID TTY          TIME CMD
   4809 pts/0    00:00:00 bash
   4922 pts/0    00:00:00 bash
   5087 pts/0    00:00:00 sleep
   5088 pts/0    00:00:00 ps
[admin@rocky tmp]$ sleep 2000 &
[2] 5094
[admin@rocky tmp]$ ps
    PID TTY          TIME CMD
   4809 pts/0    00:00:00 bash
   4922 pts/0    00:00:00 bash
   5087 pts/0    00:00:00 sleep
   5094 pts/0    00:00:00 sleep
   5095 pts/0    00:00:00 ps
[admin@rocky tmp]$ jobs
[1]+  Stopped                 sleep 1000
[2]-  Running                 sleep 2000 &
[admin@rocky tmp]$ fg %1 → foreground로 실행
sleep 1000
^Z
[1]+  Stopped                 sleep 1000
[admin@rocky tmp]$ bg %1 → background로 실행
[1]+ sleep 1000 &
[admin@rocky tmp]$ jobs
[1]-  Running                 sleep 1000 &
[2]+  Running                 sleep 2000 &
[admin@rocky tmp]$ kill %1
[1]-  Terminated              sleep 1000
[admin@rocky tmp]$ kill -9 %2
[admin@rocky tmp]$ 
[2]+  Killed                  sleep 2000
[admin@rocky ~]$ sudo vi /etc/ssh/sshd_config → passwordauthentication을 yes에서 no로 바꿨음.
[admin@rocky ~]$ sudo kill -1 921 → 재시작하지 않고 SIGHUP으로 기존 프로세스를 그대로 쓰면 설정파일을 다시 불러옴.
```

![PasswordAuthentication을 no로 바꿔서 password로 ssh에 접근 불가.](Linux02%20de602754103c4dd28cd8ee5457e42d3f/Untitled.png)

PasswordAuthentication을 no로 바꿔서 password로 ssh에 접근 불가.

```bash
[root@rocky ~]# getenforce
Enforcing
[root@rocky ~]# setenforce 0
[root@rocky ~]# getenforce
Permissive
```

1. job control : 현재 터미널에서 동작 중인 작업 제어
jobs : 작업 목록 출력.
fg : 중단된 프로세스(작업)을 foreground에서 동작.
     ⇒ foreground에서 동작인 프로세스 중단시킬 때 `ctrl + z`
     `fg %작업번호` : 현재 멈춰있는 작업을 foreground로 동작
                   % 생략 가능
bg : 중단된 프로세스(작업)를 background에서 동작
     `bg %작업번호` : 현재 멈춰있는 작업을 background로 동작
`명령 &` : 해당 명령을 백그라운드에서 동작시킴.
`kill %작업번호` : 해당 작업 종료. %생략하면 PID가 됨.
2. 작업 자동화: `at`, `cron`
`at` : 특정 시점에 수행할 작업을 지정하고 해당 시점에서 자동으로 작업 수행
`cron` : 주기적으로 수행할 작업을 지정하고 매 시점에서 해당 작업 수행

at 날짜, 시간 지정
at 14:17 today
실행할 작업 1
실행할 작업 2
ctrl + d
* 서버 시간을 시간 서버와 동기화할 경우 : sudo systemctl 
  restart chronyd
* timezone 변경: timedatectl set-timezone Asiz/Seoul

`crontab -e` : 반복적으로 수행할 작업 등록(vi)
            `분 시 일 월 요일(0~6)수행할 명령or쉘스크립트 파일 등록`
            10  *  *  *  *
           */20 *  *  *  * → 20분마다
- at, cron에 대한 접근제어: at.deny, at.allow
                          cron.deny, cron.allow
  .deny: 해당 파일에 등록된 사용자는 at, cron 사용 불가
  .allow: 해당 파일에 등록된 사용자만 at, cron 사용 가능
  .deny, .allow 파일이 모두 있는 경우 .allow가 적용됨.
  내용이 비어 있는 경우 모두 허용.

![화요일마다 매시간 40분마다 수행.
분 시 일 월 요일](Linux02%20de602754103c4dd28cd8ee5457e42d3f/eda39f56-4590-43dc-964d-ff586f8d974f.png)

화요일마다 매시간 40분마다 수행.
분 시 일 월 요일

1. sudo : 일반 사용자가 root권한으로 명령을 수행할 필요가 있는 경우 /etc/sudoers파일에 사용자를 등록하고 사용할 수 있는 명령을 지정함.
su 명령으로 root로 사용자를 전환하고 명령을 수행할 수 있으나 특정 명령만 허용할 수 없음.
2. tar, 압축프로그램
- tar(tape archive) : 여러 파일들을 묶어서 하나의 파일로 만드는 유틸리티.
  옵션
  `-c` : 파일묶기
  `-x` : 묶인 파일 풀기
  `-v` : 묶거나 푸는 과정 출력
  `-t` : 실제로 풀거나 묶지 않고 파일 확인
  `-f` : 장치나 파일 지정
ex) tar cvf test.tar c* → 파일 묶기
    tar xvf test.tar → 파일 풀기
    tar tvf test.tar
    tar xvf test.tar -C /tmp : `-C` 옵션은 tar 해제 위치 지정.
- 압축프로그램 : gzip(gunzip), bzip2(bunzip2), xz, compress, zip(unzip)
  압축 시 : gzip 파일명
           bzip2 파일명
           xz 파일명
           compress 파일명
           zip 파일명.zip 압축할 파일
  압축해제 시 : gunzip 파일명, gzip -d 파일명
              bunzip2 파일명, bzip2 -d 파일명
              xz -d 파일명
              uncompress 파일명
              unzip 파일
- tar와 압축프로그램을 함께 사용하여 tar 묶고 압축하는 과정을 한 번에 처리
  tar xvfz 파일명.tar.gz ⇒ gzip으로 압축해제하고 tar 파일 풀기 동시에 진행.(z 생략가능)
  tar svfj 파일명.tar.bz2 ⇒ gzip으로 압축해제하고 tar파일 풀기 동시에 진행.
  tar xvfJ 파일명.tar.xz ⇒ xz으로 압축해제하고 tar 파일 풀기 동시에 진행.
  tar xvfZ 파일명.tar.Z ⇒ compress로 압축해제하고 tar 파일 풀기 동시에 진행.

```bash
[admin@rocky workdir]$ tar cvf cfile.tar c*
cache_check
cache_dump
cache_metadata_size
cache_repair
cache_restore
cache_writeback
capsh
cfdisk
chcpu
chgpasswd
chkconfig
chpasswd
chronyd
chroot
clock
clockdiff
consoletype
cracklib-check
cracklib-format
cracklib-packer
cracklib-unpacker
create-cracklib-dict
crond
ctrlaltdel
ctstat
[admin@rocky workdir]$ tar cf pfile.tar p*
[admin@rocky workdir]$ ls *tar
cfile.tar  pfile.tar
[admin@rocky workdir]$ ls *file*
cfile.tar  pfile.tar
[admin@rocky workdir]$ tar tvf pfile.tar
-rwxr-xr-x admin/admin   12328 2024-07-02 16:04 packer
-rwxr-xr-x admin/admin   46272 2024-07-02 16:04 pam_console_apply
-rwxr-xr-x admin/admin   12144 2024-07-02 16:04 pam_timestamp_check
-rwxr-xr-x admin/admin   87720 2024-07-02 16:04 parted
-rwxr-xr-x admin/admin   16752 2024-07-02 16:04 partprobe
-rwxr-xr-x admin/admin   96768 2024-07-02 16:04 partx
-rwxr-xr-x admin/admin 1500856 2024-07-02 16:04 pdata_tools
-rwxr-xr-x admin/admin   16960 2024-07-02 16:04 pidof
-rwxr-xr-x admin/admin   67664 2024-07-02 16:04 ping
-rwxr-xr-x admin/admin   67664 2024-07-02 16:04 ping6
-rwxr-xr-x admin/admin   12672 2024-07-02 16:04 pivot_root
-rwxr-xr-x admin/admin  223688 2024-07-02 16:04 poweroff
-r-xr-xr-x admin/admin 2735808 2024-07-02 16:04 pvchange
-r-xr-xr-x admin/admin 2735808 2024-07-02 16:04 pvck
-r-xr-xr-x admin/admin 2735808 2024-07-02 16:04 pvcreate
-r-xr-xr-x admin/admin 2735808 2024-07-02 16:04 pvdisplay
-r-xr-xr-x admin/admin 2735808 2024-07-02 16:04 pvmove
-r-xr-xr-x admin/admin 2735808 2024-07-02 16:04 pvremove
-r-xr-xr-x admin/admin 2735808 2024-07-02 16:04 pvresize
-r-xr-xr-x admin/admin 2735808 2024-07-02 16:04 pvs
-r-xr-xr-x admin/admin 2735808 2024-07-02 16:04 pvscan
-rwxr-xr-x admin/admin   58640 2024-07-02 16:04 pwck
-rwxr-xr-x admin/admin   54376 2024-07-02 16:04 pwconv
-rwxr-xr-x admin/admin   20920 2024-07-02 16:04 pwhistory_helper
-rwxr-xr-x admin/admin   54400 2024-07-02 16:04 pwunconv
[admin@rocky workdir]$ tar tf pfile.tar p*
packer
pam_console_apply
pam_timestamp_check
parted
partprobe
partx
pdata_tools
pidof
ping
ping6
pivot_root
poweroff
pvchange
pvck
pvcreate
pvdisplay
pvmove
pvremove
pvresize
pvs
pvscan
pwck
pwconv
pwhistory_helper
pwunconv
[admin@rocky workdir]$ tar xf cfile.tar -C ~/tardir
[admin@rocky workdir]$ ls
cache_check          cracklib-check        ping6
cache_dump           cracklib-format       pivot_root
cache_metadata_size  cracklib-packer       poweroff
cache_repair         cracklib-unpacker     pvchange
cache_restore        create-cracklib-dict  pvck
cache_writeback      crond                 pvcreate
capsh                ctrlaltdel            pvdisplay
cfdisk               ctstat                pvmove
cfile.tar            packer                pvremove
chcpu                pam_console_apply     pvresize
chgpasswd            pam_timestamp_check   pvs
chkconfig            parted                pvscan
chpasswd             partprobe             pwck
chronyd              partx                 pwconv
chroot               pdata_tools           pwhistory_helper
clock                pfile.tar             pwunconv
clockdiff            pidof
consoletype          ping
[admin@rocky workdir]$ ls ~/tardir
cache_check          cfdisk     clock              create-cracklib-dict
cache_dump           chcpu      clockdiff          crond
cache_metadata_size  chgpasswd  consoletype        ctrlaltdel
cache_repair         chkconfig  cracklib-check     ctstat
cache_restore        chpasswd   cracklib-format
cache_writeback      chronyd    cracklib-packer
capsh                chroot     cracklib-unpacker
[admin@rocky workdir]$ tar xf pfile.tar -C ~/tardir
[admin@rocky workdir]$ ls ~/tardir
cache_check          cracklib-check        ping6
cache_dump           cracklib-format       pivot_root
cache_metadata_size  cracklib-packer       poweroff
cache_repair         cracklib-unpacker     pvchange
cache_restore        create-cracklib-dict  pvck
cache_writeback      crond                 pvcreate
capsh                ctrlaltdel            pvdisplay
cfdisk               ctstat                pvmove
chcpu                packer                pvremove
chgpasswd            pam_console_apply     pvresize
chkconfig            pam_timestamp_check   pvs
chpasswd             parted                pvscan
chronyd              partprobe             pwck
chroot               partx                 pwconv
clock                pdata_tools           pwhistory_helper
clockdiff            pidof                 pwunconv
consoletype          ping
[admin@rocky workdir]$ ls /home
admin  user1  user2  user3  user4
[admin@rocky workdir]$ sudo tar cf user1.tar /home/user1
[sudo] admin의 암호:
tar: 구성 요소 이름 앞에 붙은 `/' 제거 중
[admin@rocky workdir]$ tar tvf user1.tar
drwx------ user1/user1       0 2024-07-02 12:04 home/user1/
-rw-r--r-- user1/user1      18 2024-02-10 21:29 home/user1/.bash_logout
-rw-r--r-- user1/user1     141 2024-02-10 21:29 home/user1/.bash_profile
-rw-r--r-- user1/user1     376 2024-02-10 21:29 home/user1/.bashrc
-rw------- user1/user1     186 2024-07-02 15:40 home/user1/.bash_history
[admin@rocky workdir]$ tar xf user1.tar -C ~/tardir
[admin@rocky workdir]$ ls ~/tardir
cache_check          cracklib-check        ping
cache_dump           cracklib-format       ping6
cache_metadata_size  cracklib-packer       pivot_root
cache_repair         cracklib-unpacker     poweroff
cache_restore        create-cracklib-dict  pvchange
cache_writeback      crond                 pvck
capsh                ctrlaltdel            pvcreate
cfdisk               ctstat                pvdisplay
chcpu                home                  pvmove
chgpasswd            packer                pvremove
chkconfig            pam_console_apply     pvresize
chpasswd             pam_timestamp_check   pvs
chronyd              parted                pvscan
chroot               partprobe             pwck
clock                partx                 pwconv
clockdiff            pdata_tools           pwhistory_helper
consoletype          pidof                 pwunconv
```

```bash
[admin@rocky workdir]$ ls -l cfile?.*
-rw-rw-r--. 1 admin admin 10055680  7월  2 16:20 cfile1.tar
-rw-rw-r--. 1 admin admin 10055680  7월  2 16:20 cfile2.tar
-rw-rw-r--. 1 admin admin 10055680  7월  2 16:20 cfile3.tar
-rw-rw-r--. 1 admin admin 10055680  7월  2 16:21 cfile4.tar
-rw-rw-r--. 1 admin admin 10055680  7월  2 16:21 cfile5.tar
[admin@rocky workdir]$ ls -lh cfile?.tar
-rw-rw-r--. 1 admin admin 9.6M  7월  2 16:20 cfile1.tar
-rw-rw-r--. 1 admin admin 9.6M  7월  2 16:20 cfile2.tar
-rw-rw-r--. 1 admin admin 9.6M  7월  2 16:20 cfile3.tar
-rw-rw-r--. 1 admin admin 9.6M  7월  2 16:21 cfile4.tar
-rw-rw-r--. 1 admin admin 9.6M  7월  2 16:21 cfile5.tar
[admin@rocky workdir]$ gzip cfile1.tar
[admin@rocky workdir]$ sudo dnf install bzip2
[admin@rocky workdir]$ bzip2 cfile2.tar
[admin@rocky workdir]$ xz cfile3.tar
[admin@rocky workdir]$ sudo dnf install compress
[admin@rocky workdir]$ compress cfile4.tar
[admin@rocky workdir]$ sudo dnf install zip
[admin@rocky workdir]$ ls -lh cfile?.*
-rw-rw-r--. 1 admin admin 3.9M  7월  2 16:20 cfile1.tar.gz
-rw-rw-r--. 1 admin admin 3.6M  7월  2 16:20 cfile2.tar.bz2
-rw-rw-r--. 1 admin admin 768K  7월  2 16:20 cfile3.tar.xz
-rw-rw-r--. 1 admin admin 5.6M  7월  2 16:21 cfile4.tar.Z
-rw-rw-r--. 1 admin admin 9.6M  7월  2 16:21 cfile5.tar
[admin@rocky workdir]$ rm -rf ../tardir/*
[admin@rocky workdir]$ ls ../tardir
[admin@rocky workdir]$ tar xvfz cfile1.tar.gz -C ~/tardir
cache_check
cache_dump
cache_metadata_size
cache_repair
cache_restore
cache_writeback
capsh
cfdisk
chcpu
chgpasswd
chkconfig
chpasswd
chronyd
chroot
clock
clockdiff
consoletype
cracklib-check
cracklib-format
cracklib-packer
cracklib-unpacker
create-cracklib-dict
crond
ctrlaltdel
ctstat
[admin@rocky workdir]$ mkdir ~/tardir/bzip2
[admin@rocky workdir]$ mkdir ~/tardir/xz
[admin@rocky workdir]$ mkdir ~/tardir/compress
[admin@rocky workdir]$ tar xfj cfile2.tar.bz2 -C ~/tardir/bzip2
[admin@rocky workdir]$ tar xvfJ cfile3.tar.xz -C ~/tardir/xz
cache_check
cache_dump
cache_metadata_size
cache_repair
cache_restore
cache_writeback
capsh
cfdisk
chcpu
chgpasswd
chkconfig
chpasswd
chronyd
chroot
clock
clockdiff
consoletype
cracklib-check
cracklib-format
cracklib-packer
cracklib-unpacker
create-cracklib-dict
crond
ctrlaltdel
ctstat
[admin@rocky workdir]$ ls cfile?.*
cfile1.tar.gz  cfile2.tar.bz2  cfile3.tar.xz  cfile4.tar.Z  cfile5.tar
[admin@rocky workdir]$ zip cfile5.tar.zip cfile5.tar
  adding: cfile5.tar (deflated 60%)
[admin@rocky workdir]$ ls cfile?.*
cfile1.tar.gz   cfile3.tar.xz  cfile5.tar
cfile2.tar.bz2  cfile4.tar.Z   cfile5.tar.zip
[admin@rocky workdir]$ ls -lh  cfile?.*
-rw-rw-r--. 1 admin admin 3.9M  7월  2 16:20 cfile1.tar.gz
-rw-rw-r--. 1 admin admin 3.6M  7월  2 16:20 cfile2.tar.bz2
-rw-rw-r--. 1 admin admin 768K  7월  2 16:20 cfile3.tar.xz
-rw-rw-r--. 1 admin admin 5.6M  7월  2 16:21 cfile4.tar.Z
-rw-rw-r--. 1 admin admin 9.6M  7월  2 16:21 cfile5.tar
-rw-rw-r--. 1 admin admin 3.9M  7월  2 16:58 cfile5.tar.zip
```

1. 파일시스템: 물리적인 하드디스크에 파일저장하기 위한 체계
리눅스 파일시스템 : ext2, ext3, ext4, xfs
파일시스템 확인: df -Th
리눅스에서는 모든 장치를 파일로 취급함. 그리고 모든 장치는 마운트가 되어야 사용할 수 있음.
                                                    ↳리눅스를 장치에 연결.
mount 지점에 lost+found가 생김.

시스템 종료 명령 : poweroff, halt, shutdown -h now, init 0
시스템 리부팅 명령 : reboot, shutdown -r now, init 6

```bash
[root@rocky ~]# ls /dev
autofs           kmsg          rtc       tty17  tty37  tty57    vcs3
block            log           rtc0      tty18  tty38  tty58    vcs4
bsg              loop-control  sda       tty19  tty39  tty59    vcs5
cdrom            mapper        sg0       tty2   tty4   tty6     vcs6
char             mcelog        sg1       tty20  tty40  tty60    vcsa
console          mem           shm       tty21  tty41  tty61    vcsa1
core             mqueue        snapshot  tty22  tty42  tty62    vcsa2
cpu              net           snd       tty23  tty43  tty63    vcsa3
cpu_dma_latency  ng0n1         sr0       tty24  tty44  tty7     vcsa4
disk             null          stderr    tty25  tty45  tty8     vcsa5
dm-0             nvme0         stdin     tty26  tty46  tty9     vcsa6
dm-1             nvme0n1       stdout    tty27  tty47  ttyS0    vfio
dri              nvme0n1p1     tty       tty28  tty48  ttyS1    vga_arbiter
fb0              nvme0n1p2     tty0      tty29  tty49  ttyS2    vhci
fd               nvram         tty1      tty3   tty5   ttyS3    vhost-net
full             port          tty10     tty30  tty50  uhid     vhost-vsock
fuse             ppp           tty11     tty31  tty51  uinput   vmci
hpet             ptmx          tty12     tty32  tty52  urandom  zero
hugepages        pts           tty13     tty33  tty53  usbmon0
hwrng            random        tty14     tty34  tty54  vcs
initctl          raw           tty15     tty35  tty55  vcs1
input            rl            tty16     tty36  tty56  vcs2
[root@rocky ~]# fdisk -l
Disk /dev/nvme0n1: 30 GiB, 32212254720 bytes, 62914560 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xa828558e

Device         Boot   Start      End  Sectors Size Id Type
/dev/nvme0n1p1 *       2048  2099199  2097152   1G 83 Linux
/dev/nvme0n1p2      2099200 62914559 60815360  29G 8e Linux LVM

Disk /dev/mapper/rl-root: 26 GiB, 27913093120 bytes, 54517760 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

Disk /dev/mapper/rl-swap: 3 GiB, 3221225472 bytes, 6291456 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

Disk /dev/sda: 10 GiB, 10737418240 bytes, 20971520 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
[root@rocky ~]# fdisk /dev/sda → 파티셔닝할 디스크.

Welcome to fdisk (util-linux 2.32.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x8f5952c6.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p):

Using default response p.
Partition number (1-4, default 1):
First sector (2048-20971519, default 2048):
Last sector, +sectors or +size{K,M,G,T,P} (2048-20971519, default 20971519):

Created a new partition 1 of type 'Linux' and of size 10 GiB.

Command (m for help): p
Disk /dev/sda: 10 GiB, 10737418240 bytes, 20971520 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x8f5952c6

Device     Boot Start      End  Sectors Size Id Type
/dev/sda1        2048 20971519 20969472  10G 83 Linux

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
[root@rocky ~]# mkfs -t ext4 /dev/sda1 → 어떤 방식으로 파일을 만들지 파일시스템 설정.
mke2fs 1.45.6 (20-Mar-2020)
Creating filesystem with 2621184 4k blocks and 655360 inodes
Filesystem UUID: aa4ad144-e526-42f7-9eb4-e3cc897072a3
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done
[root@rocky ~]# ls /mnt
[root@rocky ~]# mkdir /mnt/mpoint
[root@rocky ~]# cp /usr/sbin/x* /mnt/mpoint
[root@rocky ~]# ls /mnt/mpoint
xfs_admin     xfs_freeze  xfs_logprint   xfs_quota        xtables-nft-multi
xfs_bmap      xfs_fsr     xfs_mdrestore  xfs_repair
xfs_copy      xfs_growfs  xfs_metadump   xfs_rtcp
xfs_db        xfs_info    xfs_mkfile     xfs_spaceman
xfs_estimate  xfs_io      xfs_ncheck     xtables-monitor
[root@rocky ~]# mount -t ext4 /dev/sda1 /mnt/mpoint
[root@rocky ~]# ls /mnt/mpoint → mount 지점임을 알 수 있음. 기존에 있던 파일이 보이지 않게 됨. mount 지점에는 파일이 있으면 안됨.
lost+found
```

- vi 편집기 시 `ctrl + z`는 멈춰놓는 것.
- `ctrl + c`를 이용해서 interrupt를 발생시켜 종료.

- ssh서비스에 개인키를 이용해서 접속하려면 해당되는 공개키가 서버의 authorized_keys 파일에 추가돼 있어야 함.
- `#ssh-copy-id -i 개인키 사용자@서버IP` ⇒ 개인키의 쌍인 공개키가 서버에 복사가 됨.