
## 임베디드 리눅스 디버깅 및 프로파일링 (161101)

### 1. KDBG
* printk

```
	+--------------+------------------------------------------------------
	|  로그 수준   | 설명
	+--------------+------------------------------------------------------
	| KERN_EMERG   | 비상상황, 시스템이 중단될 수 있음
	| KERN_ALERT   | 주의가 필요한 상황
	| KERN_CRIT    | 치명적 오류
	| KERN_ERR     | 오류
	| KERN_WARNING | 경고
	| KERN_NOTICE  | 일반적인 메시지
	| KERN_INFO    | 정보제공을 위한 메시지
	| KERN_DEBUG   | 디버그 메시지
	+--------------+------------------------------------------------------
```

* KGDB : Kernel GDB
	- a source level debugger for the linux kernel
	- http://kgdb.sourceforge.net
	- 주요 인터페이스: Serial or Ethernet
	- 커널 모듈 디버깅 지원, 커널에 attach되고 디버깅하고 detach

* CONFIG\_KGDB
	- make menuconfig as followings

```sh
	# make menuconfig
		-> enter kernel hacking
			 -> [o] KGDB: kernel debugger
					-> <*) KGDB: use kgdb over the serial console (NEW)
	# CONFIG_DEBUG_RODATA is not set
	CONFIG_FRAME_POINTER=y
	CONFIG_KGDB=y
	CONFIG_KGDB_SERIAL_CONSOLE=y
```

* KGDB boot arg 추가
	- add `kgdbwait kgdboc=ttySAC2,115200`

```sh
set bootargs mem=500M root=/dev/nfs rw nfsroot=192.168.1.1:/sysroot ip=192.168.1.2:192.168.1.1:192.168.1.1:255.255.255.0::usb0:off hwaddr=00:12:34:56:78:90 console=ttySAC1,115200n81 kgdbwait kgdboc=ttySAC2,115200 console=ttySAC2,115200n81
save
```

* KERNEL parameter: kgdboc
	- built-in driver
		* kernel boot argument: kgdboc=<tty-device>,[baud]
	- module driver
		* the command: modprobe kgdboc kgdboc=<tty-device>,[baud]
	- example

```sh
	kgdboc=ttyS0,115200
	kgdboc=ttyAMA1,115200
```

* KERNEL parameter: kgdboc @ TARGET
	* sysfs를 이용한 실시간 Configure kgdboc
	* Enable/Disable kgdboc on ttyS0 @ target

```sh
	# echo ttyS0 > /sys/module/kgdboc/parameters/kgdboc		<-- enable
	# echo "" > /sys/module/kgdboc/parameters/kgdboc			<-- disable
```

	* loading symbol @ host

```sh
	# arm-insignal-linux-gnueabi-gdb vmlinux
	(gdb) set remotebaud 115200
	(gdb) set debug remote 1
	(gdb) target remote /dev/ttyUSB0
```



### 2. Memory Management
* Memory Management
	* Physical Page
		* 메모리 관리의 기본 단위
		* MMU는 페이지 테이블을 관리
		* struct page로 표현

	* Zone
```
	+--------------+----------+--------------------------------------------+
	| ZONE_DMA     |   0-16MB | 특정 장치(ISA/PCI)가 필요로 하는 더 작은   |
	|              |          | 물리적 메모리 영역에 상주하는 메모리 범위  |
	+--------------+----------+--------------------------------------------+
	| ZONE_NORMAL  | 16-896MB | 물리적 메모리의 상위 영역에 mapping.       |
	|              |          | 성능이 좋고 커널 대부분이 이 영역에서 동작 |
	+--------------+----------+--------------------------------------------+
	| ZONE_HIGHMEM |  > 896MB | 커널에 의해 매핑되지 않은 시스템용 메모리  |
	+--------------+----------+--------------------------------------------+
```

* APIs for Memory Usage
	* APIs

