# vChat_On_Linux
This is a basic description of how to run the vChat server on Linux, and which versions of Linux gave the best results when running Meterpriter through Wine. Due to the nature of both the vChat server and Wine difficulties may be faced depending on how your system, and Wine are configured (or updated!).

## Environment
### Wine
[Wine](https://www.winehq.org/) is a compatibility layer program which allows Windows programs to be run on Linux systems by converting windows system calls to POSIX/Linux call on the fly. This can be installed on a Linux system using it's normal package manager.

```bash
$ sudo apt install Wine
```

![Some example output -- placeholder](images\Wine_Install.png)

**Once Wine is installed** you can run vChat.exe with wine, since this will be the first time running the program it will have to configure some file, so it may take some time. The vChat.exe file **should be** in the **same** directory as the necessary DLLs, like **essfunc.dll**.

So the command will look like the following:
```bash
wine vChat.exe
```

Wine may ask you to install additional drivers, in the event of this follow any instructions given (that make sense!)

Wine may also produce an error such as:
```
kernel32.dll could not be loaded
```
![Placeholder](https://s3-alpha.figma.com/hub/file/948140848/1f4d8ea7-e9d9-48b7-b70c-819482fb10fb-cover.png)
In this case you may need to run wine as a root user (using sudo). If the issue persists you can remove the wine configuration folder located in the hidden **.wine** directory the user's (and or root's) home directory and reconfigure wine (simply by launching it again).


### Linux
As mentioned earlier, Wine will run on most if not all common Linux distributions and versions. But due to reasons we likely cannot control such as kernel versions, the Meterpriter shell code that will be deployed will work better on some Linux distributions and versions.

**All**
* Launching the vChat program and exploiting it will work regardless of the distribution and OS version.
* Local security features such as Address Space Layout Randomization (ASLR) do not appear to affect the vChar program when loaded with Wine. 
  * This is likely due to how Wine loads both [itself](https://wiki.winehq.org/Wine_Developer%27s_Guide/Architecture_Overview), the necessary [DLLs](https://wiki.winehq.org/Wine_Developer%27s_Guide/Kernel_modules) and the program it is *running*
* Basic functions of Meterpriter appear to work across the different distributions and versions
  * This includes file system traversal, accessing and editing files, ect.
* Interacting with host system processes may not work, as the linux processes may be hidden from Wine. 
* Windows specific attacks and resources may not work or be accessible (They, and their specific conditions may not exist on the Linux Distribution) ex. SAM database.
* It was **not possible** to launch the attack on a vChat server hosted locally on the same system with Wine, socket errors occurred.

**Ubuntu**
* This is the Linux Distribution vChat and later Meterpriter running through Wine appeared to work the best on.
  * Once Meterpriter is running on the system it is able to access attached devices like webcams
  * On most current distributions I was unable to access the display, to take screenshots and stream the screen.
* Ubuntu version 20.4.3? worked the best allowing for access to both the attached webcam and display

**Kali Linux**
* This Linux distribution appears to work to a similar level that Ubuntu does.
  * Meterpriter is unable to access attached devices such as web cams.
  * Wine may need root (sudo) privileges in order to run properly.
* I was unable to find a version that work to a similar degree 20.4.3? did.
