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
