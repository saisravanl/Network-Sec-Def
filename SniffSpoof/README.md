# ICMP Packet Spoofer

## Overview

This Python script listens for ICMP packets from a specific IP address and responds by spoofing the packet with swapped source and destination IP addresses. It utilizes the `scapy` library for packet manipulation and raw socket programming for sending the spoofed packets.

## Prerequisites

- Python 3.x
- `scapy` library

Ensure that the required Python packages are installed. You can install `scapy` using pip:

```bash
pip install scapy
```

## Usage

1. Save the script to a file, for example, `icmp_spoofer.py`.
2. Run the script with appropriate permissions (root or sudo) as it requires raw socket access:

```bash
sudo python icmp_spoofer.py
```

## Script Details

### Imports

- **socket**: For creating raw sockets.
- **scapy.all**: For packet manipulation and sniffing.

### Callback Function: `handle_packet(packet)`

- **Purpose**: Handles incoming ICMP packets and sends a spoofed response.
- **Parameters**: 
  - `packet`: The incoming packet captured by `scapy`.
- **Functionality**:
  1. Checks if the packet contains an ICMP layer and is from the IP address `10.9.0.1`.
  2. Creates a new IP packet with the source and destination addresses swapped.
  3. Creates a new ICMP packet with the type and code set to `0` (Echo Reply).
  4. Constructs the spoofed packet by combining the new IP and ICMP headers with the original payload.
  5. Sends the spoofed packet using a raw socket.

### Packet Sniffing

- **sniff(filter="icmp and src host 10.9.0.1", prn=handle_packet)**:
  - Captures ICMP packets from the IP address `10.9.0.1` and processes them using the `handle_packet` function.

## Notes

- The script must be run with elevated privileges to access raw sockets.
- The IP address `10.9.0.1` is hardcoded as the source address to listen for ICMP packets. Modify it as necessary for your use case.
- This script is for educational purposes and should be used responsibly and ethically.