```c
	struct page * alloc_pages(unsigned int gfp_mask, unsigned int order);
	void * page_address(struct page * page);
	unsigned long __get_free_pages(unsigned int gfp_mask, unsigned int order);
	struct page * alloc_page(unsigned int gfp_mask);
	unsigned long __get_free_page(unsigned int gfp_mask);

	// to get page with all zerorized data
	get_zeroed_page(gfp_mask);

	// free pages to be allocated
	void __free_pages(struct page *page, unsigned int order);
	void free_pages(unsigned long addr, unsigned int order);
	void freE_page(unsigned long addr);
```

	* Example 1

```c
	unsigned long page;

	page = __get_free_pages (GFP_KERNEL, 3);
	if (!page) {
		return ENOMEM;
	}

	free_pages(page, 3);
```


	* Example 2

```c
	#include <linux/slab.h>
	void *kmalloc (size_t size, int flags);
	void kfree (const void *ptr);
```

	* Flags


### 3. Linux Booting Sequence
* Linux Image
	* vmlinux
		* ELF object

* Booting Sequence
	* booter -> Bootloader -> zImage (Compressed Kernel)
	* booter
		* copy Bootloader(u-boot.bin) to memory and execute
	* Bootloader (uboot)
		* copy zImage to memory and execute
	* zImage (Linux Kernel)
		* uncompress zImage to another memory space and execute


* Kernel Image
	* zImage 구조 @ DRAM copied by boot loader
		`[ relocation_code || piggy ]`

		* piggy : compress(vmlinux)
		* relocation_code : head.s misc.s
			- uncompress piggy @ start_address		<<-- physical memory map
			- goto start_kernel
				- enable MMU												<<-- virtual memory map

```c
 start_kernel
		rest_init
			kernel_thread(kernel_init, NULL, CLONE_FS | CLONE_SIGHAND);
				kernel_init_freeable
					smp_init
						smp_cpus_done
					do_basic_setup
						do_initcalls
					ramdisk_execute_command="/init"
					...
					{android}/kernel/main.c
```


* RamDISK
```c
	
```


* Built-in Drivers
	* Built-in DD 들은 대부분 install table에 등록
	* install table - refer to include/linux/init.h





### 4. Signal
* Signal
	* 프로세스에게 event의 발생을 알리기 위해 전달되는 SW interrupt
	* event causing signal
		* hardware exception (divided by zero)
		* software condition (alarm, expire)
		* exception key input (^c, ^z)
		* system call (e.g. kill)

* Signal
	* SIGSEGV 신호가 발생하면 OS는 해당 app에 대한 coredump 파일을 생성
	* 사용하기 위한 선행조건
		* -g 옵션으로 컴파일
		* 권한 필요 (root)
		* coredump size를 설정 (default 0)

```sh
	# ulimit -c			// <-- 현재 core dump 크기 확인
	0
	# ulimit -c 10000
```

	+----------+----+------------------------------------------------
	|  type    | no | Description
	+----------+----+------------------------------------------------
	| SIGINT   |  2 | interrupt a process (block a process)
	| SIGABRT  |  6 | a process is aborted
	| SIGKILL  |  9 | kill a process
	| SIGSEGV  | 11 | a process tries to access invalid address
	| SIGALARM | 14 | 알람을 발생한다.
	| SIGSTP   | 19 | stop a process
	| SIGCONT  | 18 | continue a stopped process
	+----------+----+------------------------------------------------


* GDB로 core 분석하기
	* coredump파일은 core 파일이름으로 생성됨
	* gdb로 확인: ` # gdb $PROGRAM_NAME core`
	* core파일의 원인 프로그램을 확인할 때.

```sh
	# file core
  core: ELF 32-bit LSB core file ARM, version 1 (SYSV), SVR4-style, from './gtest'
```



### 5. Integrated Trace-32 Support Package (iTSP)


