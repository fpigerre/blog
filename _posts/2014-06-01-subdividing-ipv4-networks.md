---
author: psgs
layout: post
excerpt: >
  Using block-chains to transfer data securely and anonymously is quite an interesting idea.
  Data could easily be traded, given, bought and sold anonymously, without others gaining access to the data.
  A data block-chain system could even hook into the BitCoin protocol and execute
  BitCoin transactions directly when transacting data for currency or vice versa.
categories:
  - networking
tags:
  - ip
  - ipv4
  - ipv6
  - subnet
  - subnetting
  - network
  - networks
  - networking
reading-time: 10 minutes
title: Sub-Dividing IPv4 Networks
---

Over the past few weeks, as part of the Cisco CCNA (Cisco Certified Network Associate) curriculum, i've been studying how to set-up and correctly address networks using IP (Internet Protocol) addresses.
At the moment, two types of IP addresses are in use. IPv4 and IPv6. Most networks and devices at the moment are addressed with IPv4 addresses. These addresses are most often represented as a set of numerical digits, delimited by dot points, for example ```192.168.0.1```.
It's quite likely that you may have seen similar addresses floating around your home; every device connected to the mainstream internet must have access to an IP address in order to send and receive traffic. 
IP addresses are usually allocated by the Internet Assigned Numbers Authority ([IANA](https://www.iana.org/)) and other organisations to Internet Service Providers (ISPs) for distribution, however, the amount of IPv4 addresses available for use have rapidly been decreasing, to the point that the Asia-Pacific Network Information Centre (APNIC) has run out of addresses to assign.
This depletion of addresses has prompted the deployment of IPv6, a newer version of the IP protocol, that implements addresses over four times larger than IPv4 addresses. IPv4 however, is still the predominant standard used when addressing networked devices.

In order to address a large number of networked devices while only possessing a small block of allocated IP addresses, a process called *sub-netting* can be used. "Sub-netting an IP address" is essentially an abbreviated way of saying "Sub-dividing an IPv4 address".

> Sub-netting an IPv4 address is the process of breaking down a 'chunk' of IPv4 addresses into a larger amount of smaller "chunks", through the use of a *subnet mask*.

When using IPv6, sub-netting isn't required, simply because of the gigantic amount of IPv6 addresses available for use[^1].
You may have noticed when dealing with IPv4 addresses, for example, when configuring a shiny new home router or examining your computer's IP configuration, that IPv4 addresses are often coupled with "subnet masks". Subnet masks most often look similar to the following: ```255.255.255.0```.
Subnet masks are used to determine which particular network or sub-network an IP address resides in. This is done by using the subnet mask to separate "host bits" from "network bits" in an IP address.

Before embarking on the process of sub-netting, it is important to understand the "classfull" nature of IPv4 addresses and specific properties of these addresses.
Displayed here are some interesting facts that should be of use when sub-dividing an address.

* One portion (also known as an octet) of an address, which equates to eight binary bits of an address can only add up to a maximum decimal value of 255.
* In decimal notation, portions are delimited by dots.
* An IPv4 address can have a maximum of four portions. This also applies to subnet masks.
* IP addresses are actually addresses written in binary digits (bits). Decimal notation is used when displaying IP addresses in order to make them easier to read.

IPv4 addresses are divided into classes. These classes are labelled using alphabetical letters. At the moment, IP addresses can be categorised into classes A, B, C, D and E.
*Classes D and E are used for multi-cast and experimental purposes, and therefore aren't used when addressing common networked devices.*
Any IP address whose first octet lies in the range of 1-127 can be categorised into category A. From there, each category only accepts a certain window of first-octet values, each window decreasing by a certain multiple of eight, as the class changes.
This information is summarised in the following table:


Class | Range
------|--------
A | 1-127
B | 128-191
C | 192-223
D | 224-239
E | 240-255

Classes A to C (publicly used address classes) each utilise a default subnet mask. This subnet mask is changed during the process of sub-netting, in order to allow host bits to become network bits and vice versa.

Class | Subnet Mask
------|--------------
A | 255.0.0.0
B | 255.255.0.0
C | 255.255.255.0

Any bits in an IP address that correspond to a non-null value in a subnet mask are regarded as network bits. For example, in the following class A address ```12.182.43.6```, with the following subnet mask ```255.0.0.0```, the value of 12 (in binary digits) would be regarded as network bits (bits indicating the network on which the IP address would reside).
If the subnet mask was 255.252.0.0, the end result would change however. 
*It is important to note that a subnet mask can only comprise of continuous values; for example, a subnet mask of ```255.255.234.0``` or ```255.255.255.167``` would be valid, however, subnet masks of ```255.0.255.0``` or ```255.183.255.0``` wouldn't.*

---

When sub-netting IPv4 addresses, it is understood that a network address is divided into several different network addresses, each with different subnet masks.
Addresses are usually subdivided in order to address a large number of devices with unique IP addresses.

### Example One

An address ```192.168.1.0``` with a subnet mask of ```255.255.255.0``` has been allocated to a business by an ISP.
The first address ```192.168.1.0``` is the address of the network, and therefore can't be used by other hosts (devices). The last address ```192.168.1.255``` can't be allocated to hosts, as it's used to broadcast messages to the hosts within the network. This leaves us with 253 usable addresses that can be allocated to devices such as computers, printers and servers on the network.
600 hosts, however, are required by the business in question. In order to allow every host to communicate on the network, the main network address ```192.168.1.0``` would be divided into several "sub networks".
This is where we start delving into the world of binary numbers.

An IP address in binary digits (bits) is made up of four octets, each comprising eight digits. Four octets comprising entirely of zeros ```0.0.0.0``` would be represented in binary as ```00000000.00000000.00000000.00000000```.
An IP address of ```255.255.255.255``` would be represented as ```11111111.11111111.11111111.11111111```.

Each binary digit in the address represents a certain value equivalent to a multiple of eight, in order to create any number between ```0``` and ```131,255```.
These values are represented as follows:

*Binary Digit* | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | . | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | . | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | . | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
*Value*  |                                                                                                             | . | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

A last octet of ```224``` would be represented as ```11100000```.

In order to subdivide an address into multiple networks, each comprising a maximum number of available addresses, the amount of bits needed to create the sum of required hosts is calculated. This is done by adding up the values of the *empty bits* in the subnet mask, *from left to right* until the closest value to the number of required hosts is reached.
When adding up the binary value of a subnet mask, however, instead of the first bit being equal to a value of ```1```, the first bit is equal to a value of ```2```.

In this case, to reach 600 available hosts, the empty bits of the subnet mask, being the ```0``` in ```255.255.255.0```, would be 

[^1]: [Tongue twister: The number of possible IPv6 addresses read out loud](http://royal.pingdom.com/2009/05/26/the-number-of-possible-ipv6-addresses-read-out-loud/)