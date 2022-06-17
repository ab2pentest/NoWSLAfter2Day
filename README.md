# No WSL After Today !
No Windows System Linux After Today

# Description

> This document is talking about how to set a virtual linux system in your windows machine without using WSL.

# Final result

![2021-12-14_15-52-47](https://user-images.githubusercontent.com/84577967/174202000-65a8e1ad-b4fb-4127-9bc1-31a5ac17382f.png)

# Download

The used softwares and apps in this document can be found below:

[Windows Terminal Preview](https://www.microsoft.com/store/productId/9N8G5RFZ9XK3)

[VirtualBox](https://www.virtualbox.org/wiki/Downloads)

[Kali Linux](https://www.kali.org/get-kali/#kali-virtual-machines)

# Setting our virual machine using VirtualBox

After downloading and installing Windows Terminal & VirtualBox, let's extract the `Kali Linux` OVA Image file and import it to our `VirtualBox`, in this step you don't have to make any modification just leave everything as default settings.

![2021-12-14_16-49-33](https://user-images.githubusercontent.com/84577967/174202013-4f41c965-9263-4e49-92f5-1ceeb33dd627.png)

![2021-12-14_17-46-38](https://user-images.githubusercontent.com/84577967/174202021-91e05065-e104-4ba6-a7e1-434ac53d2cf3.png)

After the image was imported without erros, we will change some machine settings by right clicking on the machine and then going to `Settings` and follow the screenshots steps below

1) In the advanced tab let's choose Bidirectional for both of `Shared Clipboard` & `Drag'nDrop`

![2021-12-14_18-22-37](https://user-images.githubusercontent.com/84577967/174202035-d7f98079-abc6-4095-ba4d-bf54ae6df8c0.png)

2) For the system base memory and processor the best recommendation is 2 CPU's and 2 GB of RAM.

![2021-12-14_18-26-39](https://user-images.githubusercontent.com/84577967/174202045-238f8363-3fc8-45bc-bb99-f4b443592680.png)

![2021-12-14_18-26-52](https://user-images.githubusercontent.com/84577967/174202055-f62db662-793c-4f9f-82c3-7f54e9e2025b.png)

3) In the network, choose `NAT` - this is required.

![2021-12-14_18-28-52](https://user-images.githubusercontent.com/84577967/174202071-68be61ab-3bfa-4fa0-b4b7-76e6650c680d.png)

4) Share a folder from your windows to the linux machine, In my case I shared my win desktop and mounted to `/root/Desktop/Windows/`. Don't forget to check `Make Permanent` option.

![2021-12-14_18-30-38](https://user-images.githubusercontent.com/84577967/174202080-a14d2dcd-840b-422a-a007-b4dfc3912ce1.png)

After that you set everything click OK and Ð congrats! Your machine is ready.

Great ! Now let's run our virtual machine in normal mode ...

![2021-12-14_18-36-38](https://user-images.githubusercontent.com/84577967/174202095-9ec46f35-df40-4d40-ad91-9851bb80528e.png)

Default creds: kali/kali

![2021-12-14_18-40-09](https://user-images.githubusercontent.com/84577967/174202107-98a4c016-798c-4bd5-aa62-ae4caa6b145f.png)

We need this to check only if our VM is working fine and also to check the network settings.

**Note: Record your IP address - we will need it later.**

![2021-12-14_18-44-16](https://user-images.githubusercontent.com/84577967/174202154-04de206f-d53a-4c50-b269-305ca7962783.png)

Generate a new ssh key for any user, I selected to be root always so by going to terminal and type `sudo su -` then `ssh-keygen` and typing `Enter` few times to generate the key, the process will automatically save our private & public key in `/root/.ssh/id_rsa`

![2021-12-14_18-52-07](https://user-images.githubusercontent.com/84577967/174202168-c82c4034-53d5-4ecb-b17e-2ace5b0ffba8.png)

After that let's copy `id_rsa.pub` to `authorized_keys` by typing: `cp id_rsa.pub authorized_keys`

And then copy the private key `cat /root/.ssh/id_rsa`

![2021-12-14_18-51-41](https://user-images.githubusercontent.com/84577967/174202276-599a0069-1f97-4f76-999b-76360b4ff874.png)

And paste it in your Windows Desktop Directory

![2021-12-14_19-21-39](https://user-images.githubusercontent.com/84577967/174202256-6ad3889f-d284-4e1e-9dcd-329cc2db2dfd.png)

Awesome! Now let's add to our `C:\Windows\System32\drivers\etc\hosts` file this line

```
127.0.0.1 kali.box
```

Let's go back to `VirtualBox` and go to the settings of the Kali machine, and exactly to the network category and click on advanced -

![2021-12-14_19-03-25](https://user-images.githubusercontent.com/84577967/174202309-c0eb20bc-903b-4143-a35b-7bd78de778bf.png)

And then port forwarding - 

![2021-12-14_19-03-54](https://user-images.githubusercontent.com/84577967/174202327-ce8c8383-08af-42ec-8714-66ef55626892.png)

And follow the steps in this screenshot -

![2021-12-14_19-06-29](https://user-images.githubusercontent.com/84577967/174202336-8a50f67f-8f5f-4314-887d-18cd51d649a2.png)

In this rule we added a new value to allow our Windows machine to connect directly to the ssh port of the kali VM.

Now let's go back to the machine, and let's modify the sshd_config file to allow root logins `nano /etc/ssh/ssh_config`

![2021-12-14_19-45-27](https://user-images.githubusercontent.com/84577967/174202353-5fb724f9-d040-4eb3-9061-f99eba920dd3.png)

Let's change this line to this

```
PermitRootLogin yes
```

Then let's restart the ssh service and also enabling it in case wasn't enabled by default.

![2021-12-14_20-11-16](https://user-images.githubusercontent.com/84577967/174202372-b1a33e59-1a5f-4a03-9d6c-c0070c0a2084.png)

Commands: 

```bash
# To restart SSH
systemctl restart ssh

# To enable SSH
systemctl enable ssh
```

# Setting our Windows Terminal

Now after that everything is set in our Kali virtual machine ... Let's go to the Windows Terminal and create a new profile

![2021-12-14_20-25-07](https://user-images.githubusercontent.com/84577967/174202383-e622ecdb-825d-4473-8d99-809eda9a2021.png)

Now follow the steps shown in the screenshot below

![2021-12-14_20-26-49](https://user-images.githubusercontent.com/84577967/174202395-4d0a6555-ceaf-4c2e-afdc-795969f9095b.png)

The command line:
```
ssh.exe -i id_rsa root@kali.box
```

**Note: `Starting directory` must be where we have our SSH `id_rsa` private key.**

After you set everything as shown click on Save and then go to startup and choose your new profile as the Default profile . Don't forget to click on Save. 

![2021-12-14_20-32-52](https://user-images.githubusercontent.com/84577967/174202412-68c4f5c1-22f2-4b74-8197-9504a77e4586.png)

Now if you restart the Windows Terminal, it will run the automatically ssh command and connect you to your Kali VM.

![2021-12-14_20-35-28](https://user-images.githubusercontent.com/84577967/174202425-bde379e1-9612-414d-b987-bd6d55ed463f.png)

type `yes` and click Enter 

![2021-12-14_21-30-12](https://user-images.githubusercontent.com/84577967/174202433-d6da16a3-47c8-4e3e-a747-6f29cd7c873a.png)

To open the shell directly on our shared Desktop we can add in the end of the file `~/.zshrc` the mounted point (path) we set earlier and in our case:

```bash
cd /root/Desktop/Windows
# or
cd ~/Desktop/Windows
```

![2021-12-14_21-32-12](https://user-images.githubusercontent.com/84577967/174202457-19fcfafa-846b-4df9-a2c6-49096a4ce47b.png)

---

# Make it run at startup and headless

Go to `C:\Users\Username\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`, change the value Username with your computer username and create a new file let's name it `kali_vbox.bat`

`"C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" startvm "Kali-Linux-2021.4-vbox-amd64" --type headless`

First check if `VBoxManage.exe` is in the right path and the `"Kali-Linux-2021.4-vbox-amd64"` value we can get from the box Name in the settings.

![2021-12-14_21-43-12](https://user-images.githubusercontent.com/84577967/174202470-c8906b00-0fa4-4a71-9bd8-59544f21c584.png)

And congratulations! Now the linux vm machine will start in background and you can use everything on your windows desktop as you are working on linux.

![2021-12-14_21-37-02](https://user-images.githubusercontent.com/84577967/174202479-33a0edae-a420-4eb8-915d-bad3ce10dfc2.png)

--

# Additional Port Forwarding in my own configuration

![2021-12-14_19-08-49](https://user-images.githubusercontent.com/84577967/174202494-3a951b7c-ee8f-43f6-a307-f155058aac04.png)

The 1st IP is the IP of my THM VPN that I'm connecting to it using my Windows Machine, I use this rule in case I was playing a machine in THM or HTB (must update the IP each time in case you are not using a VIP membership)

The 2nd IP is my lan network address.

---

# Thanks

In the end I would like to thank `leeloo1313` for encouraging and helping to write this nosense setup :D

# Coming
* An auto setup for the whole process.
* Setting an android emulator on windows and bypassing SSL pinning, and more ...

---

If you liked this content and want to encourage me for more, you can [buymeacoffee](https://www.buymeacoffee.com/ab2pentest).
