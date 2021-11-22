# CMPE283 : Virtualization<br />
## Assignment 1: Discovering VMX Features<br />

• Read the VMX configuration MSRs to ascertain support capabilities/features
Entry / Exit / Procbased / Secondary Procbased / Pinbased controls<br />

• For each group of controls above, interpret and output the values read from the MSR to the system
via printk(..), including if the value can be set or cleared.<br />

### Answer 1

### Answer 2
### Steps to create a Linux kernel module that will query various MSRs to determine virtualization features available in your CPU<br />
<br />
System configurations<br />
Host OS : Windows 10 x64 based processor<br />
RAM : 8GB <br />
Processor : Intel(R) Core(TM) i5<br />

1) Install VMware Workstation 16 Player on host OS. (Install VM fusion if you are using Mac as Host OS)<br />
2) Install Ubuntu OS as Guest OS (Ubuntu 64-bit 20.04.3) <br />
3) After installation enable hardware assisted virtaulization as follows <br />
Go to VM Machine settings -> Processors -> Virtualization engine -> Enable the 'Virtualize Intel VT-x/EPT or AMD-V/RVI' checkbox.<br />
4) Install  git, build-essential, libssl-dev, flex, bison.<br />
5) Clone the repository https://github.com/archana-kamath/linux.git<br />
6) Update config file to get the configurations required the latested cloned version of linux.<br />
7) Install all the modules and kernel code.(This step will take time depending on the your system configuration)<br />
8) Reboot the VM check if the latest recently installed linux version is selected.<br />
9) Create a folder cmpe283 inside linux folder and copy Makefile and cmpe283-1.c provided.<br />
10) Update the cmpe283-1.c file to get all the VMX functionalities and build it. Add Modules license code too. <br/>
11) Run the command make and see if build is successful.<br />
12) Run command 'sudo insmod cmpe283-1.ko' to load the file to kernel.<br />
13) Run command 'dmesg' to get the output. Observe the if the values can be set or cleared for the 5 controls.<br />
14) Run command 'sudo rmmod cmpe283-1' to load the file out of the kernel.<br />
15) Commit and push the Makefile and cmpe283-1.c to git repository, after giving username and token.<br />

Screen prints of the output

