#!/usr/bin/env python

import socket
from scapy.all import *

# Callback function to handle incoming packets
def handle_packet(packet):
    if packet.haslayer(ICMP) and packet[IP].src == "10.9.0.1":
        # Create a new IP packet with the source and destination swapped
        ip_packet = IP(src=packet[IP].dst, dst=packet[IP].src)

        # Create a new ICMP packet with the type and code swapped
        icmp_packet = ICMP(type=0, code=0)

        # Construct the spoofed packet by combining the new IP and ICMP packets
        spoofed_packet = ip_packet/icmp_packet/str(packet[Raw])

        # Send the spoofed packet using a raw socket
        with socket.socket(socket.AF_INET, socket.SOCK_RAW, socket.IPPROTO_ICMP) as s:
            s.sendto(bytes(spoofed_packet), (packet[IP].dst, 0))

# Sniff ICMP packets sent from 10.9.0.1
sniff(filter="icmp and src host 10.9.0.1", prn=handle_packet)
