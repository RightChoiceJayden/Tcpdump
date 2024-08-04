# Tcpdump 

<h2>Scenario</h2> 
You’re a network analyst who needs to use tcpdump to capture and analyze live network traffic from a Linux virtual machine.

The lab starts with your user account, called analyst, already logged in to a Linux terminal.

Your Linux user's home directory contains a sample packet capture file that you will use at the end of the lab to answer a few questions about the network traffic that it contains.

Here’s how you’ll do this: First, you’ll identify network interfaces to capture network packet data. Second, you’ll use tcpdump to filter live network traffic. Third, you’ll capture network traffic using tcpdump. Finally, you’ll filter the captured packet data.


<h2>Solutions</h2>
1. Identify Network Interfaces.
   
* Use `ifconfig` to identify the interfaces that are available:
  
![Screenshot 2024-08-03 at 10 35 59 PM](https://github.com/user-attachments/assets/a6c32369-33f6-43ca-822e-151c29a5e057)

* Identify the interface options available for packet capture:

![Screenshot 2024-08-03 at 10 36 05 PM](https://github.com/user-attachments/assets/d46347d1-ace1-432e-9d6e-9c923822d5f2)

2. Inspect the network traffic of a network interface with tcpdump.

* Use `sudo tcpdump -i eth0 -v -c5` to filter live network packet data:
  
![Screenshot 2024-08-03 at 10 36 14 PM](https://github.com/user-attachments/assets/2520b3b1-a733-4e48-a927-f941e62e261d)

> `-i eth0`: Capture data specifically drom the `eth0` interface.

> `-v`: Display detailed packet data.

> `-c5`: Capture 5 packets of data.

3. Capture network traffic:

* Capture packet data into a file named called `capture.pcap`:  `sudo tcpdump -i eth0 -nn -c9 port 80 -w capture.pcap &`.

> `-i eth0`: Capture data from the eth0 interface.

> `-nn`: Do not attempt to resolve IP addresses or ports to names.This is best practice from a security perspective, as the lookup data may not be valid. It also prevents malicious actors from being alerted to an investigation.

> `-c9`: Capture 9 packets of data and then exit.

> `port 80`: Filter only port 80 traffic. This is the default HTTP port.

> `-w capture.pcap`: Save the captured data to the named file.

> `&`: This is an instruction to the Bash shell to run the command in the background.

![Screenshot 2024-08-03 at 10 36 22 PM](https://github.com/user-attachments/assets/171194af-4937-47e6-bad1-441cf2e6608b)

* Use `curl` to generate some HTTP (port 80) traffic: `curl opensource.google.com`.
> Open a website and generate some HTTP (TCP Port 80) traffic that can be captured.   


![Screenshot 2024-08-03 at 10 36 27 PM](https://github.com/user-attachments/assets/0c985cb0-792a-43e1-8385-78460c744d15)

* Verify the packet data has been captured: `ls -l capture.pcap`.

![Screenshot 2024-08-03 at 10 36 33 PM](https://github.com/user-attachments/assets/ccf3711f-8ed1-4f8c-a2c5-33b56db31eb0)

4. Filter the captured packet data.
* Filter the packet header data from the `capture.pcap` capture file: `sudo tcpdump -nn -r capture.pcap -v`.

![Screenshot 2024-08-03 at 10 36 43 PM](https://github.com/user-attachments/assets/4140cafb-9161-4806-a2a2-ddd18e6f09bc)

> `-nn`: Disable port and protocol name lookup.

> `-r`: Read capture data from the named file.

> `-v`: Display detailed packet data. 

* Filter the extended packet data from the `capture.pcap` capture file: `sudo tcpdump -nn -r capture.pcap -X`.

![Screenshot 2024-08-03 at 10 37 00 PM](https://github.com/user-attachments/assets/61a94013-d80d-4db4-bfc4-52dda509754f)

> `-nn`: Disable port and protocol name lookup.

> `-r`: Read capure data from the named file.

> `-X`: Display the hexadecimal and ASCII output format packet data. Security analysts can analyze hexadecimal and ASCII output to detect patterns or anomalies during malware analysis or forensic analysis.

> Note: Hexadecimal, also known as hex or base 16, uses 16 symbols to represent values, including the digits 0-9 and letters A, B, C, D, E, and F. American Standard Code for Information Interchange (ASCII) is a character encoding standard that uses a set of characters to represent text in digital form.
