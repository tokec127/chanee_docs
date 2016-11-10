
## �Ӻ���� ������ ����� �� �������ϸ� (161101)

### 1. KDBG
* printk

```
	+--------------+------------------------------------------------------
	|  �α� ����   | ����
	+--------------+------------------------------------------------------
	| KERN_EMERG   | ����Ȳ, �ý����� �ߴܵ� �� ����
	| KERN_ALERT   | ���ǰ� �ʿ��� ��Ȳ
	| KERN_CRIT    | ġ���� ����
	| KERN_ERR     | ����
	| KERN_WARNING | ���
	| KERN_NOTICE  | �Ϲ����� �޽���
	| KERN_INFO    | ���������� ���� �޽���
	| KERN_DEBUG   | ����� �޽���
	+--------------+------------------------------------------------------
```

* KGDB : Kernel GDB
	- a source level debugger for the linux kernel
	- http://kgdb.sourceforge.net
	- �ֿ� �������̽�: Serial or Ethernet
	- Ŀ�� ��� ����� ����, Ŀ�ο� attach�ǰ� ������ϰ� detach

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

* KGDB boot arg �߰�
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
	* sysfs�� �̿��� �ǽð� Configure kgdboc
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
		* �޸� ������ �⺻ ����
		* MMU�� ������ ���̺��� ����
		* struct page�� ǥ��

	* Zone
```
	+--------------+----------+--------------------------------------------+
	| ZONE_DMA     |   0-16MB | Ư�� ��ġ(ISA/PCI)�� �ʿ�� �ϴ� �� ����   |
	|              |          | ������ �޸� ������ �����ϴ� �޸� ����  |
	+--------------+----------+--------------------------------------------+
	| ZONE_NORMAL  | 16-896MB | ������ �޸��� ���� ������ mapping.       |
	|              |          | ������ ���� Ŀ�� ��κ��� �� �������� ���� |
	+--------------+----------+--------------------------------------------+
	| ZONE_HIGHMEM |  > 896MB | Ŀ�ο� ���� ���ε��� ���� �ý��ۿ� �޸�  |
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
	* zImage ���� @ DRAM copied by boot loader
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
	* Built-in DD ���� ��κ� install table�� ���
	* install table - refer to include/linux/init.h





### 4. Signal
* Signal
	* ���μ������� event�� �߻��� �˸��� ���� ���޵Ǵ� SW interrupt
	* event causing signal
		* hardware exception (divided by zero)
		* software condition (alarm, expire)
		* exception key input (^c, ^z)
		* system call (e.g. kill)

* Signal
	* SIGSEGV ��ȣ�� �߻��ϸ� OS�� �ش� app�� ���� coredump ������ ����
	* ����ϱ� ���� ��������
		* -g �ɼ����� ������
		* ���� �ʿ� (root)
		* coredump size�� ���� (default 0)

```sh
	# ulimit -c			// <-- ���� core dump ũ�� Ȯ��
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
	| SIGALARM | 14 | �˶��� �߻��Ѵ�.
	| SIGSTP   | 19 | stop a process
	| SIGCONT  | 18 | continue a stopped process
	+----------+----+------------------------------------------------


* GDB�� core �м��ϱ�
	* coredump������ core �����̸����� ������
	* gdb�� Ȯ��: ` # gdb $PROGRAM_NAME core`
	* core������ ���� ���α׷��� Ȯ���� ��.

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
	open() -> swi 0x05 -> exception �߻�

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

	* "dev/mydrv"��� ����̽��� ����� ��,
		* file_struct fd @ fd table: 0, 1,2, ... -> get file
		* struct file *p



* Interrupt
	* system ���� �߻��� (0~31)
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
			* ���ͷ�Ʈ ��ó��/��ó���� ������ ó����
			* ��ó���� ���� ������, ��ó���� context ��ȯ �� ó��
			* ��ó���� enqueue, ��ó���� dequeue + handle

* Interface for bottom half
	* softirq
		* ������ ������ ������ ���ѵǾ� ���������� ������� ����
	* tasklet - not used in general
	* workqueue
		* �Ϲ������� ����ϴ� Queue
		* 2.4���� ���Ǵ� task queue�� ��ü
		* work queue ����ü���̿��� ��ϵ� �Ϲ��� keventd�� ���� Ŀ�� ������� ����
		* Ŀ�� ������ ������ ���μ��� ������ ���
		* ��ϵ� �Լ��� ����� �� ���ͷ�Ʈ ��� ����


		
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
