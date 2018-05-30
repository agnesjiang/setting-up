# Lab: Setting up your environment

This lab guide has a comprehensive tutorial to complement what we discussed in class. We will guide you step-by-step through the process of:

* uploading your public ssh key to cloud services
* setting up a Security Group on AWS
* learning to use ssh-agent
* learning about terminal based text editors
* starting an Amazon Virtual Machine (also referred to as instance, VM, EC2, remote server)
* connecting to it
* performing some tasks on the VM (the actual assignment)

# Setup

## Create your Secure Shell keypair

**You only need to do this one time.**

* Open a terminal if not already open (Terminal/iTerm2 in Mac, or Babun in Windows). By default, every time you open a terminal it will open in your home directory.
* Type `ssh-keygen -t rsa -b 2048`, press enter
* You will see this prompt, just press enter

```  
Generating public/private rsa key pair.
Enter file in which to save the key (/home/User/.ssh/id_rsa):
```

* You will see this prompt, just press enter

```
Created directory '/home/User/.ssh'.
Enter passphrase (empty for no passphrase):
```

* You will see this prompt, just press enter

```
Enter same passphrase again:
```

* You will see these messages (your randomart will look different) and your keypair has been created.

```
Your identification has been saved in /home/User/.ssh/id_rsa.
Your public key has been saved in /home/User/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:xPJtMLmJSO73x/NQo3qMpqF6r6St4ONmshS8QZqfmHA User@WinDev1802Eval
The key's randomart image is:
+---[RSA 2048]----+
|                 |
|       . .       |
| .  . . *        |
|+. o . = *       |
|++E o . S o o    |
|.=+o     . o .   |
|+oo o o  +o      |
|+= +.o oo.*.     |
|*+=++ooooo o.    |
+----[SHA256]-----+
``` 

### See the contents of your key files

* Open a terminal if not already open (Terminal/iTerm2 in Mac, or Babun in Windows). By default, every time you open a terminal it will open in your home directory.
* Change to your `.ssh` directory. This is a hidden directory so if you list your files using `ls` you won't see it. For seeing all files, use `ls -la`. To change into it type `cd .ssh`.
* Type `pwd` which prints your current working directory. If you are on a Mac, you should see something like:
 
```
$ pwd
/Users/marck/.ssh
```

If you are using Babun or Linux you'll see something like 

```
$ pwd
/home/myusername/.ssh
```

* Type `ls -la` to list the files in the directory. What is displayed is my output and yours may look different. You will not have a `config` file unless you have already created one. The `id_rsa` is your **private key** and this file will not leave your computer. The `id_rsa.pub` file is the **public key**, whose contents we will upload to cloud services so you authenticate. The **known_hosts** is a file that gets generated as you connect to different remote systems.
 
```
$ ls -la
total 32
drwxr-xr-x   6 marck  staff   192 May 29 20:39 .
drwxr-xr-x+ 75 marck  staff  2400 May 30 13:35 ..
-rw-r--r--@  1 marck  staff   181 May 29 15:50 config
-r--------@  1 marck  staff  3243 May 29 15:50 id_rsa
-rw-r--r--@  1 marck  staff   742 May 29 15:50 id_rsa.pub
-rw-r--r--   1 marck  staff   363 May 29 20:42 known_hosts
```
* View the contents of your **public_key** file: `cat id_rsa.pub` (what is shown is my public key, yours will be different.)

```
$ cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCnKuIRXwZu0JZH0/Q2XNrYYTaJT7bMtXGhGQaSSOZs6MhQ4SkSbHiygO7RauQf741buLnASzY27GKMMMml6InwfxJWrF60KhNK0r869POQkuZa9v9/cmYcEIzmAJe1xRPABEZ2yfbTG9Wq4sg9cU0mwt1Bx7wiN4QNf0Bak62EC8JWTbcKLduuzO1zabIb5xW9gfR9b4K3HwmqRLl18S8bNsfYQZfvtlwd0mCWQUeuEGbDOgqh//nLIj6DeXdyxbD5xrz79iOAuAK2nXAjNCEtKpxNGQr2Py7aWQjlH+U5laDEHVg4hzmBY7yoZ5eC3Ye45yPqpQA1y8JrbXVhPJRP User@WinDev1802Eval
```
* Open a text editor (Notepad on Windows or Textpad on Mac, **NOT MICROSOFT WORD**) and select the output of your terminal from the `ssh-rsa` beginning all the way to the end, and paste it in your text editor as-is. We will use this in the next step. You can also just leave it here and copy/paste from your terminal screen.

