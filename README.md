# vChat_On_Linux
This is a basic description of how to run the [vChat](https://github.com/xinwenfu/vchat) server on Linux, and which versions of Linux gave the best results when running Meterpriter through Wine. Due to the nature of both the vChat server and Wine difficulties may be faced depending on how your system, and Wine are configured (or updated!).

## Environment
### Wine
[Wine](https://www.winehq.org/) is a compatibility layer program which allows Windows programs to be run on Linux systems by converting windows system calls to POSIX/Linux call on the fly. This can be installed on a Linux system using a distribution's normal package manager.

```bash
$ sudo apt install Wine
```

![Some example output -- my own machine](/images/Wine_Install.png)

**Once Wine is installed** you can run vChat.exe with wine, since this will be the first time running the program it will have to create and configure files, so it may take some time. The vChat.exe file **should be** in the **same** directory as the necessary DLLs, like **essfunc.dll**. Further whenever I ran the program through wine, I did it from the directory the vChat.exe was located in.

> See screenshots and a more detailed explanation in the [Running the vChat executable](#running-the-vchat-executable)) section

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
___

## Running the vChat executable

### Starting vChat 
As mentioned earlier you utilize wine, running as a normal user, or root user to allow the windows vChat server to work on POSIX/Linux systems. When running Wine as either a normal or root user for the first time will require some time to setup it's necessary configuration files. 

See the examples below of how to run vChat:
``` bash 
$ wine vChat.exe

#Or 

$ sudo wine vChat.exe
```
![Without Sudo](/images/Wine-UB-Fu.png)
![With Sudo](/images/Wine-Sudo-Run.png)
> Do note that these images were taken on a separate Ubuntu machine then previously pictured for the installing image. When creating them it I made the user "kali". This is not a kali machine! The username was chosen so I could keep users identical between my VMs (-Matt).

Do note that Wine may ask you to install additional drivers, in the event of this occuring follow any instructions given (that make sense!).

It is also possible that Wine will produce an error when attempting to run the program similar to the following:
```
kernel32.dll could not be loaded
```
In this case you may need to run wine as a root user (using sudo). If the issue persists you can remove the wine configuration folder located in the hidden **.wine** directory the user's (and or root's) home directory and reconfigure wine (simply by launching it again) as this my be a configuration/setup error. 


The following is an example of Wine starting the configuration process running it as sudo for the first time:
![Sudo first configuration](/images/Sudo-CFG.png)
> There are a number of errors and various output occuring in the backround, they appear to have no affect when running vChat.

### Reconfigure Wine
It was not necessary for us to change any default configurations in order to get vChat to work with Wine. However you may find it interesting to change certain configurations to see how it affects the program. You can do this using Wine's provided configuration tool [winecfg](https://wiki.winehq.org/Winecfg). This will provide a graphical interfaces to configure Wine, and the environment vChat will appear to be in. See example below:
```bash
$ winecfg
```
![WineConfiguration](/images/winecfg.png)

### Wine's Debugger
In the event Wine crashes, Wine's debugger will intercept the exception or fault caused and display the state of the program at the time of the [crash](https://wiki.winehq.org/Wine_Developer%27s_Guide/Debugging_Wine#:~:text=3%20Using%20the%20Wine%20Debugger). This includes the stack contents, error codes, ect. First it will notify you that an exception occurred, then you can look at further details. See an example of both below:

![Initial Notification](/images/debug_catch.png)
![More Details](/images/debug_details.png)


You will likely encounter this when you insert values onto the stack to exploit the vChat server and evidently crash it!

