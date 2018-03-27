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
