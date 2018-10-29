# Linux4.16.0 arm64 kernel build
### for debuging with KGDB
###### will add KDB later..

## What is this repository?
kgdb研 戚遂背 arm64 朕確聖 郊稽 巨獄焔 拝 呪 赤亀系 柵球 背兜精
煽舌社脊艦陥. //
_~~慎嬢研 公背辞 廃越稽 旋精 依精 焼鑑艦陥. ぞぞぞぞぞぞ ^^;;;;;~~_

* [Linux4.16.0 arm64 kernel build](https://github.com/TOT0RoKor/Linux4.16_arm64_debug_build)

### Debugging Environment
* **Host OS** : Ubuntu(18.04 LTS) on Windows (Download from Windows Store)
* **Target Kernel Version** : v4.16.0
* **GDB Version** : GNU gdb-multiarch 8.1.0
* **Compiler Version** : aarch64-linux-gnu-gcc 7.3.0
* **Qemu Version** : qemu-system-aarch64 2.11.1


## How to do it
~~_謝 降聖 Do it! 舘 却戚 却戚! 戚 鴻聖 Take it!_~~

### Install

#### GDB

###### Ubuntu
```sh
sudo apt-get install gdb-multiarch
```

#### QEMU

###### Ubuntu
```sh
sudo apt-get install qemu-system-aarch64
```


### Use

戚 Repository 巨刑斗軒拭辞 疑拙獣轍陥.

#### QEMU

###### Ubuntu
```sh
qemu-system-aarch64 -machine virt -cpu cortex-a57 -smp 2 -nographic \
                -m 4096M -s -S -kernel Image -append "kgdboc=ttyS0,115200"
```

* `$ qemu-system-aarch64` : Start qemu emulator.
* `-machine (-M) virt` : Use virt machine and virt machine's default dtb.
* `-cpu cortex-a57` : Use cortex-a57 cpu.
* `-smp 2` : Use 2 cores.
* `-nographic` : Do not use GUI qemu.
* `-m 4096M` : Use 4096MB memory. Default 128M.
* `-s` : Use gdbserver (localhost:1234, tcp)
* `-S` : Freeze the cpu.
* `-kernel Image` : Use the kernel image.
* `-append "kgdboc=ttyS0,115200"` : Use serial port of ttyS0 and Baud rate is 115200bps. 

> <http://studyfoss.egloos.com/5491211>

> <https://www.slideshare.net/aksmj/kgdbkdb>

> <https://withinrafael.com/2018/02/11/boot-arm64-builds-of-windows-10-in-qemu/>

> <http://jake.dothome.co.kr/qemu/>

#### GDB

###### Ubuntu

**Prompt= $**
```sh
gdb-multiarch vmlinux
```

**Prompt= (gdb)**
```sh
set architecture aarch64
set serial baud 115200
target remote :1234
```
* `(gdb) set architecture aarch64` : 巨獄焔 拝 焼徹努団研 aarch64稽 痕井. 奄沙 gdb拭澗 蒸澗 焼徹努団.
* `(gdb) set serial baud 115200` : baud rate研 115200bps稽 痕井.
* `(gdb) target remote :1234` : qemu稽 姥疑廃 os拭 羨紗. qemu拭辞 `-s` 辛芝拭 奄沙 匂闘澗 `:1234`

戚板澗 gdb 誤敬嬢研 戚遂背 戚遂 馬檎 吉陥. `-S` 辛芝生稽 昔背 Head.S拭辞 cpu亜 Freeze 雌殿.
`b(ranch) [func]` 稽 働舛 敗呪拭 breakpoint研 杏壱 `c(ontinue)`研 叔楳馬檎 背雁 敗呪猿走 姥疑廃陥.

> <https://sjp38.github.io/post/qemu_kernel_debugging/>

> <https://code.i-harness.com/ko-kr/q/14fc388>

## You can do
~~_格砧? 醤 蟹砧._~~

### How to get ready for compiling


**Install Dependent Packages**

###### Ubuntu
```sh
sudo apt-get install build-essentials libncurses5-dev libssl-dev bc bison flex \
                libelf-dev
```

###### Fedora
```sh
sudo dnf install ncurses-devel bison-devel bison flex-devel flex \
                elfutils-libelf-devel openssl-devel
```
> [https://sjp38.github.io/post/linux-kernel-build/](https://sjp38.github.io/post/linux-kernel-build/)

**Install Compiler Package**

###### Ubuntu
```sh
sudo apt-get install binutils-aarch64-linux-gnu gcc-aarch64-linux-gnu \
                g++-aarch64-linux-gnu
```

> [https://blog.thinkbee.kr/linux/crosscompile-arm/](https://blog.thinkbee.kr/linux/crosscompile-arm/)

### How to configure a Makefile

Modify a Makefile.

```sh
vi Makefile
```

1. Set Architecture and Cross compiler.

![config-defconfig](https://tot0ro-prog.firebaseapp.com/Image/Linux4.16_arm64_debug_build/makefile1.PNG)

2. Set Checkstack architecture.

![config-defconfig](https://tot0ro-prog.firebaseapp.com/Image/Linux4.16_arm64_debug_build/makefile2.PNG)



### How to make Image

```sh
make mrproper
make defconfig
make menuconfig
make Image
```

* `$ make mrproper` : config 督析 clean.
* `$ make defconfig` : 背雁 arch税 defconfig 督析稽 .config 竺舛. (./arch/ARCH/configs/defconfig 含櫛 馬蟹赤製)
* `$ make menuconfig` : 蓄亜 痕井拝 config 竺舛(kgdb 竺舛).
* `$ make Image` :  Image (朕確戚耕走) 柵球.  
        * Caution : -j 辛芝聖 爽走 省澗 依聖 映舌. (督析 税糎失 庚薦稽 陳督析 掻舘戚 切爽 忽嬢像)

> [http://xenostudy.tistory.com/485](http://xenostudy.tistory.com/485)

### How to update configuration file for debugging

* **defconfig**
![config-defconfig](https://tot0ro-prog.firebaseapp.com/Image/Linux4.16_arm64_debug_build/make_config.PNG)

#### make menuconfig

* **CONFIG\_DEBUG\_INFO=y**
![config-debuginfo](https://tot0ro-prog.firebaseapp.com/Image/Linux4.16_arm64_debug_build/debug_info.PNG)

* **CONFIG\_KGDB=y**
![config-kgdb](https://tot0ro-prog.firebaseapp.com/Image/Linux4.16_arm64_debug_build/kgdb.PNG)

* **CONFIG\_KGDB\_SERIAL\_CONSOLE=y**
![config-kgdb-serial-console](https://tot0ro-prog.firebaseapp.com/Image/Linux4.16_arm64_debug_build/kgdb_serial_console.PNG)

* **CONFIG\_FRAME\_POINTER=y**
![config-frame-pointer](https://tot0ro-prog.firebaseapp.com/Image/Linux4.16_arm64_debug_build/frame_pointer.PNG)

> [http://studyfoss.egloos.com/5490783](http://studyfoss.egloos.com/5490783)



