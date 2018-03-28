# NMAP-IPTABLES

# 1. NMAP:

Nmap (Network Mapper) is a security scanner used to discover hosts and services on a computer network, thus creating a map of the network. To accomplish its goal, Nmap sends specially crafted packets to the target host and then analyzes the responses.

* learn from this
* https://nmap.org/

The command in terminal is following:
* nmap -A (your ip_range)
* eg:
* nmap -A 10.10.111.0/24


Explaination of common command:
. a) -n
. Never do DNS resolution/Always resolve

. b) -PO
 This allows you to turn off IP protocol Ping
 
 c) -O
Enable OS detection

d) -v
. Increase verbosity level

e) -oN  
. Output scan in normal to the given filename

f) -sS
do TCP SYN scan


# 2. IPTABLES:

iptables is a user-space application program that allows a system administrator to configure the tables provided by the Linux kernel firewall (implemented as different Netfilter modules) and the chains and rules it stores.

* learn from this
* https://wiki.archlinux.org/index.php/Iptables
* https://wiki.archlinux.org/index.php/Simple_stateful_firewall

Here is my command example For incoming traffic
//outgoing traffic should no restrict
* 1) The internal machine (10.20.111.2) should respond to a ping from 10.10.111.0/24 
* 2) The internal machine (10.20.111.2) should block all incoming SSH and http requests from 10.10.111.0/24
```
iptables -I INPUT -s 10.10.111.0/24 -p tcp --dport 22 -j DROP
iptables -I INPUT -s 10.10.111.0/24 -p tcp --dport 80 -j DROP
```

The next rule will accept all new incoming ICMP echo requests, also known as pings. Only the first packet will count as NEW, the others will be handled by the RELATED, ESTABLISHED rule. Since the computer is not a router, no other ICMP traffic with state NEW needs to be allowed.
```
# iptables -I INPUT -s 10.10.111.0/24 -p icmp --icmp-type 8 -m conntrack --ctstate NEW -j ACCEPT
```
```
iptables-save     //to save and see new rules

iptables -nvL --line-numbers
```

# Some sample of rules set:
```
iptables 限制ip访问
通过iptables限制9889端口的访问（只允许192.168.1.201、192.168.1.202、192.168.1.203）,其他ip都禁止访问
iptables -I INPUT -p tcp --dport 9889 -j DROP
iptables -I INPUT -s 192.168.1.201 -p tcp --dport 9889 -j ACCEPT
iptables -I INPUT -s 192.168.1.202 -p tcp --dport 9889 -j ACCEPT
iptables -I INPUT -s 192.168.1.203 -p tcp --dport 9889 -j ACCEPT



单个IP的命令是
iptables -I INPUT -s 59.151.119.180 -j DROP

封IP段的命令是
iptables -I INPUT -s 211.1.0.0/16 -j DROP
iptables -I INPUT -s 211.2.0.0/16 -j DROP
iptables -I INPUT -s 211.3.0.0/16 -j DROP

封整个段的命令是
iptables -I INPUT -s 211.0.0.0/8 -j DROP

封几个段的命令是
iptables -I INPUT -s 61.37.80.0/24 -j DROP
iptables -I INPUT -s 61.37.81.0/24 -j DROP
```

restriction on TCP packets (you can use tcp-flags to specific certain bits//SYN,FIN...)
```
iptables -I INPUT -s 10.10.111.0/24 -p tcp --dport 443 -j DROP
iptables -I INPUT -s 10.10.111.0/24 -p icmp --icmp-type 8 -m conntrack --ctstate NEW -j DROP
//icmp deny, which means deny ping 

iptables -A INPUT -i eth0 -p tcp --syn -j DROP 

iptables -A INPUT -i eth0 -p tcp --tcp-flags ALL NONE -j DROP 
# iptables -A INPUT -i eth0 -p tcp --tcp-flags SYN,RST SYN,RST -j DROP 
```
