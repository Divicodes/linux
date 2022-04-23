<h2> CMPE 283 <br>
Divija Naredla - 015945969 <br>
SPRING 2022 </h2>

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
