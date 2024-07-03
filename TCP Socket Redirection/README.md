# TCP Socket Redirection Example

## Overview

This C program demonstrates the creation and use of a TCP socket to connect to a specified server, send data, and redirect standard output to the TCP connection. It shows how to establish a TCP connection and use `dup2` to redirect output, providing a practical example of network programming in C.

## Prerequisites

- A Unix-like operating system (e.g., Linux)
- GCC (GNU Compiler Collection) or another C compiler

## Compilation

To compile the program, use the following command:

```bash
gcc -o redirect_to_tcp redirect_to_tcp.c
```

## Usage

To run the compiled program, use the following command:

```bash
./redirect_to_tcp
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

5. **Data Transmission**:
   - `char *data = "Hello World!"`: Data to be sent to the server.
   - `dup2(sockfd, 1)`: Duplicates the socket file descriptor to standard output (stdout). This redirects `printf` output to the TCP connection.
   - `printf("%s\n", data)`: Sends the data to the server via the TCP connection.

### Example Output

Upon running the program, the string "Hello World!" will be sent to the server at `10.0.2.69:8080`. The program will print the file descriptor used for the connection and send the message through the established TCP connection.

## Notes

- Ensure that the server at the specified IP address and port is running and capable of receiving TCP connections.
- The `inet_addr` function converts the IP address from a string to an appropriate format for the `sockaddr_in` structure.
- The `dup2` function is used to duplicate the socket file descriptor to standard output, allowing `printf` statements to send data through the socket instead of the terminal.
- This program is a basic example and does not include comprehensive error handling for simplicity. Consider adding error checks for each system call for a more robust implementation.
