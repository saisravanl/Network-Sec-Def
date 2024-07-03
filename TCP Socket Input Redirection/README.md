# TCP Socket Input Redirection Example

## Overview

This C program demonstrates how to create a TCP socket to connect to a specified server, read data from the server, and redirect standard input from the TCP connection. It showcases basic network programming techniques in C, such as creating sockets, connecting to a server, and using `dup2` to redirect input.

## Prerequisites

- A Unix-like operating system (e.g., Linux)
- GCC (GNU Compiler Collection) or another C compiler

## Compilation

To compile the program, use the following command:

```bash
gcc -o redirect_from_tcp redirect_from_tcp.c
```

## Usage

To run the compiled program, use the following command:

```bash
./redirect_from_tcp
```

## Program Details

### Headers

- **stdio.h**: For standard input and output functions.
- **string.h**: For memory manipulation functions.
- **sys/socket.h**: For socket functions.
- **arpa/inet.h**: For Internet operations.
- **unistd.h**: For POSIX API functions.

### Function: `main()`

1. **Variable Declaration**:
   - `struct sockaddr_in server`: A structure to hold server information such as IP address, port number, and address family.
   - `int sockfd`: File descriptor for the created socket.
   - `char data[100]`: Buffer to hold the data read from the server.

2. **Socket Creation**:
   - `int sockfd = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP)`: Creates a TCP socket. 
     - `AF_INET`: Specifies the address family for IPv4.
     - `SOCK_STREAM`: Specifies a stream socket (TCP).
     - `IPPROTO_TCP`: Specifies the TCP protocol.

3. **Server Information Initialization**:
   - `memset(&server, '\0', sizeof(struct sockaddr_in))`: Initializes the server structure to zero.
   - `server.sin_family = AF_INET`: Sets the address family to IPv4.
   - `server.sin_addr.s_addr = inet_addr("10.0.2.69")`: Sets the server IP address. Replace `"10.0.2.69"` with the target server's IP address.
   - `server.sin_port = htons(8080)`: Sets the server port number. Replace `8080` with the target port number. The `htons` function converts the port number to network byte order.

4. **TCP Connection Establishment**:
   - `connect(sockfd, (struct sockaddr*) &server, sizeof(struct sockaddr_in))`: Establishes a connection to the server.

5. **Data Reception**:
   - `dup2(sockfd, 0)`: Duplicates the socket file descriptor to standard input (stdin). This redirects `scanf` input from the TCP connection.
   - `scanf("%s", data)`: Reads data from the TCP connection into the buffer.
   - `printf("%s\n", data)`: Prints the received data to the standard output.

### Example Output

Upon running the program, it connects to the server at `10.0.2.69:8080`, reads data from the TCP connection, and prints the received data.

## Notes

- Ensure that the server at the specified IP address and port is running and capable of sending data over TCP connections.
- The `inet_addr` function converts the IP address from a string to an appropriate format for the `sockaddr_in` structure.
- The `dup2` function is used to duplicate the socket file descriptor to standard input, allowing `scanf` to receive data from the socket instead of the terminal.
- This program is a basic example and does not include comprehensive error handling for simplicity. Consider adding error checks for each system call for a more robust implementation.
