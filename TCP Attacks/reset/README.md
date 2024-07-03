# TCP Reset Attack Script

## Overview

This Python script uses Scapy to perform a TCP reset attack. It listens for TCP packets on a specific interface and port, then sends spoofed TCP RST packets to terminate the connection.

## Prerequisites

- Python 3.x
- Scapy library

Ensure that the required Python packages are installed. You can install Scapy using pip:

```bash
pip install scapy
```

## Usage

1. Save the script to a file, for example, `tcp_reset.py`.
2. Run the script with appropriate permissions (root or sudo) as it requires raw socket access:

```bash
sudo python3 tcp_reset.py <client_ip> <server_ip>
```

### Arguments

- `<client_ip>`: The IP address of the client.
- `<server_ip>`: The IP address of the server.

### Example

```bash
sudo python3 tcp_reset.py 192.168.1.100 192.168.1.200
```

## Script Details

### Imports

- **scapy.all**: For packet manipulation and sniffing.
- **sys**: For command-line argument handling.

### Function: `spoof(pkt)`

- **Purpose**: Handles incoming TCP packets and sends spoofed TCP RST packets to terminate the connection.
- **Parameters**: 
  - `pkt`: The incoming packet captured by Scapy.
- **Functionality**:
  1. Extracts the IP and TCP layers from the incoming packet.
  2. Creates new IP and TCP headers with modified values to spoof a TCP RST packet.
  3. Sends the spoofed packet.

### Packet Sniffing

- **sniff(iface='br-10e5fd113347', filter=myFilter, prn=spoof)**:
  - Captures TCP packets based on the filter criteria and processes them using the `spoof` function.
  - **iface**: The network interface to listen on. Replace `'br-10e5fd113347'` with the actual interface name.
  - **filter**: A BPF filter string that captures TCP packets from the specified server to the client on port 23.

### Filter String

- Constructs a filter string to capture TCP packets from the server to the client on port 23:

```python
myFilter = 'tcp and src host {} and dst host {} and src port 23'.format(server, client)
```

### Print Statements

- Displays information about the attack being performed, the filter used, and the source and destination IP addresses.

## Notes

- The script must be run with root privileges to access raw sockets.
- The `iface` field should be updated with the actual name of the network interface on your system.
- This script is for educational purposes only. Unauthorized use of this script for attacking systems is illegal and unethical.
