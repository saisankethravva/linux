# CMPE 283 Assignment 2

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
  
  ![image](https://user-images.githubusercontent.com/38378122/205841844-46668f50-b0cb-4f03-8392-84179420113a.png)

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
step 8: if everything is loaded perfectly then we can test it by creating a VM upon the GCP VM follow the below steps
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
 
   
   

