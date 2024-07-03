# Docker Compose Configuration for Seed Labs

## Overview

This Docker Compose configuration sets up a network environment with multiple hosts using the `handsonsecurity/seed-ubuntu:large` image. The setup includes an attacker machine and three hosts (hostA, hostB, hostC) within a specified subnet.

## Prerequisites

- Docker
- Docker Compose

Ensure that Docker and Docker Compose are installed on your system.

## Usage

1. Save the Docker Compose configuration to a file named `docker-compose.yml`.
2. Run the following command to start the services:

```bash
docker-compose up -d
```

This will create and start all the containers defined in the configuration.

## Configuration Details

### Version

- **version: "3"**: Specifies the version of the Docker Compose file format.

### Services

- **attacker**:
  - **image**: `handsonsecurity/seed-ubuntu:large`
  - **container_name**: `seed-attacker`
  - **tty**: `true`
  - **cap_add**: `ALL`
  - **privileged**: `true`
  - **volumes**: 
    - `./volumes:/volumes`
  - **network_mode**: `host`

- **hostA, hostB, hostC**:
  - **image**: `handsonsecurity/seed-ubuntu:large`
  - **container_name**: Corresponding to the host name (hostA, hostB, hostC)
  - **tty**: `true`
  - **cap_add**: `ALL`
  - **networks**:
    - `net-192-168-40-0` with respective IP addresses:
      - hostA: `192.168.40.2`
      - hostB: `192.168.40.3`
      - hostC: `192.168.40.4`
  - **command**: Starts the OpenBSD inetd service and then runs an infinite loop to keep the container running.

### Networks

- **net-192-168-40-0**:
  - **driver**: `bridge`
  - **ipam**:
    - **driver**: `default`
    - **config**:
      - **subnet**: `192.168.40.0/24`

## Notes

- The `attacker` service uses `network_mode: host`, giving it access to the host’s network stack. This is useful for penetration testing scenarios.
- The hosts (hostA, hostB, hostC) are part of a custom bridge network with specific IP addresses.
- The `tty: true` and `cap_add: ALL` options are used to enable a full terminal interface and add all capabilities to the containers.
- The `privileged: true` option in the attacker container grants it extended privileges.

## Stopping the Services

To stop and remove the containers, use the following command:

```bash
docker-compose down
```
