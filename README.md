# NMAP-IPTABLES

# 1. NMAP:

Nmap (Network Mapper) is a security scanner used to discover hosts and services on a computer network, thus creating a map of the network. To accomplish its goal, Nmap sends specially crafted packets to the target host and then analyzes the responses.

* learn from this
* https://nmap.org/

The command in terminal is following:
* nmap -A (your ip_range)
* eg:
* nmap -A 10.10.111.0/24


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
