# ICA: 1

??? info "Description"
    **According to information from our intelligence network, ICA is working on a secret project. We need to find out what the project is. Once you have the access information, send them to us. We will place a backdoor to access the system later. You just focus on what the project is. You will probably have to go through several layers of security. The Agency has full confidence that you will successfully complete this mission. Good Luck, Agent!**

File Download: https://download.vulnhub.com/ica/ica1.zip

## Solution

For this challenge a friend of mine called AB2 guided me. I learn a lot from this challenge through him. So I started the machine and it gave me the IP. So I opened it in my broswer and was welcomed with this screen:

![Login page](../assets/files/vulnhub/ica1/login_page.png)

So I searched for a exploi for `qdPM 9.2` on searchsploit and got this:
![Exploitation for qdPM in searchsploit](../assets/files/vulnhub/ica1/searchsploit.png)
![Exploitation for qdPM in searchsploit](../assets/files/vulnhub/ica1/searchsploit2.png)
![Exploitation for qdPM in searchsploit](../assets/files/vulnhub/ica1/searchsploit3.png)

So we went to the `/core/config/databases.yml` and got this:
![databses.yml](../assets/files/vulnhub/ica1/databases.png)

So as you can see we get the username and password as `qdpmadmin` and `UcVQCMQk2STVeS6J` respectively! After this leap, we played around it a bit but got nothing. So we tried nmap scan on this ip and got a mysql sevrer's port

![Nmap scan results](../assets/files/vulnhub/ica1/nmap.png)
![Nmap scan results](../assets/files/vulnhub/ica1/nmap2.png)

We entered to the mysql server using `mysql` command using the username and password from the `databases.yml` file.

![Connecting to mysql server](../assets/files/vulnhub/ica1/mysql.png)
![Connecting to mysql server](../assets/files/vulnhub/ica1/mysql2.png)

We found some tables there and checked some of them like `staff` and the `qdpm` table. Now it's stealing time! let's steal some informations from these tables. Let'f first steal credentials from the `staff` table.

![Staff table](../assets/files/vulnhub/ica1/mysql3.png)

So the passwords are base64 encoded and when we decode those we get:
```
suRJAdGwLp8dy3rF
7ZwV4qtg42cmUXGX
X7MQkP3W29fewHdC
DJceVy98W28Y7wLg
cqNnBWCByS2DuJSy
```

And when we leak the `configuration` table in the `qdpm` database, we get the email and the password hash for the login page.

![qdpm database](../assets/files/vulnhub/ica1/mysql4.png)
![configuration table](../assets/files/vulnhub/ica1/mysql5.png)


We tried to crack the password using hashcat with the phpass mode(as the hash type is phpass), but it didn't work. After this me and my friend played along the web interface for some time but go nothing. Then we tried to change the value of the `app_administrator_password` field and it worked! So I generated a phpass hash for `123456` and changed `app_administrator_password` to that hash.

![Phpass hash for 123456](../assets/files/vulnhub/ica1/phpass.png)
![Updated password in the qdpm databse](../assets/files/vulnhub/ica1/mysql_pwd_updated.png)

And Voila! when we try to login using the email `admin@localhost.com` and the password `123456`, we get access to a dashboard like this:

![Dashboard](../assets/files/vulnhub/ica1/dashboard.png)

Now, we can create a user with admin privileges. Then logout and when we login as this new user, we get another dashboard with a configuration button looking like this:

![New Project](../assets/files/vulnhub/ica1/new project.png)

We can give it any name and then let's attach the [pentest monkey's php reverse shell](https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php) to it. Now when we go to `/uploads/attachments` we can see the reverse shell script. So we can use this to get a reverse shell with the username as `www-data` like this:

![Reverse shell](../assets/files/vulnhub/ica1/rev_shell.png)

Now when we check `/home/` we can see 2 users called `dexter` & `travis`. Do you remember these guys? these guys were there in the `staff` table in the mysql server. So let's try to login as dexter using ssh using the decoded passwords we got from the `staff` database. When we try the 2nd password in that list which is `7ZwV4qtg42cmUXGX` we get access to Dexter's home directory.

![SSH into Dexter's home directory](../assets/files/vulnhub/ica1/dexters_pc.png)

And there was a file called `note.txt` in his home directory. This was the content of the file:

```
It seems to me that there is a weakness while accessing the system.
As far as I know, the contents of executable files are partially viewable.
I need to find out if there is a vulnerability or not.
```

We didn't know what this meant and we continued to mess with the machine. Then AB2 found a executable file called `get_access` with setuid capability in the `/opt/` directory.

![get_access file](../assets/files/vulnhub/ica1/get_access.png)

So then he tried `strings` on the file and found a suspicious `cat` in the output

![strings ran on get_access](../assets/files/vulnhub/ica1/cat_vuln.png)

We immediately knew what we needed to attack. The exploit here is that since the program doesn't call `cat` using the absolute path, we can make our own script and call it `cat` and save it in `/tmp/` and then change the `PATH` variable to include the `/tmp` before every other paths. So here is what we did:

```bash
dexter@debian:/opt$ echo '/bin/bash' > /tmp/cat
dexter@debian:/opt$ chmod +x /tmp/cat
dexter@debian:/opt$ export PATH=/tmp:$PATH
dexter@debian:/opt$ ./get_access
root@debian:/opt# id
uid=0(root) gid=0(root) groups=0(root),1001(dexter)
root@debian:/opt# whoami
root
```

and boom we are now the root user! and when we go to `/root` we can see there is a file called `root.txt` there. So we can get the contents of the `root.txt` by using something like `strings`. If you are wondering why I didn'tuse the `cat` command here, it's because that we had changed the `cat` command before to execute `/bin/bash`

```bash
root@debian:/root# strings root.txt
ICA{Next_Generation_Self_Renewable_Genetics}
```
