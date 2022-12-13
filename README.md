# CMPE 283 Assignment 3


## Question 1:
Assignment is done By myself.

## Question 2: (Step by step procedure to complete the assignment and test the functionality)

step 1: Created a Virtual Machine instance in Google Cloud Platform with 100 GB disk space and enabling nested Virtualization.  

![image](https://user-images.githubusercontent.com/38378122/205823594-48fcb850-dffd-4473-beb3-da84d691a95a.png)  


![image](https://user-images.githubusercontent.com/38378122/205823686-2e9011f3-e6cc-4e6f-b76b-6f02d31e9370.png)


step 2:  Forked the Git repository from https://github.com/torvalds/linux to my github account and cloned it into the GCP VM instance.   

```
git clone https://github.com/saisankethravva/linux 
```

step 3: Built the kernel by following the steps specified in the video. After performing all the steps- The new kernel version is booted.    


![image](https://user-images.githubusercontent.com/38378122/205826065-4a79e5a4-dfc2-4dd5-8946-57d598aa6e17.png)

 Step 4: move to cpuid.c and vmx.c files to make changes as per our assignment
 
 step 5: move to linux folder and type the below command to build modules
 
 ```  make -j 8 modules  ```
 
 step 6: move to sudo bash to install modules
 
 ``` sudo bash  
   make INSTALL_MOD_STRIP=1 modules_install
   ```
  ![2nd reg](https://user-images.githubusercontent.com/38378122/205847953-fe2cb229-78ce-4625-88f9-59f79a267be5.PNG)


![2nd reg2](https://user-images.githubusercontent.com/38378122/205848003-066f212b-270a-4d75-9f34-fe8a3c826cd9.PNG)


step 7: verify the KVM modules loaded if not load them 

``` 
  root@cmpe283:/home/saisanketh_ravva/linux# rmmod kvm_intel
  root@cmpe283:/home/saisanketh_ravva/linux# rmmod kvm
  root@cmpe283:/home/saisanketh_ravva/linux# modprobe kvm
  root@cmpe283:/home/saisanketh_ravva/linux# modprobe kvm_intel
  root@cmpe283:/home/saisanketh_ravva/linux# lsmod | grep kvm
  kvm_intel             352256  2
  kvm                  1126400  1 kvm_intel
  irqbypass              16384  1 kvm
```
step 8: if everything is loaded perfectly then we can test it by creating a nested VM upon the GCP VM, follow the below steps to
   Download the Ubuntu cloud image from this site (https://cloud-images.ubuntu.com/).
   ```
   sudo apt-get install wget
   wget https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64.img
   ```
   install necessary qemu packages by below command
   ```
   sudo apt update && sudo apt install qemu-kvm -y
   ```
 step 9: goto the directory where you downloaded cloud image file to setup the password to login into the new VM
 
 ```
 sudo apt-get install cloud-image-utils   
 
 cat >user-data <<EOF  
    password: sanketh  
    chpasswd: { expire: False }  
    ssh_pwauth: True  
    EOF
    
cloud-localds user-data.img user-data
```

step 10: execute the ubuntu image VM by using below command

```
sudo qemu-system-x86_64 -enable-kvm -hda bionic-server-cloudimg-amd64.img -drive "file=user-data.img,format=raw" -m 512 -curses -nographic
```
now you will be redirected to new terminal of VM  
login to the VM by username : ubuntu and password give in user-data file (in this case it is "sanketh")

step 11: To check the functionality, we need to install cpuid utility package by below command

```
sudo apt-get update  
sudo apt-get install cpuid  
```
step 12: Test the cpuid 0x4FFFFFFE and 0x4FFFFFFE using below script test_cpuid.sh.

```
#!/bin/bash

for i in {0..76}
do
    echo "EXIT $i"
    cpuid -l $1 -s $i
done
```

step 13: to test cpuid 0x4FFFFFFE run below command

```
bash test_cpuid.sh 0x4ffffffe
```
 ![ffe nestedvm](https://user-images.githubusercontent.com/38378122/207184419-3f7a4f14-79f7-47f9-8a99-e9c0818c67ee.PNG)
 
 run sudo dmesg in GCP VM terminal to see the output
 
 ![ffe1](https://user-images.githubusercontent.com/38378122/207184616-06a71379-9954-47da-9e6b-cea26e93e9a2.PNG)


![ffe2](https://user-images.githubusercontent.com/38378122/207184625-ff1861ac-2972-441e-80d6-2f21d17d2ad6.PNG)

step 14: to test cpuid 0x4FFFFFFF run below command


```
bash test_cpuid.sh 0x4ffffffe
```
![ff nested vm](https://user-images.githubusercontent.com/38378122/207184925-dd6ae170-b937-4b95-b5c7-41d956e51087.PNG)

run sudo dmesg in GCP VM terminal to see the output

![ff1](https://user-images.githubusercontent.com/38378122/207185062-1ac99351-6a9b-418c-9227-b76a3bec65af.PNG)

![ff2](https://user-images.githubusercontent.com/38378122/207185073-df1ffaa0-ba2b-42c1-ba17-3d9a4fd961bd.PNG) 

## Question 3:
Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?

### Answer: 

The number of exits are increasing at a study rate for exit numbers 10 and 12.
![image](https://user-images.githubusercontent.com/38378122/207193997-d0796baa-7db6-4f2b-b8d4-5df9456e71f6.png)

![image](https://user-images.githubusercontent.com/38378122/207194365-72a80e15-38db-41a2-821d-2bf2c73ecf8f.png)


## Question 4:
Of the exit types defined in the SDM, which are the most frequent? Least?

### Asnwer:

The most Frequently called exit is Exit number- 30 (IO_INSTRUCTION). Total exits count=898930  
The least Frequently called exit is Exit number- 29 ( DR_ACCESS). Total exits count=1, Ofcourse there are some exit reasons with Zero count too.  
example exit reasons from 13 to 24.



# CMPE 283 Assignment 2

## Question 1:
Assignment is done By myself.

## Question 2: (Step by step procedure to complete the assignment and test the functionality)

step 1: Created a Virtual Machine instance in Google Cloud Platform with 100 GB disk space and enabling nested Virtualization.  

![image](https://user-images.githubusercontent.com/38378122/205823594-48fcb850-dffd-4473-beb3-da84d691a95a.png)  


![image](https://user-images.githubusercontent.com/38378122/205823686-2e9011f3-e6cc-4e6f-b76b-6f02d31e9370.png)


step 2:  Forked the Git repository from https://github.com/torvalds/linux to my github account and cloned it into the GCP VM instance.   

```
git clone https://github.com/saisankethravva/linux 
```

step 3: Built the kernel by following the steps specified in the video. After performing all the steps- The new kernel version is booted.    


![image](https://user-images.githubusercontent.com/38378122/205826065-4a79e5a4-dfc2-4dd5-8946-57d598aa6e17.png)

 Step 4: move to cpuid.c and vmx.c files to make changes as per our assignment
 
 step 5: move to linux folder and type the below command to build modules
 
 ```  make -j 8 modules  ```
 
 step 6: move to sudo bash to install modules
 
 ``` sudo bash  
   make INSTALL_MOD_STRIP=1 modules_install
   ```
  ![2nd reg](https://user-images.githubusercontent.com/38378122/205847953-fe2cb229-78ce-4625-88f9-59f79a267be5.PNG)


![2nd reg2](https://user-images.githubusercontent.com/38378122/205848003-066f212b-270a-4d75-9f34-fe8a3c826cd9.PNG)


step 7: verify the KVM modules loaded if not load them 

``` 
  root@cmpe283:/home/saisanketh_ravva/linux# rmmod kvm_intel
  root@cmpe283:/home/saisanketh_ravva/linux# rmmod kvm
  root@cmpe283:/home/saisanketh_ravva/linux# modprobe kvm
  root@cmpe283:/home/saisanketh_ravva/linux# modprobe kvm_intel
  root@cmpe283:/home/saisanketh_ravva/linux# lsmod | grep kvm
  kvm_intel             352256  2
  kvm                  1126400  1 kvm_intel
  irqbypass              16384  1 kvm
```
step 8: if everything is loaded perfectly then we can test it by creating a nested VM upon the GCP VM, follow the below steps to
   Download the Ubuntu cloud image from this site (https://cloud-images.ubuntu.com/).
   ```
   sudo apt-get install wget
   wget https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64.img
   ```
   install necessary qemu packages by below command
   ```
   sudo apt update && sudo apt install qemu-kvm -y
   ```
 step 9: goto the directory where you downloaded cloud image file to setup the password to login into the new VM
 
 ```
 sudo apt-get install cloud-image-utils   
 
 cat >user-data <<EOF  
    password: sanketh  
    chpasswd: { expire: False }  
    ssh_pwauth: True  
    EOF
    
cloud-localds user-data.img user-data
```

step 10: execute the ubuntu image VM by using below command

```
sudo qemu-system-x86_64 -enable-kvm -hda bionic-server-cloudimg-amd64.img -drive "file=user-data.img,format=raw" -m 512 -curses -nographic
```
now you will be redirected to new terminal of VM  
login to the VM by username : ubuntu and password give in user-data file (in this case it is "sanketh")

step 11: To check the functionality, we need to install cpuid utility package by below command

```
sudo apt-get update  
sudo apt-get install cpuid  
```

step 12: To test the CPUID functionality for 0x4ffffffc lead node type below command in nested VM terminal multiple times

```
sudo cpuid -l 0x4ffffffc
```
![fc2](https://user-images.githubusercontent.com/38378122/205846493-55acfeda-b512-4261-b7c5-505a8cd6fed9.PNG)

Now SSH into GCP VM terminal and give the below command to see output

```
sudo dmesg
```


![exits2](https://user-images.githubusercontent.com/38378122/205846915-2f941640-f260-42e4-b560-72cb397e1df4.PNG)


step 13: To test the CPUID functionality for 0x4ffffffd lead node type below command in nested VM terminal

```
sudo cpuid -l 0x4ffffffd
```


![d2](https://user-images.githubusercontent.com/38378122/205847252-947eb261-7c7d-4181-8228-4311faa78dbe.PNG)

Now SSH into GCP VM terminal and give the below command to see output

```
sudo dmesg
```
![vmm2](https://user-images.githubusercontent.com/38378122/205847407-235d93e0-60dc-45c9-9b8c-3793bf26984f.PNG)


   
   

