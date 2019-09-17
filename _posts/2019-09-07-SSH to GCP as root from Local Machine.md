---
layout: post
hero-bg-color: "#000"
title:  "SSH to GCP instance as the Root User"
date: 2019-09-07
category: Misc
#github: FaceSearch
thumb: "gcp.png"

subtitle: "Connect to your GCP instance from your local machine as the root user."
worktype: "Cloud Computing"
summary: "Google cloud SDK by default logs you in as a different user when connected via SSH. The steps to overcome this limitation and login as the root user are discussed briefly in the tutorial. BONUS: If your ISP blocks the default SSH port, I have also included a small section that still allows you to SSH to your GCP instance, using an alternative port."
#progress: 100
---

&nbsp;&nbsp;&nbsp;&nbsp;I recently created my own Google Cloud Platform account and had to setup the whole thing by myself. After going through multiple tutorials online, I finally managed to SSH as the root user to my instance from my local system, even though my ISP blocks outbound connections from port 22. The process was slightly hectic and there wasn't a single tutorial covering the whole of it on the internet, so I thought I would share the complete list of steps myself.

So, here you go:

## Logging in as the root user

### On Local System

Perform the following steps on your local machine.

1. Run the following command to generate a key.
```bash
$ ssh-keygen
```
You'll be asked to enter passphrase. You can leave that empty.
```bash
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```
A pair of keys (public and private) will now be generated and saved at the location you earlier entered.

```bash
Your identification has been saved in /Users/username/.ssh/id_rsa.
Your public key has been saved in /Users/username/.ssh/id_rsa.pub.
The key fingerprint is:
<Your Key Here> your@email.com
```

2. Start the `ssh-agent`:
```bash
$ eval `ssh-agent`
Agent pid 59566
```
add your key to the agent:
```bash
$ ssh-add ~/.ssh/id_rsa
```

3. Copy the contents of `~/.ssh/id_rsa.pub` to your clipboard. The file location can vary if you chose a different path for your key earlier.

Hint: It starts with `ssh-rsa` and ends with `something@something`.

### On your GCP instance

Perform the following steps on your GCP instance.

1. Connect to your instance by choosing _Open in browser window on custom port_ option.

![Open GCP instance in a browser window](https://tlgur.com/d/gvabz0pG)

2. Open `~/.ssh/authorized_keys` file for editing:
```bash
$ sudo nano ~/.ssh/authorized_keys
```
Paste the contents of clipboard and press `CTRL` + `S` to save the file. Press `CTRL` + `X` to exit the editor.

3. Allow for root login on SSH connections. Open `/etc/ssh/sshd_config` for editing:
```bash
$ sudo nano /etc/ssh/sshd_config
```
Uncomment the line starting with `PermitRootLogin prohibit-password` and add another line right below it as shown:
```bash
PermitRootLogin prohibit-password
PermitRootLogin yes
```
**Note**: For systems older thatn Ubuntu 16.04, The lines would look like:
```bash
PermitRootLogin without-password
PermitRootLogin yes
```
Save the file and exit editor.

4. Restart SSH service.
```bash
sudo service ssh reload
```

### Connect to GCP from you local machine.

If you followed all the steps correctly, give yourself a pat on the back. You did it!

Now you should be able to ssh to your instance as root user by doing:
```bash
$ ssh <your root username>@<public IP of your instance>
```


## Bonus: If outbound port 22 is blocked on your network

If your ISP has blocked the default SSH port (22), you can use SSH over the HTTPS port (443), which is generally open. Follow the steps given below.

### On your GCP instance
1. Connect to your instance by choosing _Open in browser window on custom port_ option.

![Open GCP instance in a browser window](https://tlgur.com/d/gvabz0pG)

2. Open `/etc/ssh/sshd_config` file for editing:
```bash
$ sudo nano /etc/ssh/sshd_config
```
3. Uncomment the line
```bash
#Port 22
```
and replace it with
```bash
Port 443
```
4. Save the file and exit the editor.
5. Restart the SSHD service:
```bash
sudo service ssh reload
```
6. Click on the 3 dots next to your instance name and then select _View network details_ option.

![View Network Details of a GCP instance](https://tlgur.com/d/g09Zyw34)

Now click on _default-allow-ssh_ in the list.

![selct default_allow_ssh option from the list](https://tlgur.com/d/8BQOw5d4)

Click on _Edit_ option.

![Click on Edit](https://tlgur.com/d/4AQXmkv4)

Look for _Protocols and ports_ heading and change it from

```
tcp: 22
```
to
```
tcp:22,443
```
Save the rule.

### Connect from you local machine
Now you should be able to ssh to your instance as root user by doing:
```bash
$ ssh -p 443 <your root username>@<public ip of your instance >
```

That's it. If you face any problems following the tutorial, comment down below and I might be able to help out.

Cheers!
