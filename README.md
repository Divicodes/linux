<h2> CMPE 283 <br>
Divija Naredla - 015945969 <br>
SPRING 2022 </h2>

All assignments are done by myself.

<h3>Assignment 1: </h3>

Used GCP for doing this assignment. <br>
Created VM instance using Google Cloud Platform <br>
Build and install new kernel version: <br>
Fork the Git repo torvalds/linux to my own git account <br>
git clone https://github.com/Divicodes/linux.git to clone it locally into linux and cd into the new directory <br>
uname -a : give the current running linux version <br>

sudo apt install gcc flex libssl-dev to install the necessary prerequisites for installing the kernel <br>
sudo apt install make <br>
make oldconfig to copy the current boot config into the new one (kept the enter key pressed as per the Professor’s suggestion to get default new values) <br>
make prepare <br>
scripts/config --set-str SYSTEM_TRUSTED_KEYS  <br>
scripts/config --disable SYSTEM_REVOCATION_KEYS // to disable the revocation keys <br>
sudo apt install dwarves //to fix the error <br>
make -j 8 modules to build all the new kernel modules <br>
make -j 8 to build the new kernel <br>
sudo make INSTALL_MOD_STRIP=1 modules_install //to install the modules <br>

Created CMPE-283 directory inside linux directory and copied the Makefile and cmpe283-1.c file provided by the Professor <br>
Appended the licence information at the end of cmpe283-1.c <br>
make to build the kernel <br>
sudo insmod cmpe283-1.ko to install the module <br>
sudo rmmod cmpe283-1 to remove the module <br>

After successfully executed all commands, made few changes to the cmpe283-1.c file and executed the above 3 commands again. <br>

Followed the steps guided in the assignment video and resolved the issues which occured with the help of stackoverflow and other websites <br>

Following is the obtained output : <br>

<img width="1436" alt="Screen Shot 2022-04-22 at 7 49 56 PM" src="https://user-images.githubusercontent.com/91343818/164872890-8ec33db4-9e1c-41b7-b4f4-66c333bcb05d.png">


<img width="1411" alt="Screen Shot 2022-04-22 at 7 53 07 PM" src="https://user-images.githubusercontent.com/91343818/164872806-da5c7f64-5df2-4aa3-8f21-2cdf283413ad.png">

<h3>Assignment 2: </h3>


STEPS <br>

Modified the CPUID exit handler code in cupid.c and vmx.c. Changes to KVM Hypervisor are done in arch directory. <br>
Below is the path of those files <br>
linux/arch/x86/kvm/vmx/vmx.c  <br>
linux/arch/x86/kvm/cupid.c   <br>

First instruction 0x4FFFFFFF. Here we need to know the total exits of all types when the exit equals to 0x4FFFFFFF instruction. We can do this by incrementing it by 1 whenever the instructions equal the value, using an if condition. <br>

We can do this by following steps, <br>
	• In vmx.c, initialize a global variable total_exits using u32 and increment its value by 1. <br>
	Note you have to use EXPORT_SYMBOL for minimizing the error undefined total_exits. <br>
	• In cupid.c, use extern to extend the visibility of variables like EXPORT_SYMBOL. Write an if condition to equal eax to total_exits satisfying the condition.  <br>

Second instruction 0x4FFFFFFE. In this instance we have to find total time spent for low and high cycles. We can use rdtsc() function, which is a time stamp counter.  <br>

Run the below commands <br>
make modules install <br>
make INSTALL_MOD_STRIP=1 modules_install && make install <br>
lsmod | grep kvm. <br>
modprobe kvm <br>

Created a nested VM. KVM is a hypervisor, we changed the modified/ added the code inside the KVM, so to see the changes, we need to install a VM inside a VM through KVM. <br>

Tested my code if it is working fine. ‘cpuid’ is a package in Ubuntu that gives detailed information about the CPU(s) gathered from the CPUID instruction.
Followed this command to install it  <br>
	
sudo apt-get install cupid  <br>

cpuid -l 0x4FFFFFFF <br>
cpuid -l 0x4FFFFFFE <br>


<h3>Assignment 3: </h3> <br>


STEPS <br>

Steps used to complete the assignment : <br>
Added code in cpuid.c and vmx.c to implement the functionality for eax= 0x4FFFFFFD and 0x4FFFFFFC as mentioned in the question.  <br>

Below are the commands used for the process. <br>

make -j 8 modules  <br>
sudo bash    <br>
make INSTALL_MOD_STRIP=1 modules_install && make install   <br>
lsmod | grep kvm   <br>
rmmod kvm_intel   <br>
rmmod kvm   <br>
modprobe kvm   <br>
modprobe kvm_intel  <br>
lsmod | grep kvm  <br>

Below commands are used to login into the inner VM  <br>
sudo virsh start centOSvm  <br>
sudo virsh console centOSvm  <br>

Questions :  <br>
**Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?**  <br>
No, the number of people who exist does not rise at a consistent rate. During operation, more exits are done, such as MSR access, IO instructions, and EPT violation. On a full VM boot, there are a total of 1371554 exits.  <br>

**Of the exit types defined in the SDM, which are the most frequent? Least?**  <br>
The majority of frequent departures are associated with EPT Violation(48), EPT Misconfiguration(49), MSR access(32), CPUID(10), HLT(12),IO Instruction(30), and Interrupt Window (7). VMWRITE, VMXON, VMXOFF, VM-entry failure owing to machine-check event, and other exits are among the least common.  <br>


<h3>Assignment 4: </h3>


STEPS <br>

Steps used to complete the assignment : <br>

Noted total the exit count information in assignment 3  <br>
To shutdown inner vm, below command is used   <br>
sudo virsh shutdown centOSvm   <br>
Removed kvm_intel module using below command  <br>
rmmod kvm-intel  <br>
Reloaded the kvm-intel module with the parameter ept=0  <br>
insmod /lib/modules/5.18.0-rc3+/kernel/arch/x86/kvm/kvm-intel.ko ept=0  <br>
Started inner vm(centOSvm) again using below command  <br>
sudo virsh start centOSvm  <br>
Noted all the total exit count data  <br>

**What did you learn from the count of exits? Was the count what you expected? If not, why not?** <br>
The count of exits increased significantly in my output. This is the expected behaviour as shadow paging requires more number of exits to be performed due increased page faults, TLB flushes and CR3 exit  <br>
**What changed between the two runs (ept vs no-ept)?**  <br>
There is an increase in number of exits for NMI(0), External Interrupt(1), Interrupt Window(7), CPUID(10), IO Instruction(30), INVPCID(58), CR Access(28) among other exits.   <br>

