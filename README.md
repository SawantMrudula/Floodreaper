# Floodreaper
FloodReaper is a comprehensive and powerful tool designed to simulate a variety of flooding attacks against websites and domains. It offers an advanced command-line interface (CLI) and a suite of features tailored for stress testing and educational purposes.

FloodReaper

FloodReaper is a comprehensive and powerful network stress-testing and educational tool designed to simulate multiple flooding attack techniques against websites and servers in controlled environments. It provides a command-line interface (CLI) with integrated utilities for packet crafting, multithreading, traffic generation, and IP hopping for research and cybersecurity learning purposes.

Table of Contents
Introduction
Features
Installation
Initial Setup and Configuration
Setting the Target
Dashboard Overview
Inbuilt IP Hopping Feature
Attack Modules
SYN Flood Attack
HTTP Flood Attack
Slowloris Attack
IP Fragmentation Attack
DNS Amplification Attack
Code Breakdown
Importing Libraries
Global Variables
Initialization System Checks
Function Overview
Attack Functionality
Contributions
Disclaimer
Introduction

FloodReaper demonstrates how different denial-of-service techniques function in networking environments. The project was built for:

Educational demonstrations
Cybersecurity research
Stress testing in authorized lab environments
Understanding offensive network strategies
Learning packet crafting and network behavior

The tool integrates multiple attack simulation techniques into a single CLI-based platform.

Features
Multi-Type Attack Modules
SYN Flood

Simulates TCP SYN flooding by exploiting the TCP three-way handshake process to overwhelm server resources with half-open connections.

HTTP Flood

Generates large volumes of HTTP GET requests to exhaust web server resources.

Slowloris

Maintains multiple incomplete HTTP connections to consume available sockets on the target server.

IP Fragmentation

Sends fragmented IP packets to disrupt the target’s packet reassembly process.

DNS Amplification

Uses DNS resolver amplification techniques to increase traffic volume directed toward a target.

Advanced CLI Interface
Interactive command-line dashboard
User-friendly navigation
Configurable attack parameters
Real-time target locking system
Automated System Checks

FloodReaper automatically verifies:

Operating system compatibility
Root privileges
Required dependencies
External networking tools
Python libraries
IP Hopping Integration

FloodReaper supports IP rotation using:

Tor Service
Tornet Integration

This helps anonymize outbound traffic during controlled testing.

Multithreading Support

The tool utilizes Python threading for:

High-performance request generation
Parallel attack execution
Improved stress-testing capability
Installation
1. Clone the Repository
git clone https://github.com/TushN101/FloodReaper.git
2. Navigate to the Project Directory
cd FloodReaper
3. Install Required Libraries
pip install scapy
4. Run the Tool
python main.py
Initial Setup and Configuration

Before execution, FloodReaper performs multiple automated checks.

Operating System Check

Confirms the script is running on a Linux/POSIX environment.

Root Privilege Verification

Checks whether the program is executed with sudo/root permissions.

Dependency Validation

Verifies the availability of:

Scapy
dig
hping3
Tor
Tornet
Slowloris
Automatic Installation Support

Missing dependencies may be installed automatically where supported.

Setting the Target

FloodReaper supports both:

Direct IP addresses
Website URLs
IP Address Input

If an IP is provided, the tool immediately locks onto the target.

URL Resolution

When a URL is entered:

The domain is extracted
dig +short resolves the domain to an IP
The resolved IP becomes the locked target
Error Handling

If resolution fails:

The user is prompted to manually provide the target IP
The script continues safely without crashing
Dashboard Overview

The interactive dashboard provides:

Target Display

Shows the currently locked target.

Attack Options

Available attack modules include:

SYN Flood
HTTP GET Flood
Slowloris
IP Fragmentation
DNS Amplification
Tor Service Integration

Users can:

Start Tor service
Enable IP hopping
Launch Tornet automatically
Safe Exit

The exit functionality:

Terminates Tor processes
Stops Tornet services
Prevents leftover background processes
Inbuilt IP Hopping Feature

FloodReaper includes integrated IP rotation functionality.

Activation Steps
Open Dashboard
Select:
Start Tor Service [IP HOPPING]
Tor service initializes
Tornet begins rotating IP addresses every 5 seconds
Purpose
Enhances anonymity
Reduces traceability
Helps bypass basic IP filtering during testing
Attack Modules
SYN Flood Attack
What is a SYN Flood?

