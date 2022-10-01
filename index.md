# **Remote Access and the Filesystem**
September 29, 2022

## Installing VScode 
First, visit the VScode website https://code.visualstudio.com/ to download and install VScode. According to what operating system you're utilizing (Windows or Mac), download and install the corresponding version. 

## Remotely Connecting
Install [OpenSSH](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui) if you are utilizing window. Otherwise, you can skip this step. 

In VScode, open a new terminal (Ctrl or Command + `, or use the Terminal → New Terminal menu option). 

Enter `ssh cs15lfa22zz@ieng6.ucsd.edu` replacing the letters `zz` to your course-specific account's letters. 

It would respond with 
`The authenticity of host 'ieng6.ucsd.edu (128.54.70.227)' can't be established.
RSA key fingerprint is SHA256:ksruYwhnYH+sySHnHAtLUHngrPEyZTDl/1x99wUQcec.
Are you sure you want to continue connecting (yes/no/[fingerprint])?`

Enter `yes`

The system would then ask for your password. Login to your course-specific account by entering your password. (Notice the visibility of the password is invisible)

Respond should look something like this: 

Image 


## Trying Some Commands
You can then play around by trying to run some commands. Try typing some commands such as 
- cd ~
- cd
- ls -lat
- ls -a
- ls <directory> where <directory> is /home/linux/ieng6/cs15lfa22/cs15lfa22abc, where the abc is one of the other group members’ username
- cp /home/linux/ieng6/cs15lfa22/public/hello.txt ~/
- cat /home/linux/ieng6/cs15lfa22/public/hello.txt

Respond should look something like this: 

Image 

To log out of the remote server type: 

- Ctrl-D
- Run the command `exit`

## Moving Files with scp
An essential part to working remotely is capability to copy files back and forth between the computers.

This can be accomplished through the utilization of the command `scp`. This command is always ran from the client. 

Lets try moving a file with scp. 

First, create a file in VScode called WhereAmI.java and copy and paste the following into it. 

```
class WhereAmI {
  public static void main(String[] args) {
    System.out.println(System.getProperty("os.name"));
    System.out.println(System.getProperty("user.name"));
    System.out.println(System.getProperty("user.home"));
    System.out.println(System.getProperty("user.dir"));
  }
}
```

Compile and run the WhereAmI program through entering javac and java command in the terminal. 
```
javac WhereAmI.java
java WhereAmI
```
In the terminal from the directory where you made this file, run the following command (replacing `zz` to your course-specific account's letters): 
`scp WhereAmI.java cs15lfa22zz@ieng6.ucsd.edu:~/`

It would then prompt you to enter your password again asking for verification to copy the file. 

Open a new terminal and log into ieng6 server again. The file should then be in your home directory. You can check by utilizing the command `ls`

It should look something like this: 

Image

Notice that I had to change directory a few times be at the directory where the file “WhereAmI.java” is actually at. The commands `ls` (list) and `cd` (change directory) are helpful to help you locate and move to the directory where you want to be. Then, I was able to compile and run the file successfully. 

Notice that the os.name is Linux on remote and the os.name is Mac OS X on client, the user.name is bec002 on remote and the user.name is ben on client, and the user.home and user.dir are different. This is because the command `getProperty` is being ran different computers. The command `getProperty` would rectrieve information about the system that it is currently running on.

## Setting an SSH Key
`ssh` keys is a solution to avoid always having to type passwords repetitively to verify actions. 

## Optimizing Remote Running
