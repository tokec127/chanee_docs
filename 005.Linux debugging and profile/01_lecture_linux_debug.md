
## 임베디드 리눅스 디버깅 및 프로파일링 (161031)

### 1. 개발환경 설정
* target system bap 설치 (Exynos4421)
* host system 설치
  * vmware 설치
  * linux vmware 준비
  * T32 설치


* 개발환경
  * NFS 이용 (/sysroot @ host가 target의 /)
  * target kernel 부팅이 완료된 이후 연결
  * NFS 설정방법
    - [HOST] : /sysroot 폴더 공유

```sh
 # chmod 777 /sysroot
 # echo "/sysroot *(rw,sync,no_root_squash,no_all_squash)" >> /etc/exports
 # service nfs restart
```
    - [Client] /sysroot@host를 /에 mount

```sh
 # showmount -e $HOST_IP
 # mount -t nfs $HOST_IP:/sysroot /
```

    - HOW-TO-CONFIG-NFS (http://blog.naver.com/ruddlekt/20091307949)
  * Kernel image patch using USB/Ethernet
    - smdk\_usb (binary command)
    - /tftpboot <-- 커널 이미지(zImage) 파일을 bootloadr에서 download



* 시작하기
  * Serial Configuration: 115200, N81 @ Target
  * bootloader configuration

```sh
baudrate=115200
bootargs=mem=500M root=/dev/nfs rw nfsroot=192.168.1.1:/sysroot ip=192.168.1.2:192.168.1.1:192.168.1.1:255.255.255.0::usb0:off hwaddr=00:12:34:56:78:90 console=ttySAC2,115200n81
bootcmd=dnw 40008000;bootm 40008000
bootcmd_extend=mmc read 1 40008000 7800 8; source 40008000
bootcmd_normal=movi read kernel 0 40008000;movi read rootfs 0 41000000 100000;bootm 40008000 41000000
bootdelay=1
ethbootargs=root=/dev/nfs rw nfsroot=192.168.106.103:/sysroot ip=192.168.106.33:192.168.106.103:192.168.106.1:255.255.255.0::eth0:off hwaddr=00:12:34:56:78:90 console=ttySAC2,115200n81
usbbootargs=root=/dev/nfs rw nfsroot=192.168.1.1:/sysroot ip=192.168.1.2:192.168.1.1:192.168.1.1:255.255.255.0::usb0:off hwaddr=00:12:34:56:78:90 console=ttySAC2,115200n81

Environment size: 777/16380 bytes
```
    * nfsroot=192.168.1.1:/sysroot   <-- host의 mount할 폴더
    * connection: eth @ host(192.168.1.1) - usb0 @ target
    * boot @ boot loader : boot
      - to download zImage, 
```sh
  # smdk-usbdl -f /tftpboot/zImage
```
  * Booting target 후 host에는 usb0에 192.168.1.1로 설정됨
  * Auto booting sequence
  - booting /tftpboot/zImage
  - mount /sysroot@host into /@target
  * How to auto booting
  - add `80-dnw.rules` to `/dev/udev/rules.d`
```sh
# cat 80-dnw.rules
SUBSYSTEMS=="usb", ATTRS{idVendor}=="04e8", ATTRS{idProduct}=="1234",
RUN+="/usr/bin/smdk-usbdl -a 0xc0008000 -f /tftpboot/zImage"
```
  - add `85-ifupdown.rules` to `/dev/udev/rules.d`
```sh
KERNEL=="usb0" RUN+="/etc/init.d/nfs-kernel-server restart"
SUBSYSTEM=="usb", ACTION=="add", DRIVERS=="?*", KERNEL=="usb0", NAME="eth0"
```





### 2. Attack using Buffer Overflow
* Concept
  * Stack 메모리를 침범하여 공격하는 행위 
* Example

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int checkpass() {
    int auth = 0;
    char passwd[9];

    printf("passwd at %p and\n auth at %p\n", (void*)passwd, (void*)&auth);

    printf("Enter a short word: ");
    scanf("%s", passwd);

    if(strcmp(passwd, "passnum") == 0)
        auth = 1;

    printf("auth(%d) \n", auth);
    return auth;
}

int main()
{
    int ret = checkpass();
    if(ret == 0)
        printf("You need right password to enter.\n");
    else
        printf("You are welcome to here !!\n");

    return 0;
}
```


### 3. Debugging and Memory
* 진단툴
  * strace: 참고(https://brunch.co.kr/@alden/12)
  * backtrace
  * gdb
  * trace32

* Memory Management
  * ARM architecture

```sh
    CPSR       SPSR_fiq, SPSR_svc, SPSR_abt, SPSR_irq, SPSR_und, SPSR_mon
  +---------+
  |   R1    |
  +---------+
  |   R2    |
  +---------+
  |   R3    |
  +---------+
  |   R4    |
  +---------+
  |   R5    |
  +---------+
  |   R6    |
  +---------+
  |   R7    |
  +---------+  +----------+
  |   R8    |  |  R8_fiq  |
  +---------+  +----------+
  |   R9    |  |  R9_fiq  |
  +---------+  +----------+
  |   R10   |  |  R10_fiq |
  +---------+  +----------+
  |   R11   |  |  R11_fiq |
  +---------+  +----------+
  |   R12   |  |  R12_fiq |
  +---------+  +----------+  
  | R13(SP) |  |  R13_fiq |  R13_svc, R13_abt, R13_irq, R13_und, R13_mon
  +---------+  +----------+ 
  | R14(LR) |  |  R14_fiq |  R14_svc, R14_abt, R14_irq, R14_und, R14_mon
  +---------+  +----------+
  | R15(PC) |               <Priviledged Mode>
  +---------+
<Non-privileged mode>
```

* Register bank
  - 응용이 동작할 때 사용하는 register들과 메모리를 구분하여 시스템에 영향을 주지 않도록 설계 또는 권한을 제어함
  - Mode가 변경되는 순간
  Reset, IRQ, SWI, SVC 등의 이벤트(exception)가 발생하면 mode가 변경됨
  - Sequence
  cp CPSR -> SPSR_<mode>
  - Exception address table

```
  0x0000    : Reset -> supervisor
  0x0004    : Undefined Ins
  0x0008    : Software interrupt (SWI, SVC)
  0x000c    : prefetch Abort/Break
  0x0010    : Data abort
  0x0014    : Reserved
  0x0018    : IRQ
  0x001c    : FIQ
```

  - 모드에 따라 MMU 동작을 달리한다.
  * Page talbe: Virtual address <--> Physical address
  * Page table은 physical memory 메모리 어딘가에 있다? 이 정보는 TTBA register
  * Page 크기: 4KB
  * 접근권한을 세분화할 수 있다
  * 영역별로 Cache 설정을 다르게 할 수 있다.

  - MMU 설정정보
  * L1: 
    - 4GB을 1MB단위로 base address, permission을 저장 (section)
    - 4KB 단위로 page를 관리할 경우, 32bit로 표현하지 못해 L2까지 합한 64bit로 표현
  * L2: physical address를 저장
        - Large page 단위: 64KB
        - Small page 단위: 4KB
  * 리눅스의 경우 2개의 TTBA가 존재 (user mode, kernel mode)

  - MMU @ Linux (32-bit)
```
  +--------------+  0x0000 0000
  |              |
  |  User Space  |   (3GB)    : 1 to 1 mapping
  |              |
  +--------------+  0xc000 0000
  |              |
  |  Kernel Sp   |  (1GB)    : 1 to N mapping
  |              |
  +--------------+  0xffff ffff
```

  * 4KB paging - include/linux/sched.h
```sh
  +--------------+
  |  Kernel      |
  |   space      |
  +--------------+
  | 환경변수문자열|
  +--------------+
  | 명령행 인자  |
  +--------------+
  |동적링크 테이블|
  +--------------+
  |    envp[]    |
  +--------------+
  |    argv[]    |
  +--------------+
  |    argc      |
  +--------------+  start_stack
  |    Stack     |  RLIMIT\_STACK (e.g. 8MB)
  +--------------+
  |    BSS       |
  +--------------+
  |    Data      |  Memory Mapping Segment
  +--------------+  (shared library, libxxx.so)
  |    Text      |
  +--------------+
  |    Heap      |  heap영역은 위로 확장 (0x40000000 @ 2.4, 동적 @ 2.6)
  +--------------+  
  |    BSS       |
  +--------------+
  |    Data      |
  +--------------+
  |    Text      | 실행파일 영역
  +--------------+ start_code (2.6에서는 동적, 2.4 0x08048000)
```

  * Kernel space에서 User permission은 noaccess
   * 자원이 부족한 상황시 응용 프로그램의 경우 SIGKILL을 통해 OS의 Process관리에서 제거해야 할 대상을 제거할 수 있다.


* Proc 파일시스템
  * Performance and Memory
  * Process의 argument
  * Hardware/Resource 정보
  * Example : maptest.c

```c
#include <stdio.h>

int global_var = 3;
int global_bssvar;
static int static_var1 = 5;

int main(void)
{
    int stack_var1 = 1;
    int stack_var2 = 2;
    char command[64];
    static int static_var2 = 6;
    char *heap_var = NULL;

    heap_var = (char*)malloc(1024);

    printf("Address of global_var is %p\n", &global_var);
    printf("Address of global_bssvar is %p\n", &global_bssvar);

    printf("Address of static_var1 is %p\n", &static_var1);
    printf("Address of static_var2 is %p\n", &static_var2);
    printf("Address of heap_var is %p\n", heap_var);
    printf("Address of stack_var1 is %p\n", &stack_var1);
    printf("Address of stack_var2 is %p\n", &stack_var2);

    sprintf(command, "cat /proc/%d/maps", getpid() );

    system(command);

    free(heap_var);

    return 0;
}
```

    - result

```sh
Address of global_var is 0x1103c
Address of global_bssvar is 0x1104c
Address of static_var1 is 0x11040
Address of static_var2 is 0x11044
Address of heap_var is 0x12008
Address of stack_var1 is 0xbef95c90
Address of stack_var2 is 0xbef95c8c
00008000-00009000 r-xp 00000000 00:0d 3289064    /work/t
00010000-00011000 r--p 00000000 00:0d 3289064    /work/t
00011000-00012000 rw-p 00001000 00:0d 3289064    /work/t
00012000-00033000 rw-p 00000000 00:00 0          [heap]
40041000-40042000 rw-p 00000000 00:00 0
40047000-40048000 rw-p 00000000 00:00 0
4006a000-40086000 r-xp 00000000 00:0d 3408198    /lib/ld-2.9.so
4008d000-4008e000 r--p 0001b000 00:0d 3408198    /lib/ld-2.9.so
4008e000-4008f000 rw-p 0001c000 00:0d 3408198    /lib/ld-2.9.so
400a0000-400a1000 rw-p 00000000 00:00 0
401ce000-402e9000 r-xp 00000000 00:0d 3408195    /lib/libc-2.9.so
402e9000-402f0000 ---p 0011b000 00:0d 3408195    /lib/libc-2.9.so
402f0000-402f2000 r--p 0011a000 00:0d 3408195    /lib/libc-2.9.so
402f2000-402f3000 rw-p 0011c000 00:0d 3408195    /lib/libc-2.9.so
402f3000-402f6000 rw-p 00000000 00:00 0
bef75000-bef96000 rw-p 00000000 00:00 0          [stack]
ffff0000-ffff1000 r-xp 00000000 00:00 0          [vectors]
```


### 4. Using GDB

	* 디버깅 환경
		- Host에는 소스, 디버깅 정보
		- Target에는 binary

```sh
	+--------+-------------------------------------------------------+
	|  cmd   | Description
	+--------+-------------------------------------------------------+
	| r      | start program
	| c      | continue until the next breakpoint
	| s      | perform step-into
	| n      | perform next step
	| u      | exit this loop
	| finish | exit this function
	| return | return this function at this point
	+--------+-------------------------------------------------------+
```

	* Cross-compile 환경에서의 디버깅
		1. gdb @ target if shell is enable @ target (e.g., arm-gdb)
		2. Target에는 gdb-server가 동작하고, Host에는 gdb-client가 동작

	* GDB-server and client 사용하기
		1. run gdb-server	@ target
```sh
	# gdbserver $GDB_SERVER_IP:$GDB_SERVER_PORT program
```

		2. run gdb-client @ host
```sh
	# arm-insignal-linux-gnueabi-gdb /sysroot/work/endian_test
	# set sysroot /sysroot					// not mandatory
  # (gdb) target remote $GDB_SERVER_IP:$GDB_SERVER_PORT
  # (gdb) c		// gdb server-client 환경에서는 run명령이 아닌 continue로 시작
```