Pinbased and Primary Proc Based controls
![alt text](https://github.com/archana-kamath/linux/blob/master/assignment_screenshots/pin_prim.JPG?raw=true)

Secondary Proc Based controls
![alt text](https://github.com/archana-kamath/linux/blob/master/assignment_screenshots/secondary.JPG?raw=true)

Entry and Exit Based controls
![alt text](https://github.com/archana-kamath/linux/blob/master/assignment_screenshots/entry_exit.JPG?raw=true)

## Assignment 2:

### Answer 1: 

Archana Miyar Kamath (015276378)<br />
Setting up Virtual machine<br />
Code changes in cpuid.c and vmx.c.<br />
Building the code changes. <br />
Debugging the build issues.<br />

Mounica Kamireddy (015949414)<br />
Understanding the usage of int, atomic variables and u32, u64 etc<br />
Installing nested machine and implemented test cases.<br />
Debugging issues while installing nested virtual machine.<br />


### Answer 2

### Part 1:<br />
For CPUID leaf node %eax=0x4FFFFFFF:
Return the total number of exits (all types) in %eax

### Part 2:<br />
For CPUID leaf node %eax=0x4FFFFFFE:
Return the high 32 bits of the total time spent processing all exits in %ebx
Return the low 32 bits of the total time spent processing all exits in %ecx
%ebx and %ecx return values are measured in processor cycles, across all VCPUs

Configuration done in Assignment 1 is required to proceed with assignment 2.

Do the code changes in cpuid.c and vmx.c 
path :
/linux/arch/x86/kvm/cpuid.c 
/linux/arch/x86/kvm/vmx/vmx.c

Run the below commands in order

sudo make modules
sudo make INSTALL_MOD_STRIP=1 modules_install 
sudo make install

lsmod | grep kvm
// These below two files should not be there, if so remove them 
sudo rmmod kvm_intel 
sudo rmmod kvm

lsmod | grep kvm (kvm_intel and kvm should not be in response)

sudo modprobe kvm
sudo modprobe kvm_intel

Reboot the VM

### Steps to test 

Set up an VM inside the host machine to test the changes.
Install the following - virtinst, libvirt-clients, virt-top, qemu-kvm, libvirt-daemon, libvirt-daemon-systems,  virt-manager, bridge-utils

Run the following command - sudo virt-manager
Download ubuntu-20.04.3-desktop-amd64.iso 
Start Virtual Machine Manager and install ubuntu. 
Once the inner VM is started run the below commands in the inner VM
sudo apt install cpuid

Run below to get output of part 1:<br />
cpuid -l 0x4FFFFFFF 

Run below to get output of part 2: <br />
cpuid -l 0x4FFFFFFE

![alt text](https://github.com/archana-kamath/linux/blob/master/assignment_screenshots/output2.JPG?raw=true)

## Assignment 3

### Answer 1: 

Archana Miyar Kamath (015276378)<br />
Setting up Virtual machine<br />
Code changes in cpuid.c and vmx.c.<br />
Building the code changes. <br />
Debugging the build issues.<br />

Mounica Kamireddy (015949414)<br />
Understanding the usage of int, atomic variables and u32, u64 etc<br />
Installing nested machine and implemented test cases.<br />
Debugging issues while installing nested virtual machine.<br />

### Answer 2

### Part 1: <br />
For CPUID leaf node %eax=0x4FFFFFFD:
Return the number of exits for the exit number provided (on input) in %ecx
This value should be returned in %eax 

### Part 2: <br />
For CPUID leaf node %eax=0x4FFFFFFC:
Return the time spent processing the exit number provided (on input) in %ecx
Return the high 32 bits of the total time spent for that exit in %ebx

Answer 2: 

Repeat the steps from assignment 2 to build the changes. As follows

Do the code changes in cpuid.c and vmx.c 
path :
/linux/arch/x86/kvm/cpuid.c 
/linux/arch/x86/kvm/vmx/vmx.c

Run the below commands in order

sudo make modules
sudo make INSTALL_MOD_STRIP=1 modules_install 
sudo make install

lsmod | grep kvm
// These below two files should not be there, if so remove them 
sudo rmmod kvm_intel 
sudo rmmod kvm

lsmod | grep kvm (kvm_intel and kvm should not be in response)

sudo modprobe kvm
sudo modprobe kvm_intel

Reboot the VM

### Test in inner VM

Run below to get output of part 1
cpuid -l 0x4FFFFFFD -s 0

Run below to get output of part 2
cpuid -l 0x4FFFFFFC -s 0

Execute dmesg in outer VM
Link to output - [Output](https://github.com/archana-kamath/linux/blob/master/output.txt)

![alt text](https://github.com/archana-kamath/linux/blob/master/assignment_screenshots/part3_4.png?raw=true)

### Answer 3 

Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there 
more exits performed during certain VM operations? Approximately how many exits does a full VM 
boot entail?

Frequency of some exits have increased, while some remain almost same. 
Exit 0 (Exception or non-maskable interrupt) does not show any notable difference (Changed from 8919 to 8945) but Exit 1(External interrupt) almost doubles (From 27140 to 56295). I opened firefox and searched for some terms in the inner VM to note the difference in number of exits. 46 (Access to GDTR or IDTR), 48 (EPT violation) 49 (EPT misconfiguration) have increased a lot, where as reason code 28 to 32 does not show any much difference. Most of the other exit code shows 0, that is exit did not occur at all.

The total number of exits on inner VM reboot,  as follows 

Before count
001f17e3 = 2037731 decimal 

After count
0030b376 - 3191670 decimal

Difference - 11,53,939

### Answer 4

Of the exit types defined in the SDM, which are the most frequent? Least?

Most frequent 
Exit : 0   Exception or non-maskable interrupt (NMI)<br />
Exit : 1   External interrupt<br />
Exit : 7   Interrupt window<br />
Exit : 10  CPUID <br />
Exit : 12  HLT<br />
Exit : 28  Control-register accesses<br />
Exit : 30  I/O instruction<br />
Exit : 31  RDMSR<br />
Exit : 32  WRMSR<br />
Exit : 48  EPT violation<br />
Exit : 49  EPT misconfiguration<br />

Least frequent<br />
Exit : 29   MOV DR<br />
Exit : 46   Access to GDTR or IDTR. <br />
Exit : 54   WBINVD or WBNOINVD<br />
Exit : 55   XSETBV<br />

Rest of the exits did not occur.<br />