### 6. Device Driver and Library
* Device Driver
	* Software to control physical hardware device
	* ex.) Binder @ Android - DD to provide IPC @ Android
	* kernel space: 0xc0000000 ~ 0xffffffff
	* type: character/block/network device
	* all device drivers act as a file at user space, 
		ex.) open/read/write/ioctl/close

	* open 	- sys\_open
	* close - sys\_close
	* read 	- sys\_read
	* write - sys\_write

```
	+---------------+
	|  Application  |  user mode
	+---------------+	----------------------------------
	|      OS       |  kernel mode
	+---------------+
	| Device Driver | 
	+---------------+
	|   Hardware    |
	+---------------+
```	

* Network Device Driver

* Device Driver
	* all except for sound DD are located in drivers/
	* built-in driver and module driver


* Flow of opne

```c
	main()
	{
		open("/dev/mydrv");
	}

	// libc.a
	open() -> swi 0x05 -> exception 발생

	// 
	SPSR_svc = CPSR
	LR_svc = SWI ASM + 4
	CPSR[0:4] = 10011 	-> svc mode
	PC = 0x8


	// vector table @ 0x08
	0x00: Reset
	0x04: undef
	0x08: SWI			-> jump to swi process function

	//
	LDR	r10	{lr, #4}
	BIC	r10, r10, #ffffffff
	..
	ldreq pc, [tbl, scno, lsl #2]


	//
	sys_open()
	f->f_op->open = dev_open

	//
	dev_open()
	{
		...
	}
```

	* "dev/mydrv"라는 디바이스를 등록할 때,
		* file_struct fd @ fd table: 0, 1,2, ... -> get file
		* struct file *p



* Interrupt
	* system 오류 발생시 (0~31)
	* hardware interrupt (key input) (32~47)
	* system call (128)


* Interrupt API

```c
#include <linux/sched.h>
int request_irq();
void free_irq(unsigned int irq);
...
```

* Pre-requirement for interrupt handler
	* interrupt handler
		* should QUIT as soon as possible
		* very time-critical task
		* bottom half method
			* 인터럽트 전처리/후처리로 나누어 처리함
			* 전처리는 빨리 끝내고, 후처리는 context 전환 후 처리
			* 전처리는 enqueue, 후처리는 dequeue + handle

* Interface for bottom half
	* softirq
		* 빠르긴 하지만 개수가 제한되어 보편적으로 사용하지 않음
	* tasklet - not used in general
	* workqueue
		* 일반적으로 사용하는 Queue
		* 2.4에서 사용되던 task queue를 대체
		* work queue 구조체를이용해 등록된 하뭇의 keventd와 같은 커널 쓰레드로 수행
		* 커널 선점이 가능한 프로세스 스케쥴 방식
		* 등록된 함수가 수행될 때 인터럽트 허용 가능


		
* Example of Device Driver
	* misc\_register() to make a device node(/dev/xxx) automatically

```c
#include <linux/miscdevice.h>		// for misc_register(), misc_deregister()
static const struct file_operations mydev_fops = { 
  .owner          = THIS_MODULE,
  .open           = mydev_open, 
  .release        = mydev_release, 
  .unlocked_ioctl = mydev_ioctl,
  .read           = mydev_read, 
  .write          = mydev_write,
};

struct miscdevice mydev_miscdev = {
    MISC_DYNAMIC_MINOR, "mydev", &mydev_fops
};

int __init mydev_init(void)
{ 
  int ret;
      
  ret = misc_register(&mydev_miscdev);
  if( ret < 0 )
  {
    printk(KERN_ERR "mydev_miscdev driver register error\n");
    return ret;
  }

  // do something here

  printk( KERN_ALERT "\n%s module initialized.\n", DEVICE_NAME); 

  return 0; 
} 

void __exit mydev_exit(void) 
{
  misc_deregister(&mydev_miscdev);

  // do something here

  printk( KERN_ALERT "\n%s module removed.\n", DEVICE_NAME ); 
}

module_init(mydev_init); 
module_exit(mydev_exit); 
```
