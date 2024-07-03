# SYN Flood Attack Script

## Overview

This C program generates and sends spoofed TCP SYN packets to a specified IP address and port. It uses raw sockets to craft and transmit the packets, simulating a SYN flood attack.

## Prerequisites

- A Unix-like operating system (e.g., Linux)
- GCC (GNU Compiler Collection) or another C compiler
- Root privileges to run the script (required for raw socket operations)

## Compilation

To compile the program, use the following command:

```bash
gcc -o synflood synflood.c
```

## Usage

To run the compiled program, use the following command with appropriate permissions:

```bash
sudo ./synflood <IP_ADDRESS> <PORT>
```

### Arguments

- `<IP_ADDRESS>`: The target IP address.
- `<PORT>`: The target port number.

### Example

```bash
sudo ./synflood 192.168.1.1 80
```

## Program Details

### Headers

The program includes necessary headers for socket programming, packet manipulation, and other standard functions.

### IP Header Structure (`ipheader`)

Defines the structure of an IP header, including fields such as version, header length, type of service, total length, identification, fragmentation flags, time to live, protocol, checksum, and source/destination IP addresses.

### TCP Header Structure (`tcpheader`)

Defines the structure of a TCP header, including fields such as source port, destination port, sequence number, acknowledgment number, data offset, reserved bits, flags, window size, checksum, and urgent pointer.

### Pseudo TCP Header Structure (`pseudo_tcp`)

Defines a pseudo header used for TCP checksum calculation, including source and destination addresses, reserved bits, protocol type, TCP length, and the TCP header.

### Functions

- **send_raw_ip_packet(struct ipheader* ip)**: Sends a raw IP packet using a raw socket.
- **in_cksum(unsigned short *buf, int length)**: Calculates the Internet checksum for the given buffer.
- **calculate_tcp_checksum(struct ipheader *ip)**: Calculates the TCP checksum, including the pseudo header.

### Main Function

1. **Argument Validation**: Ensures that the user provides the target IP address and port number as arguments.
2. **Packet Construction**:
   - Fills in the TCP header with random source port, target port, random sequence number, SYN flag, and other necessary fields.
   - Fills in the IP header with version, header length, TTL, random source IP address, target IP address, protocol, and total length.
   - Calculates the TCP checksum.
3. **Packet Transmission**: Continuously sends the spoofed packets using the `send_raw_ip_packet` function.

## Notes

- This program is for educational purposes only. Unauthorized use of this script for attacking systems is illegal and unethical.
- The program must be run with root privileges to access raw sockets.