A SYN Flood attack abuses the TCP handshake mechanism by sending massive numbers of SYN packets without completing the connection.

This causes:

Half-open connections
Resource exhaustion
Denial of legitimate access
Implementation

FloodReaper uses:

hping3

Features include:

Randomized source IPs
Continuous packet flooding
Custom window/data size configuration
HTTP Flood Attack
What is an HTTP Flood?

An HTTP Flood overwhelms web servers with excessive GET/POST requests.

Implementation

The module:

Uses Python sockets
Generates randomized URL paths
Sends concurrent GET requests using threads

Example request format:

GET /random-path HTTP/1.1
Host: target-domain
Connection: close
Slowloris Attack
What is Slowloris?

Slowloris keeps connections open by continuously sending partial HTTP headers.

This consumes server socket resources until legitimate users cannot connect.

Implementation

FloodReaper integrates the external:

slowloris

tool to automate the attack.

IP Fragmentation Attack
What is an IP Fragmentation Attack?

This attack overwhelms systems with fragmented packets, complicating packet reassembly.

Implementation

FloodReaper:

Generates fragmented packets
Uses randomized source IPs
Sends high volumes of fragmented payloads

Scapy is used for packet crafting.

DNS Amplification Attack
What is DNS Amplification?

DNS amplification leverages open DNS resolvers to generate amplified traffic toward a target.

Implementation

The module:

Sends crafted DNS requests
Spoofs the target IP as the source
Uses public DNS resolvers
Amplifies traffic volume significantly
Code Breakdown
Importing Libraries
import os, subprocess, sys
import urllib.parse, ipaddress
import socket, string, time
from random import randint, choice, random
import threading
from scapy.all import IP, ICMP, send, conf, UDP, DNS, DNSQR
Library Purpose
Library	Purpose
os/subprocess/sys	System interaction
urllib.parse	URL parsing
ipaddress	IP handling
socket	Network communication
threading	Concurrent execution
scapy	Packet crafting
Global Variables
is_dig_avail = False
domain = ''
target_ip = '0.0.0.0'
is_ip_hopping = False
conf.verb = 0

These variables manage:

Target tracking
Domain handling
Tool states
Packet verbosity
Initialization System Checks

FloodReaper verifies:

if os.name == "posix"

Checks for Linux/POSIX systems.

if os.geteuid() == 0

Confirms root access.

External Tool Validation
dig
subprocess.run(['dig', '-v'])
hping3
subprocess.run(['hping3', '--version'])
tornet
subprocess.run(['pip', 'show', 'tornet'])
tor
subprocess.run(['tor', '--version'])
slowloris
subprocess.run(['slowloris', '-h'])
Function Overview
resolve_target_dig()
Resolves URLs into IP addresses
Uses dig +short
Automatically locks the target

Fallback:

Calls resolve_target_ip() on failure
resolve_target_ip()

Fallback function for manual IP entry.

Used when automatic resolution fails.

start_tor()

Responsible for:

Starting Tor service
Enabling IP hopping
Initializing Tornet
exit()

Safely terminates:

Tor services
Tornet processes
Background tasks
Attack Functionality
syn_flood()

Uses:

hping3

Features:

SYN packet generation
Randomized sources
Continuous flooding
http_flood()
Generates randomized URLs
Sends GET requests using sockets
Uses multithreading
ip_frag()
Creates fragmented IP packets
Randomizes source IPs
Sends fragments concurrently
slowloris_attack()
Opens multiple partial HTTP sessions
Keeps connections alive
Exhausts server sockets
dns_amp()
Sends spoofed DNS queries
Uses public resolvers
Amplifies traffic volume
Contributions

Contributions are welcome.

You can contribute by:

Improving attack simulation modules
Optimizing threading
Adding monitoring features
Improving dashboard functionality
Enhancing documentation

Fork the repository and submit a pull request.

Disclaimer

FloodReaper is intended strictly for:

Educational purposes
Authorized stress testing
Cybersecurity research
Lab simulations

Unauthorized use of this tool against systems without explicit permission may violate laws and regulations. The developer assumes no responsibility for misuse or damages caused by this software.
