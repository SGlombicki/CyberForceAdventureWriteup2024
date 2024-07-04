# Anomaly 60 - Underrated Phising eXercise

We are give a binary. Throwing it into binary ninja we can see a big issue.

![image_56.png](image_56.png)

There is a bunch of blank space we can see that on the right side and all the black. Looking at the functions and entry we can see it again confirms my suspicions.
So we need to figure out the packing solution. Guess on the name of the challenge i try upx

![image_54.png](image_54.png)

This returns that it didnt pack so I will run DIE and nfdc to see what it thinks packed it. (Get them from [here](https://horsicq.github.io)) These are tools that try to guess the packer and linker and will tell us alot about the binary

![image_55.png](image_55.png)

Since it believes it was packed by upx and we cant force it this must be custom then. So we are going to decompile it from memory. I am going to look at the memory sections for this binary.

![image_57.png](image_57.png)

We see this one section that is the blank 
![image_58.png](image_58.png)

I am assuming that it is putting the program into here. I am going to use gdb to dump this memory section once the program is running.

![image_53.png](image_53.png)

Now we got to analyze this file for the flag. Doing the basic check and hoping it is a string we search this dump for 'flag'

![image_59.png](image_59.png)