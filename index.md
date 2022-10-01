# **Remote Access and the Filesystem**
September 29, 2022

## Installing VScode 
First, visit the VScode website https://code.visualstudio.com/ to download and install VScode. According to what operating system you're utilizing (Windows or Mac), download and install the corresponding version. 

<img width="1510" alt="Screen Shot 2022-09-30 at 5 24 46 PM" src="https://user-images.githubusercontent.com/114449002/193377671-ad0cd113-2a73-497f-b173-a38810cc567b.png">

The starting page should look something like this: 
<img width="1025" alt="Screen Shot 2022-09-30 at 6 22 58 PM" src="https://user-images.githubusercontent.com/114449002/193377692-a933028c-1f38-4e6a-a0af-4545c59347c9.png">

## Remotely Connecting
Install [OpenSSH](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui) if you are utilizing window. Otherwise, you can skip this step. 

In VScode, open a new terminal (Ctrl or Command + `, or use the Terminal → New Terminal menu option). 

Enter `ssh cs15lfa22zz@ieng6.ucsd.edu` replacing the letters `zz` to your course-specific account's letters. 

(In this lab, I will be using my ucsd email `bec002@ucsd.edu` instead due to password reset failure)

It would respond with 
`The authenticity of host 'ieng6.ucsd.edu (128.54.70.227)' can't be established.
RSA key fingerprint is SHA256:ksruYwhnYH+sySHnHAtLUHngrPEyZTDl/1x99wUQcec.
Are you sure you want to continue connecting (yes/no/[fingerprint])?`

Enter `yes`

The system would then ask for your password. Login to your course-specific account by entering your password. (Notice the visibility of the password is invisible)

Respond should look something like this: 
<img width="1279" alt="Screen Shot 2022-09-29 at 10 52 22 AM" src="https://user-images.githubusercontent.com/114449002/193377479-690cbb41-f0f0-47cb-8948-eb43403f78a1.png">

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

<img width="563" alt="Screen Shot 2022-09-29 at 10 59 55 AM" src="https://user-images.githubusercontent.com/114449002/193377498-e088ab95-e873-4358-b9c3-3a90d9c77577.png">


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

<img width="1020" alt="Screen Shot 2022-09-29 at 11 25 05 AM" src="https://user-images.githubusercontent.com/114449002/193377511-ad279853-d047-433f-bc08-9ad33a5bb06e.png">

<img width="941" alt="Screen Shot 2022-09-29 at 11 34 21 AM" src="https://user-images.githubusercontent.com/114449002/193377529-6e190cb5-a30a-458e-ad46-cc4e96787b0e.png">

Notice that I had to change directory a few times be at the directory where the file “WhereAmI.java” is actually at. The commands `ls` (list) and `cd` (change directory) are helpful to help you locate and move to the directory where you want to be. Then, I was able to compile and run the file successfully. 

Notice that the os.name is Linux on remote and the os.name is Mac OS X on client, the user.name is bec002 on remote and the user.name is ben on client, and the user.home and user.dir are different. This is because the command `getProperty` is being ran different computers. The command `getProperty` would rectrieve information about the system that it is currently running on.

## Setting an SSH Key
`ssh` keys is a solution to avoid always having to type passwords repetitively to verify actions. ssh utilizes a program called `ssh-keygen`. The program creates a pair of files called the public key and private key. By copying the public key to a particular location on the server, and the private key in a particular location on the client, the ssh command can use the pair of files in place of your password.

On your client, in the terminal, type `ssh-keygen`. Then the system would prompt `Enter file in which to save the key (/Users/"your_username"/.ssh/id_rsa): /Users/"your_username"/.ssh/id_rsa`. Press enter twice specify the default path which would /Users/"your_username/.ssh/id_rsa. 

It should look something like this: 
```
# on client (your computer)
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/joe/.ssh/id_rsa): /Users/your_username/.ssh/id_rsa
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/your_username/.ssh/id_rsa. (this is the private key)
Your public key has been saved in /Users/your_username/.ssh/id_rsa.pub. (this is the public key)
The key fingerprint is:
SHA256:jZaZH6fI8E2I1D35hnvGeBePQ4ELOf2Ge+G0XknoXp0 your_username@your_computer_name.local
The key's randomart image is:
+---[RSA 3072]----+
|                 |
|       . . + .   |
|      . . B o .  |
|     . . B * +.. |
|      o S = *.B. |
|       = = O.*.*+|
|        + * *.BE+|
|           +.+.o |
|             ..  |
+----[SHA256]-----+
```
It should look something like this: 

<img width="607" alt="Screen Shot 2022-09-29 at 11 46 43 AM" src="https://user-images.githubusercontent.com/114449002/193377796-350264ff-7f17-4309-bc04-311b21954d16.png">

```
# on client
$ ssh bec002@ieng6.ucsd.edu
<Enter Password>
```

```
# now on server
$ mkdir .ssh
$ <logout>
``` 

```
# back on client
$ scp /Users/ben/.ssh/id_rsa.pub bec002@ieng6.ucsd.edu:~/.ssh/authorized_keys
# You use your username and the path you saw in the command above
```
  
## Optimizing Remote Running

You can write a command in quotes at the end of an ssh command to directly run it on the remote server, then exit. For example, this command will log in and list the home directory on the remote server:

$ ssh cs15lfa22@ieng6.ucsd.edu "ls"
You can use semicolons to run multiple commands on the same line in most terminals. For example, try:

$ cp WhereAmI.java OtherMain.java; javac OtherMain.java; java WhereAmI
You can use the up-arrow on your keyboard to recall the last command that was run
