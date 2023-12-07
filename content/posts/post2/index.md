---
title: "Lab 1: Nmap Port Scanning"
date: 2023-12-06T09:03:20-08:00
draft: false
tags: cybersecurity, networks, labs, nmap

---

Nmap, or **N**etwork **map**per, is an open-source Linux tool for network discovery and network security auditing. 

Nmap is a fantastic tool for network scanning, host discovery, and port scanning. It works mainly by sending packets and analyzing the responses, but has other functions as well. 

By reviewing and auditing the ports on machines in the network regularly, we can verify that they are configured correctly, secure, and being used in the intended manner. It can also reveal information about possible vulnerabilities, or detect irregularities in the network hosts. 

**How to get Nmap** 

Nmap comes installed on KaliLinux, and can be installed on many Linux distributions with various package managers. Click [Here](https://nmap.org/download) to download nmap for your system. 


### Environment Setup

For this lab, we'll be using a Virtualbox to set up a NAT Network which will contain three hosts: our host system, a KaliLinux instance called Kali2 and a Metasploitable 2 instance called Target. 

| Name | Type | IP Address |
| -------- | -------- | --------- |
| Host PC  | n/a | 10.0.2.1 |
| Kali2 | Kali Linux | 10.0.2.4 |
| Target | Metasploitable 2 | 10.0.2.5 |


First, let's verify the version and see the usage instructions using the commands below.

```
$ nmap --version
$ nmap -h 
```

![Output of Version Check Scan](/lab1/nmap--v-h.PNG)

## Common Nmap Scans and Commands

Here are some common types of nmap scans and what they tell us about the target machine. 

### No Port Scan

Also known as a ping scan, this type of scan is used to perform host discovery without doing a port scan. This type of scan tells us how many online and responsive hosts there are on a netowork. 

``````
$ nmap -sp 10.0.2.0/24
``````
Below is the output of the No Port Scan run on the whole subnet. This shows there are three online hosts in the network, and their ip addresses. 

![Output of No Port Scan](/lab1/nmap-sP.PNG)


### Basic Port scan

This is the most basic usage of nmap, and it scans the specified host(s) for the 1000 most well-known ports, like HTTTPS, SSL, SQL and others.

``````
$ nmap 10.0.2.5
``````

![Output of Basic Port Scan](/lab1/nmap.PNG)

<!--- Decided not to inlclude 
**TCP Connect Scan** 

To detect active services on TCP ports, we can perform a TCP Connect scan.  

````
sudo nmap -sT TARGET-HOST
````

**UDP scan**

This type of scan uses UDP pakets to scan ports like DNS, SNMP, and DHCP. UDP Scanning are generally slow, and even with packet-limiting to one packet per second results in an 18+ hour scan. 

``````
$ sudo nmap -sU TARGET-HOST
``````
--->

### Stealth Scan

A TCP SYN scan, also commonly called a stealth scan, uses the flag -sS. This type of scan initiates by sending a TCP SYN connect request, and if the target responds with a SYN ACK, it doesn't respond with an ACK but sends a RST. As it doesn't complete the ACK handshake, it is less likely to be detected by the target machine. 

````
$ sudo nmap -sS 10.0.2.5
````

![Output of Stealth Scan](/lab1/nmap-sS.PNG)

### Aggresive Scan 

By including the -A flag, this command runs every kind of scan aganist the target host, including OS detection, version detection, script scanning, and traceroute. This type of scan will generally take a very long time. 

``````
$ sudo nmap -A 10.0.2.5
``````

Since the output of this scan is quite long, the first part of the output in shown below. 

![Output Part of Aggressive Scan](/lab1/nmap-A-1.PNG)

### Version Detection

The -sV flag will allow us to collect information on the version of the service running on an open port. This can be good to know, as older versions can contain unpatched backdoors or vulnerabilities.

````
$ sudo nmap -sV 10.0.2.5
````

![Output of Version Detection Scan](/lab1/nmap-sV.PNG)

### OS Detection Scan

By using TCP/IP fingerprinting, Nmap can provide information onwhat oeprating system the target host is possibly using. It will also show a confidence percentage for each guess. In order to make this faster, we can use the option --osscan-limit to limit detection to only hosts woth at least one open and one closed TCP port and ignore all other hosts.  

````
$ sudo nmap -O 10.0.2.5
````

![Output of OS Detection Scan](/lab1/nmap-O.PNG)

### Verbose Output

By including a -v flag, the scan will output information on the scan's current actions. This option is useful for montitoring a scan, as it gives insight into the specific actions being run. 

```
$ nmap -v -sV 10.0.2.5
```

Here is an example of what the terminal looks like when this output option is enabled: 

![Output of Verbose Output](/lab1/nmap-v.PNG)

### Scan from File

If there are a lot of given addresses to be scanned, a file can be imported using the -iL flag. Other commands and flags can also be added to specify the type of scan to run. 

``````
$ nmap -iL /ips.txt
``````

Here, we are passing in a file `ips.txt` which contains one line with the IP Address of the Target Host: 

![Output of Scan from File Option](/lab1/nmap-iL.PNG)

## Other Common Nmap Uses  

``````
$ nmap 192.168.1.1-24 # scan a range of IP Addresses

$ nmap -p 21 192.168.0.1 # scan for infomation on a specific port 

$ nmap -p 69-443 TARGET-HOST # scan a range of ports 

$ nmap -6 ::ffff:c0a8:1 # scan an IPv6 Address 


``````

## Ethics Considerations

When utilizing powerful tools like Nmap for network scanning, it is important to adhere to ethical practices.  

Always obtain explicit authorization and permission before conducting any network scanning activities. Unauthorized testing can be seen as aggressive and can possibly designated as an attack. Ensure you have written consent from the system owner or network administrator before using Nmap. 

For learning purposes, it is recommended to stick to your home network, virtual network environments, and safe hosts like scanme.nmap.org. 


### Sources 

https://nmap.org/book/man.html