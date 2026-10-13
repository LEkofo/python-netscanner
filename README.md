# CyberPython: Network Scanner

A command-line network scanner written in Python. It discovers live hosts on a network, scans their TCP ports using multithreaded sockets, and can identify operating systems and MAC address vendors using Nmap.

> **Legal notice:** Only scan networks and devices that you own or have explicit written permission to test. Unauthorised scanning may be illegal and can breach network policies. This project was built for learning and for use in a personal home lab.

## Features

- **Host discovery:** ping scan of a single IP or a whole subnet to find live hosts  
- **Port scan:** finds live hosts first, then scans each one's TCP ports (1-5999) using up to 1,000 worker threads fed from a queue  
- **OS detection:** estimates the operating system of each host using Nmap's `-O` fingerprinting  
- **MAC address and vendor lookup:** shows the MAC address and manufacturer of devices on the local network  
- **Input validation:** rejects invalid IP addresses and subnets before any scan starts

## Requirements

- Python 3.8 or newer  
- [Nmap](https://nmap.org/download.html) installed on your machine (the Python library is only a wrapper and does not include Nmap itself)  
- The `python-nmap` library (listed in `requirements.txt`)

## Installation

### macOS

brew install nmap

git clone https\://github.com/LEkofo/python-netscanner.git

cd python-netscanner

pip3 install \-r requirements.txt

If you don't have Homebrew, see [brew.sh](https://brew.sh).

### Linux (Debian/Ubuntu/Kali)

sudo apt install nmap

git clone https\://github.com/LEkofo/python-netscanner.git

cd python-netscanner

pip3 install \-r requirements.txt

### Windows

Install Nmap from [nmap.org](https://nmap.org/download.html), then:

git clone https\://github.com/LEkofo/python-netscanner.git

cd python-netscanner

pip install \-r requirements.txt

### Optional: virtual environment

Keeps the library separate from the rest of your system, and helps if `sudo` can't find `python-nmap`:

python3 \-m venv venv

source venv/bin/activate

pip install \-r requirements.txt

## Usage

Run the program and choose an option from the menu:

python3 netscanner.py

(Use `python` instead of `python3` on Windows.)

1\. Host Discovery Scan

2\. Port Scan

3\. Operating System Scan

4\. MAC Address \+ Vendor

5\. Exit

You will then be asked for a target, which can be:

- a single IP address, e.g. `192.168.1.10`  
- a subnet in CIDR notation, e.g. `192.168.1.0/24`

### Example output

Enter scan type: 2

Enter target host/subnet: 192.168.1.0/24

\-------------------------------------------------------------------------

Scanning for ports on 192.168.1.1...

\[192.168.1.1\] : port 53 is open

\[192.168.1.1\] : port 80 is open

Scan for 192.168.1.1 is complete

## Notes

- **OS detection** (option 3\) needs administrator/root privileges. On macOS and Linux, run it with `sudo python3 netscanner.py`.  
- **MAC addresses** (option 4\) are only visible for devices on the same local network as the machine running the scan, and usually need administrator/root privileges.  
- Port scans run on every live host found, so scanning a large subnet can take a while.  
- Results are printed to the terminal and are not saved to a file yet.

## How it works

1. **Discovery:** Nmap performs a ping scan (`-sn`) and returns the list of live hosts.  
2. **Port scanning:** each port number is put on a `queue.Queue`. Worker threads take ports from the queue and try a TCP connection with a socket. A successful connection means the port is open.  
3. **OS and MAC lookup:** Nmap runs `-O` or `-sn` and the script reads the relevant fields from the results.

## Possible future improvements

- Command-line arguments (argparse) so scans can run without the menu and be scheduled  
- Save results as JSON with timestamps  
- Compare each scan with the previous one and flag new or closed ports  
- Banner grabbing on open ports to identify services  
- Optional GUI

## Licence

Released under the MIT Licence. See the [LICENSE](http://LICENSE) file for details.

## Author

Built by Leroy as a learning project in network programming and security tooling.