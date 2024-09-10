---
title: 'Meow - HTB'
description: "In this walkthrough we will learn how to connect to a misconfigured Telnet after a port scan with Nmap using brute force through weak credentials, thus obtaining the flag."
pubDate: 'Sep 3 2024'
categories: ['WriteUp', 'HackTheBox', 'CTF']
--- 

## Introduction

In this walkthrough, we will use Nmap to scan ports to identify open ports and services, focusing on Telnet on port 23. We will enumerate, perform brute force attacks, and unload the flag.


```
Platform: Hack The Box
Level: Very Easy
```

![Meow pwned yw4rf](https://yw4rf.vercel.app/_astro/meow-6.BpHm5Bkn_2aKIJ.webp)


## Enumeration
```
target: 10.129.48.113  
```
<br>

Initially, we use the `Ping` command. This utilizes the **ICMP (Internet Control Message Protocol)**. Specifically, `Ping` sends an ICMP "echo request" message to an `IP address` and expects to receive an "echo reply" message in response. This process allows us to verify if a machine on the network is accessible and measure the time it takes to receive a response (known as latency).

`ping -c 1 10.129.48.113`

![Ping command](https://yw4rf.vercel.app/_astro/meow-1.DiLmyByw_ZauPr8.webp)

Since the packet was received from the target computer, we can confirm that it is operational.

Next, an Nmap (Network Mapper) scan is performed to enumerate all open TCP ports on the target machine in detail.

![Nmap command](https://yw4rf.vercel.app/_astro/meow-2.BrnQL7Ta_Z23Tv0v.webp)

`sudo nmap -p- --open -sV --min-rate 5000 -n -Pn -vvv 10.129.48.113 -oG meow-scan`

```
- Nmap: Network scanning tool.

- -p-: Instructs Nmap to scan all available ports, from 1 to 65535.

- --open: Filters Nmap results to show only open ports.

- -sV: Enables service detection. Nmap will attempt to identify the versions running on the open ports.

- --min-rate 5000: Sets a minimum rate of 5000 packets per second to speed up the scan.

- -n: Avoids DNS resolution. Nmap will not try to convert IP addresses into domain names, which can make the scan faster.

- -Pn: Disables the initial host discovery ("ping scan") and treats all hosts as if they are active.

- 10.129.48.113: Specifies the IP address of the target to scan.

- -Og meow-scan: Indicates that the scan results should be saved in a "grepable" format (easy to filter or search with commands like grep). The resulting file will be named meow-scan.
```
<br>

We can see that it found the `port 23/tcp` is open, indicating that the service is `Telnet` with the version `Linux telnetd`.

`PORT   STATE  SERVICE REASON           VERSION 23/tcp open   telnet? syn-ack ttl 63   Linux telnetd`

## Telnet

With a quick search on `Google`, we can see that `Telnet` is a network protocol that allows us to access another machine to remotely manage it as if we were sitting in front of it. It generally uses port 23 and the TCP protocol for data transmission.

One of Telnet's most significant disadvantages is that it does not encrypt transmitted data, including credentials (username and password), making communications vulnerable to "man-in-the-middle" attacks and other forms of eavesdropping.

Using the `Telnet` command and specifying the target `IP Address`, we can log into the machine and perform a brute force attack.

![Brute force attack telnet](https://yw4rf.vercel.app/_astro/meow-3.B7VrxIpI_241Kx2.webp)

After trying several login usernames, such as `admin`, `administrator`, we were granted access without a password using the login name `root`.

`Meow login: root`

![Root telnet](https://yw4rf.vercel.app/_astro/meow-4.CKPXuNlI_Z23PiPz.webp)

Once we have gained root access on the target machine, we have full control of it.

With the `ls` command, we check the contents of the current directory, where we see the `flag.txt` file, which is the final step to complete the machine. Using the `cat flag.txt` command, we view the contents of the file and obtain the flag.

![Flag captured meow machine](https://yw4rf.vercel.app/_astro/meow-5.3Fo_wjEw_UlXa0.webp)

<br>

Once we have the flag captured, **we have completed the Fawn machine**.
![Meow pwnd yw4rf](https://yw4rf.vercel.app/_astro/meow-7.B07hYUuM_2fgeRo.webp)


<br>