_For Babun Users Only_

When you created the ssh keypair last week, the permissions of your private key were created incorrectly. This does not happen on Mac/Linux. **This is an important step otherwise your ssh authentication will not work.**

* Type `chmod 400 id_rsa` to change the permissions of the private key
* Type `ls -la` and you should see that the permissions of your private key file look like `-r--------`.

## Upload your Public key to AWS

* Log on to your [AWS Console](https://console.aws.amazon.com/) if you are not already logged in
* Go to the EC2 Dashboard. You can get here by clicking on **Services** in the top left and choosing **EC2** under the **Compute** heading
* Click on **Key Pairs** on the left hand side of the EC2 console. If you don't see it, scroll down the left side panel until you see the **NETWORK & SECURITY** heading, and you will see the following:

<img src='images/create-keypair-1.png'>

* Click on **Import Key Pair** at the top of the screen and the "Import Key Pair" dialog will show:

<img src='images/create-keypair-2.png'>

* In the **Key pair name** box, enter `mykeypair` or a name that is meaningful to you. It can be anything, just make sure you know what it is.
* In the **Public Key Contents** box, paste the contents of the **public key** that you got before.

<img src='images/create-keypair-3.png'>

* Click **Import** and you will see your key in the console.

<img src='images/create-keypair-4.png'>

You will use your key to connect to AWS resources later.

## Upload your Public key to GitHub

* Log on to your [GitHub](http://github.com/) account if you are not already logged in
* Click on your profile icon on the top-right of the screen and select **Settings** from the dropdown
* Click on **SSH and GPG keys** from the left hand menu
* Click on the **New SSH kek** button on the top-right
* Give your key a name (meaningful to you)
* Paste the contents of the **public key** in the Key box, and click the **Add SSH Key** button 

### Test that the ssh key works with GitHub

* Open a terminal if not already open (Terminal/iTerm2 in Mac, or Babun in Windows). By default, every time you open a terminal it will open in your home directory.
* Type `ssh -T git@github.com` and press enter to test. If it works, you will see something like this, with your GitHub username:

```
The authenticity of host 'github.com (192.30.253.112)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,192.30.253.112' (RSA) to the list of known hosts.
Hi wahalulu! You've successfully authenticated, but GitHub does not provide shell access.
```

* **You are now ready to use ssh authentication with GitHub.**

## Creating a Security Group

Before we launch our first AWS EC2 instance, we will setup a _Security Group_ in the AWS Console. Each account comes with a default security group, but we will create a specific one that we will use to connect to our EC2 instance via ssh, which means it will open up port 22 on your instance.

* Log on to your [AWS Console](https://console.aws.amazon.com/) if you are not already logged in
* Go to the EC2 Dashboard. You can get here by clicking on **Services** in the top left and choosing **EC2** under the **Compute** heading
* Click on **Security Groups** on the left hand side of the EC2 console. If you don't see it, scroll down the left side panel until you see the **NETWORK & SECURITY** heading, and you will see the following:

<img src='images/create-security-group.png'>

* Click on **Create Security Group**. 
	* Enter a name for your security group (you can call it whatever you want; I called it **open-22** to make it easy.) 
	* Enter a description for your group
	* Leave the VPC value unchanged (your number will be different than the one shown below.)
	* Click on **Add Rule** towards the bottom
	* Select **SSH** from **Type**
	* Select **Anywhere** from **Source**
	* Click **Create**

<img src='images/security-group-settings.png'>

* You have just created a security group that opens up port 22 and allows you to connect from anywhere, and you should see it in the Security Groups console (again, the group id will be different for each of you). You will use this group that you just created when you launch an instance for this assignment

<img src='images/security-group-console.png'>


## Launch your first Linux instance on AWS

This is a step-by-step guide on how to launch an instance.

* From the **EC2 Dashboard**, click on the **Launch Instance** button

<img src='images/launch-instance-01.png'>

* _Step 1: Choose an Amazon Machine Image:_ Select the **Amazon Linux 2 LTS Candidate AMI... (ami-428aa838)**. If you don't see this you can search for it by clicking the **Community AMIs** tab on the left and entering the ami number

<img src='images/launch-instance-02.png'>

* _Step 2: Choose an Instance Type:_ Select the **t2.micro** instance type, and click **Next**

<img src='images/launch-instance-03.png'>

* _Step 3: Configure Instance Details:_ Nothing to do here, click **Next**

<img src='images/launch-instance-04.png'>

* _Step 4: Add Storage:_ Nothing to do here, click **Next**

<img src='images/launch-instance-05.png'>

* _Step 5: Add Tags:_ Nothing to do here, click **Next**

<img src='images/launch-instance-06.png'>

* _Step 6: Configure Security Group:_ Select the **open-22 (or whichever name you gave it)** Security Group, click **Review and Launch**

<img src='images/launch-instance-07.png'>

* _Step 7: Review Instance Launch:_ You will see a warning about your security group being open to the world. You can ignore it, we know. Make sure that the AMI, the Instance Type, and the Security Group is what you wanted. Click **Launch**

<img src='images/launch-instance-08.png'>

* _Select an existing key pair or create a new key pair:_ **Make sure you select the key that you uploaded to AWS, otherwise you will not be able to connect to your machine. Click Launch Instances**

<img src='images/launch-instance-09.png'>

* _Launch Status:_ Click **View Instances** to go back to the EC2 Instances page

<img src='images/launch-instance-10.png'>

* In the EC2 Console, you will see the status of your instance. Sometimes the instances are launched immediately, but sometimes you have to wait. While waiting, you will see the Instance Status showing a yellow dot with "pending", otherwise it will be a green dot with "running"

* When the instance is in "running" state, it is ready to be used!

## Connecting to the remote server using Secure Shell `ssh

We will be using the terminal with Secure Shell to connect to our remote instance. 

* Open a terminal on your laptop. If you have one already open, type `cd ~` to navigate back to your home directory
* Go to the EC2 Instances page (click on EC2, then on Instances on the left)
* Click on your running instance such that the select box is blue
* Click in the **Description** tab in the bottom if not already selected
* Hover with your mouse over the **Public DNS** text, and you will see a "Copy to Clipboard" icon to the right. Click on that to copy your instances DNS address.

<img src='images/ec2-running.png'>

* In the terminal, type `cd ~` to make sure you are in your home directory
* Type `ssh ec2-user@` and paste your DNS address
* If everything is configured correctly, the first time you connect to a host that you've never connected to, you will see something like this:

```
The authenticity of host 'ec2-54-163-133-120.compute-1.amazonaws.com 
(54.163.133.120)'can't be established.
ECDSA key fingerprint is SHA256:TeYrgHLkYHvD/zcp23bO3wozsLMyPSiSn+edPPo88zE.
Are you sure you want to continue connecting (yes/no)?
```
Note: your values for DNS, IP address and fingerprint will be different

* Enter `yes` and press enter. You will only need to enter yes once.
* If you are successful, you will see something like this:

```
The authenticity of host 'ec2-54-163-133-120.compute-1.amazonaws.com (54.163.133.120)' 
can't be established.
ECDSA key fingerprint is SHA256:TeYrgHLkYHvD/zcp23bO3wozsLMyPSiSn+edPPo88zE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'ec2-54-163-133-120.compute-1.amazonaws.com,54.163.133.120' (ECDSA) to the list of known hosts.
Last login: Thu Jan 25 01:41:17 2018 from pool-108-45-73-252.washdc.fios.verizon.net

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
[ec2-user@ip-172-31-59-103 ~]$
```

**Congratulations, you have successfully connected to your remote instance!**


## Use ssh agent

One thing to keep in mind is that whenever you want to use your GitHub repository on your cloud resources, you will need to connect to your remote machines and use the SSH agent functionality to pass your ssh private key and store it in the remote server's memory. **You do not need to do this every time you connect to your remote machine; only when you want to clone/push a repository between your remote instance and GitHub.** 

You can read more about ssh-agent [here.](https://en.wikipedia.org/wiki/Ssh-agent)

If you are already connected to your remote host and did not use the ssh-agent, you can exit the remote terminal by typing `exit` and that will bring you back to your "local" terminal. 

__For Mac and Linux:__ 

* Type `ssh-add` in your terminal. You will get a confirmation that looks something like this:

```
➜  ~ ssh-add 
Identity added: /Users/marck/.ssh/id_rsa (/Users/marck/.ssh/id_rsa)
```
Note: if you have issues with `ssh-add` command on Mac/Linux, you can use the instructions for Windows. 

__For Windows:__ 

* in your Babun terminal, you need to use two commands: ``eval `ssh-agent -s` ``, and then `ssh-add`.

```
{ ~ } >> eval `ssh-agent -s`
Agent pid XXXX
{ ~ } >> ssh-add
Identity added: /home/xxxx/.ssh/id_rsa 
```

To log-on to the remote server with your ssh-agent, you need to use the `-A` parameter in your ssh command: `ssh -A ec2-user@...` where `...` is your instance's DNS. 

To test that the ssh-agent forwarded your private key to the remote machine you can test the connection to GitHub as we did before: 

* Type `ssh -T git@github.com` on your **remote** terminal
* If the `ssh-agent` worked properly, you should get a success message from GitHub:

```
[ec2-user@ip-172-31-59-103 ~]$ ssh -T git@github.com
Hi wahalulu! You've successfully authenticated, but GitHub does not provide shell access.
```

## Install git in your remote machine

The AMI image we chose is a bare-bones Linux image. You will need to install git on the remote machine so you can clone/push your repository.

* At the remote terminal prompt, type: `sudo yum -y install git`

When git is finished installing, you will see something like this:

<img src='images/git-install.png'>


## Use a text editor on your remote machine

You will be creating and editing text files using a text editor on the remote machine. Remember, you are working on a remote machine so there is no graphical user interface (GUI) like Notepad, Texpad or your favorite editor. You will be using plain text editors in the command line. Most Linux distributions come with [nano](https://www.nano-editor.org/) and [vim](http://www.vim.org/) installed. Personally, I like `nano` but you are welcome to use `vim`. Click on the links to learn more about each editor.

# Tasks


## Task 1 - Create a text file called my-text-file.txt

1. Using your preferred text editor, create a text file called `my-text-file.txt` with the following contents, replacing `______` with your name.

	```
	Hi,
	
	I am ______ and I just created a text file on a Linux server running in the cloud!
	```


## Task 2 - Download a file using `wget` and use `grep` to find certain lines

For this task, you will be working with a text file that you download using the command line. The file contains hypothetic measurements of a scientific instrument called a _quazyilx_ that has been specially created for this class. Every few seconds the quazyilx makes four measurements: _fnard_, _fnok_, _cark_ and _gnuck_. The output looks like this:

    YYYY-MM-DDTHH:MM:SSZ fnard:10 fnok:4 cark:2 gnuck:9

(This time format is called [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601) and it has the advantage that it is both unambiguous and that it sorts properly. The Z stands for _Greenwich Mean Time_ or GMT, and is sometimes called _Zulu Time_ because the [NATO Phonetic Alphabet](https://en.wikipedia.org/wiki/NATO_phonetic_alphabet) word for **Z** is _Zulu_.)

When one of the measurements is not present, the result is displayed as negative 1 (e.g. `-1`). 

The quazyilx has been malfunctioning, and occasionally generates output with a `-1` for all four measurements, like this:

    2015-12-10T08:40:10Z fnard:-1 fnok:-1 cark:-1 gnuck:-1

1. Download the _quazyilx_ file

	```
	wget s3.amazonaws.com/bigdatateaching/quazyilx/quazyilx0.txt
	```
1.	We will be using the `grep` command to find the lines where the _quazyilx_ failed

	```
	grep "fnard:-1 fnok:-1 cark:-1 gnuck:-1" quazyilx0.txt
	```
	
1. Using the redirect command that you learned about in Task 2, combine grep and redirect to create a new file called `quazyilx-failures.txt` 



