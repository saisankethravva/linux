# CMPE 283 Assignment 2

step 1: Created a Virtual Machine instance in Google Cloud Platform with 100 GB disk space and enabling nested Virtualization.  

![image](https://user-images.githubusercontent.com/38378122/205823594-48fcb850-dffd-4473-beb3-da84d691a95a.png)  


![image](https://user-images.githubusercontent.com/38378122/205823686-2e9011f3-e6cc-4e6f-b76b-6f02d31e9370.png)


step 2:  Forked the Git repository from https://github.com/torvalds/linux to my github account and cloned it into the GCP VM instance.   

```git clone https://github.com/saisankethravva/linux ```

step 3: Built the kernel by following the steps specified in the video. After performing all the steps- The new kernel version is booted.   
   
``` saisanketh_ravva@cmpe283:~$ uname -a  
         Linux cmpe283 6.1.0-rc7+ #1 SMP PREEMPT_DYNAMIC Sun Dec  4 13:11:44 UTC 2022 x86_64 GNU/Linux ```

