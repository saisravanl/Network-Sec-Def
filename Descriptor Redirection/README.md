# File Descriptor Redirection Example

## Overview

This C program demonstrates how to use file descriptors to redirect standard input and output to files. It opens an input file and an output file, redirects standard input and output to these files, reads data from the input file, and writes it to the output file.

## Prerequisites

- A Unix-like operating system (e.g., Linux)
- GCC (GNU Compiler Collection) or another C compiler

## Compilation

To compile the program, use the following command:

```bash
gcc -o fd_redirection fd_redirection.c
```

## Usage

To run the compiled program, ensure that the `/tmp/input` and `/tmp/output` files exist. You can create and populate the input file with some data for testing:

```bash
echo "HelloWorld" > /tmp/input
touch /tmp/output
```

Then, run the program with appropriate permissions:

```bash
./fd_redirection
```

## Program Details

### Headers

- **unistd.h**: For POSIX API functions.
- **stdio.h**: For standard input and output functions.
- **fcntl.h**: For file control options.

### Function: `main()`

1. **Variable Declaration**:
   - `int fd0, fd1`: File descriptors for the input and output files.
   - `char input[100]`: Buffer to hold the data read from the input file.

2. **File Operations**:
   - `fd0 = open("/tmp/input", O_RDONLY)`: Opens the file `/tmp/input` for reading.
   - `fd1 = open("/tmp/output", O_RDWR)`: Opens the file `/tmp/output` for reading and writing.
   - `printf("File descriptors: %d, %d\n", fd0, fd1)`: Prints the file descriptors returned by the `open` function.

3. **File Descriptor Redirection**:
   - `dup2(fd0, 0)`: Duplicates the file descriptor `fd0` to standard input (stdin). This redirects `scanf` input to read from the input file.
   - `dup2(fd1, 1)`: Duplicates the file descriptor `fd1` to standard output (stdout). This redirects `printf` output to write to the output file.

4. **Data Reading and Writing**:
   - `scanf("%s", input)`: Reads data from the input file into the buffer.
   - `printf("%s\n", input)`: Writes the data from the buffer to the output file.

5. **File Closing**:
   - `close(fd0)`: Closes the input file descriptor.
   - `close(fd1)`: Closes the output file descriptor.

### Example Output

Upon running the program, the contents of the `/tmp/input` file will be read and written to the `/tmp/output` file. The program will print the file descriptors used for the input and output files.

## Notes

- Ensure that the `/tmp/input` file exists and contains data to be read. The `/tmp/output` file will be created or overwritten if it exists.
- The `dup2` function is used to duplicate file descriptors to standard input and output, allowing `scanf` to read from the input file and `printf` to write to the output file.
- This program is a basic example and does not include comprehensive error handling for simplicity. Consider adding error checks for each system call for a more robust implementation.
