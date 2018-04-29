RISC-V - How To Guide
=====================

This repository is intented as a brief 'HowToGuide' in order to get in touch with the RISC-V ecosystem and start play with the resources provided.

## Proposal and Audience

This repository is targeted to students (just like I am) who are interested in the RISC-V project and would like to get their hands on it. It has been structued in a way that could allow you to reach the following milestones:

+ Set up a working environment in order to build all the required tools and resources
+ Be familiar with the Chisel HDL
+ Be familiar with the RISC-V Instruction Set Architecture
+ Get some experience in writing Chisel code
+ Build a RISC-V processor
+ Run bare-metal software on your RISC-V processor
+ Run the Linux Kernel on a RISC-V ISA simulator

Thanks to these steps I believe that you could actually have a complete overview of the platform and be able to further dig into it or contribute yourself.

## Table of Contents

+ [Install instructions](#install)
+ [Resource in this repository](#src)
+ [How should I use this repository?](#how)
    + [The Chisel HDL](#how_chisel)
    + [The RISC-V ISA](#how_riscv)
    + [The Rocket core](#how_rocket)
+ [Run software](#spike)
+ [And then?](#then)
+ [Acknowledgment](#ack)

## <a name="general"></a> General Reccomendation

This repository has been tested on an Ubuntu-like OS, so please be sure to have a supported OS on your machine. In particular this repository has been tested on a machine running this version of Ubuntu:

	$ uname -a
	Linux localhost 4.13.0-38-generic #43-Ubuntu SMP Wed Mar 14 15:20:44 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
	$ lsb_release -a
	No LSB modules are available.
	Distributor ID:	Ubuntu
	Description:	Ubuntu 17.10	
	Release:	17.10
	Codename:	artful


We will make heavy use of the [GNU Make](https://www.gnu.org/software/make/) tool, thus I suggest you to configure it in order to exploit all the hardware resources of your machine. Set the number of recipes (jobs) to run simultaneously matching the number of cores in your machine:

	$ export MAKEFLAGS="-j $(grep -c ^processor /proc/cpuinfo)"

## <a name="install"></a> Install Instructions

### Install Necessary Dependencies

You may need to install some additional packages to use this repository.

1. Developer packages

	 ```
	$ sudo apt-get install autoconf automake autotools-dev curl device-tree-compiler libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev git make autoconf g++ flex bison
	```
	
2.	Debugging packages

	 ```
	$ sudo apt-get install pkg-config verilator gtkwave
	 ```
	
3. Java

   ```
   $ sudo apt-get install default-jdk
   ```
   
4. [Install sbt](http://www.scala-sbt.org/release/docs/Installing-sbt-on-Linux.html),
    which isn't available by default in the system package manager:
    
    ```
    $ echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
    $ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 642AC823
    $ sudo apt-get update
    $ sudo apt-get install sbt
    ```
    
If you encounter in any issues while building the resources provided in this repository, please refer to the appropriate section of the `README.md` files for each of the submodules:

+ [rocket-chip "Quick Instructions"](https://github.com/freechipsproject/rocket-chip#quick)
+ [chisel3 "Installation"](https://github.com/ucb-bar/chisel3#installation)
+ [riscv-tools "Quick Start"](https://github.com/riscv/riscv-tools#quickstart)


### Checkout The Code

    $ git clone https://github.com/amerlo94/riscv-howto.git
    $ cd riscv-howto
    $ git submodule update --recursive --init

### <a name="setting"></a> Setting up the RISCV environment variable

To build the rocket-chip repository, you must point the `$RISCV` environment variable to your riscv-tools installation directory. You could find the installation folder in the rocket-chip submodules, at rocket-chip/riscv-tools/riscv-gnu-toolchain.

    $ export RISCV=/'path-to-riscv-tools'/riscv-gnu-toolchain

The riscv-tools repository is already included in rocket-chip as a Git submodule. You **must** build this version of riscv-tools (you could do also later):

    $ cd riscv-howto/rocket-chip/riscv-tools
    $ git submodule update --init --recursive
    $ export RISCV=/'path-to-riscv-tools'/riscv-gnu-toolchain
    $ ./build.sh

For more information (or if you run into any issues), please consult the
[riscv-tools/README](https://github.com/riscv/riscv-tools/blob/master/README.md).

### Keeping Your Repo Up-to-Date

If you are trying to keep your repo up to date with this GitHub repo, you could issue the following commands:

    $ git pull origin master
    $ git submodule update --init --recursive

## <a name="src"></a> Resources in this repository

This 'How to Guide' to the RISC-V ecosystem wants to be a meta-repository where you could make your first steps and contribution to the several projects described. Therefore you could find several [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) related to the RISC-V resources and folder containing literature and specs PDF files. Regarding the git submodules I tried to set them up in a way that you would be able to properly run everything with the commits provided, thus please be sure to understand the resources before updating the submodules on your own. I will try to keep this repository alive, regularly bumping up the git submodules included.

### <a name="src_submodules"></a>Git Submodules

[Git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) allow you to keep a Git repository as a subdirectory of another Git repository. Doing this way I am able to track them in a controlled way, allowing for rapid exploitation of new features while keeping commit histories separate. Here the list of the submodules that are currently being tracked:

* **riscv-isa-manual** [https://github.com/riscv/riscv-isa-manual](https://github.com/riscv/riscv-isa-manual): Draft RISC-V Instruction Set Manual. You could find compiled ones both in the repository or at [https://riscv.org/specifications/](https://riscv.org/specifications/). Please scroll trough the commit history of this repository in order to have a glimpse of the future release for the ISA.

* **riscv-asm-manual** [https://github.com/riscv/riscv-asm-manual](https://github.com/riscv/riscv-asm-manual): RISC-V Assembly Programmer's Manual.

* **rocket-chip** [https://github.com/freechipsproject/rocket-chip](https://github.com/freechipsproject/rocket-chip): The Rocket Chip Generator.

* **chisel3** [https://github.com/ucb-bar/chisel3](https://github.com/ucb-bar/chisel3):
The Rocket Chip Generator uses [Chisel](http://chisel.eecs.berkeley.edu) to generate RTL.

* **learning-journey** [https://github.com/apaj/learning-journey](https://github.com/apaj/learning-journey): The Chisel HDL learning journey.

* **riscv-sodor** [https://github.com/ucb-bar/riscv-sodor](https://github.com/ucb-bar/riscv-sodor): Demonstration repository in order to evaluate RISC-V core implementations written in Chisel.

* **riscv-linux** [https://github.com/riscv/riscv-linux](https://github.com/riscv/riscv-linux): The Linux Kernel repository.
 
### <a name="src_folder"></a>Literature and Specs Folders

* **src/literature/chisel**: Folder containing literature on the Chisel HDL.
* **src/literature/rocket**: Folder containing literature on the Rocket Core.
* **src/specs**: Folder containing specs on the RISC-V ISA.

## <a name="how"></a> How should I use this repository?

This repository is aimed at delivering a preliminary approach to the RISC-V ecosystem, thus here you could find a set of sequential steps that has been defined in order to properly understand all the resources provided. At each step, if you find yourself quite familiar with the topics that you are facing, feel free to skip to the next one and keep learning.

### <a name="how_chisel"></a> The Chisel HDL

Most of the resources that you could find in this repository are described in Chisel, an 'Hardware Construction Language' developed at UC Berkeley in the Berkeley Architecture Research (UC-BAR) center. It has been widely leveraged in the [riscv-sodor](https://github.com/ucb-bar/riscv-sodor) and [rocket-chip](https://github.com/freechipsproject/rocket-chip) design. One of the main points of Chisel is the raised abstraction level that we use in order to describe the hardware, adopting the Scala functional language.

You could have an overview of the language by reading the original paper describing the language: **Hardware construction in Chisel** by *Huy Vo*. You could find the document in the `src/literature/chisel` folder. A more in depth description on how to use the language can be found in the **Chisel 2.2 Tutorial** by *Jonathan Bachrach at al.* (Chisel 3 took the place of Chisel 2.2 as the latest revision).

If you feel that the language is still very different to traditional HDL, such as VHDL or Verilog, I strongly suggest you to take a subset of the examples that you could find in the [learning-journey](https://github.com/apaj/learning-journey) submodules.

	$ cd learning-journey
	$ sudo ./set-learning-journey

Then follow the instructions provided.

If you want to dive into processor architecture written in the Chisel HDL, have a look at the [riscv-sodor](https://github.com/ucb-bar/riscv-sodor) submodules. It is not strongly recommended now for the purposes of this guide, but it is a valid educational resource for RISC-V integer pipelines design.

### <a name="how_riscv"></a> The RISC-V ISA

An introduction reading on the purposes of the newly proposed ISA could be found in the `src/literature/riscv` folder. **Design of the RISC-V Instruction Set Architecture**, written by *Andrew Waterman*, covers a review of the other popular ISA and presents the reason behind the RISC-V ISA.

Regarding its specifications you could find the RISC-V User and Privilege Specs in the `src/specs` folder. 

In the `src/literature/riscv` a set of RISC-V lectures are included, it is highly recommended to go trough them.

### <a name="how_rocket"></a> The Rocket core

The Rocket Chip generator, included in this repository as a submodule, could generate an in-order core, called the **Rocket**, and an out-of-order one, called the **BOOM**. The paper describing the generator could be found in the `src/literature/rocket` folder, where a set of other papers provide some project tape out based on the Rocket core.

The Rocket core is entirely written in Chisel, but Verilog could be generated in order to use the Rocket core in a traditional RTL design flow. Its design includes a set of specific IPs, such as the Debug Module or the TileLink bus, for which you could find the draft specifications in the `src/specs` folder.

Firstly we initialize all the rocket-chip components and set up the environmnet.

	$ cd rocket-chip
	$ git submodule update --init --recursive
	$ export RISCV=/'path-to-riscv-tools'/riscv-gnu-toolchain
	
If you did not build the RISC-V toolchain in the [Install Instructions](#install) chapter, please build the toolchain:

	$ cd rocket-chip/riscv-tools
    $ git submodule update --init --recursive
    $ export RISCV=/'path-to-riscv-tools'/riscv-gnu-toolchain
    $ ./build.sh
    
Now we have all the components in place and the tools builted. Please add the compiled binary to your `$PATH` variable:

	$ export PATH=/'path-to-riscv-tools'/riscv-gnu-toolchain/bin:$PATH
	
A description of the code tree of the Rocket core could be found in the Rocket core `README.md` file in the rocket-chip folder. The repository gives us the option to built a C++ cycle-accurate emulator or a Verilog file that could be used in a traditional RTL design flow.

In order to built the C++ emulator and run bare-metal software issue the following commands:

	$ cd rocket-chip/emulator
	$ make run
	$ make run-asm-tests
	$ make run-bsm-tests
	
In `rocket-chip/riscv-tests` you could find several bare-metal code that you could run in your C++ emulator.

Verilog files to include in a traditional RTL tool could be issued with the following command:

	$ cd rocket-chip/vsim
	$ make verilog
	
The main output Verilog file is located at `rocket-chip/vsim/generated-src`. There are three main files that have been generated that include most of your design:

	$ ls rocket-chip/vsim/generated-src
	DefaultConfig.dts
	freechips.rocketchip.system.DefaultConfig.v
	freechips.rocketchip.system.DefaultConfig.behav_srams.v
	...
	
The `.dts` file will be used by Linux in order to understand on what is running on, since it is a structured description of the hardware generated. The two Verilog files are the full design that you have instantiated, in particular a simple behavioral model of the memories has been included in a separate file in order to be replaced with the memory blocks provided for your technology.
    
If you encountered any issue in the above steps, please refer to the `README.md` file in the submodule.

At this point you are able to design and compile a default configuration for the Rocket core. Now I highly recommend you to play with the Scala files and redo the design flow.

* Add a new class in the rocket-chip/src/main/scala/system/Configs.scala
* Add a new class as a configuration class (extendings the Config one) which include your newly created class
* Compile the C++ emulator based on the new design (overwriting the `DefaultConfig` one) and run the complete suite of assembly and benchmark test on it
* Issue the compilation of the Verilog files in the vsim folder

## <a name="spike"></a> Run software

Up to now we have been playing with the hardware provided by the RISC-V community and learn how to modify it in order to test our ideas. Lately we run bare-metal software on the C++ emulator and tested if our changes did not brake the system. The RISC-V ecosystem is recently growning very fast also on the software side, with the inclusion of the Linux Kernel and QEMU in their upstream repository. This means that we are able to boot the Linux Kernel on a RISC-V core! Unfortunately we are not be able to emulate the Linux Kernel on our deisgn with the provided C++ emulator, since this would be too slow, but we could simulate with the official [RISC-V ISA Simulator](https://github.com/riscv/riscv-isa-sim).

Before digging into the actual process, I think that these two blog posts worth a reading:

* [Boot a RISC-V Linux Kernel](https://www.sifive.com/blog/2017/10/09/all-aboard-part-6-booting-a-risc-v-linux-kernel/)
* [Entering and Exiting the Linux Kernel on RISC-V](https://www.sifive.com/blog/2017/10/23/all-aboard-part-7-entering-and-exiting-the-linux-kernel-on-risc-v/)

If you interested in getting more information and detailed news on the RISC-V ecosystem I would strongly recommend you to read all the blog posts included in the blog page of SiFive.

Detailed steps in order to boot the Linux Kernel on Spike could be found in the [README.md](https://github.com/riscv/riscv-tools/tree/master#-the-linuxrisc-v-installation-manual) of the risc-tools submodule in the rocket-chip one. I suggest you to follow that steps using the submodules commits that you could find in the rocket-chip submodules included in this repository. Please refere to the following points during the building process:

+ We have already build the ISA simulator (if you followed the instructions inlcluded [here](#setting)), so you do not need to build it again. However we have to rebuild the toolchain since we need the `riscv64-unknown-linux-gnu-gcc` cross compiler instead of the `riscv64-unknown-elf-gnu-gcc` one. These could be issue with the following commands:

	```
	$ cd rocket-chip/riscv-tools/riscv-gnu-toolchain
	$ ./configure --prefix=$RISCV
	$ make linux
	```
	Now we should have the linux compiler binaries in the bin folder:
	
	```
	$ ll rocket-chip/riscv-tools/riscv-gnu-toolchain/bin
	```
	
	Remember to have the bin folder in your `$PATH` environment variable.
+ When it asks you to obtain the [Linux Kernel](https://github.com/riscv/riscv-linux) please use the one included in this repository, since it should work with the other submodules included. Build it with the following commands:

	```
	$ cd riscv-linux
	$ make ARCH=riscv defconfig
	$ make ARCH=riscv CROSS_COMPILE=riscv64-unknown-elf-gnu- vmlinux
	```
	You should have the compiled Linux image in the current folder.
	
+ When it asks you to obtain 'Busybox', please issue the following commands:

	```
	$ cp -rf src/busybox-1.28.3.tar.bz2 riscv-linux/
	$ cd riscv-linux
	$ tar xvjf busybox-1.28.3.tar.bz2
	$ cd busybox-1.28.3
	$ make allnoconfig
	```
	We used the latest tested version of the busybox binaries. Now follow the configuration steps provided in the `README.md` and compile the busybox toolbox.
	
If no issues occurred in the building process, remember to build the Linux Kernel with the `initramfs/initrd.conf` included and the Berkeley Boot Loader again. Now you should have a bbl binary with the Linux Kernel included as a payload, and the Linux Kernel should call the Busybox toolbox as part of the 'init' process. Now in order to boot the kernel just lunch spike:

	$ spike bbl vmlinux
	
Have a look at the whole list of commands that you have with spike with:

	$ spike -h
	
## <a name="then"></a> And then?

At this point you should have an initial understanding of most of the RISC-V resources and how to use them, so feel free to learn and contribute more. Here I would like to provide a list of other useful resources and actions in order to be more involved in the development of the project.

* [Subscribe to the official RISC-V mailing lists](https://riscv.org/mailing-lists/)
* [RISC-V Software Status](https://github.com/riscv/riscv-wiki/wiki/RISC-V-Software-Status)
* [RISC-V Core and SoCs](https://github.com/riscv/riscv-wiki/wiki/RISC-V-Cores-and-SoCs)
* [SiFive Blog Page](https://www.sifive.com/blog/)
* [RISC-V Workshop Proceedings](https://riscv.org/category/workshops/proceedings/)
* [SiFive GitHub Page](https://github.com/sifive)
	

## <a name="ack"></a> Acknowledgment

This repository makes heavy use of open-source resources made available by a plethora of actors in the RISC-V ecosystem, which I would like to thank for their efforts into the RISC-V project.

+ [UC Berkeley Architecture Research](https://bar.eecs.berkeley.edu/index.html)
+ [RISC-V Foundation](https://riscv.org/)
+ [LibreCores](https://www.librecores.org/)
+ [Christopher Celio](https://people.eecs.berkeley.edu/~celio/)