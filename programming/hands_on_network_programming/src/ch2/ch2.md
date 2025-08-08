# Getting to Grips with Socket APIs

## What are sockets?

- A socket is an endpoint of communication link between systems. Your application sends and receives all of its network data through a socket.
- **Berkley sockets** -> de facto we can use it interchangeble with Unix sockets, POSIX sockets and BSD sockets.
- **Winsock** - windows sockets which are mostly compliant with Berkley sockets.

## Two types of sockets

- **TCP** -> connection-orieted, whereas **UDP** -> connectionless
- **Connection-oriented**
  - data packets must arrive in the order they were sent.
  - if we fail sending a data packet, retry is mandatory.
  - prevents duplicate data.
  - used in FTP, HTTP, SMTP, SSH, etc.
- **Connectionless**
  - each data packets is addressed individually, completely independent and unrelated.
  - postcards reference, you can send many packets but you don't know if all will arrive and the order of arrival.
  - on UDP a packet may even arrive twice.
  - used in voice, games, DNS(both the request and response can fit into a single packet).

## Socket functions

- **socket**() - create a socket.
- **bind**() - bind the socket with and IP and a port.
- **listen**() - TCP socket listening for new connections on a server.
- **connect**() - set the remote addr and port of a client.
- **accept**() - used on server to create a new socket for an incoming TCP conn.
- **send**() and **recv**() - send & receive data.
- **sendto**() and **recvfrom**() - same but without a bound remote addr.
- **close**(), **shutdown**() - it's clear
- **select**() - wait for an event on one or more sockets.
- **getnameinfo**() && **getaddrinfo**() - protocol-independent manner of handling hostnames and addreses.
- **setsockopt**() - used to change socket options.
- **fnctl**() and **ioctlsocket**() --> Winsock, used for options

## TCP program flow

1. TCP client manually introduces the address(https://example.com)
2. getaddinfo() function return the IP & port of this address into a struct addrinfo structure.
3. The client connects to the server by calling connect().
4. Client is ready to exchange data.
5. TCP server waits for the connections at the established IP and port.
6. It calls accept() function, waiting for a client to connect, which will create a new socket, leaving the listening socket available for other clients.

## UDP program flow

1. An UDP client must know the address of the remote UDP peer in order to send the first packet(calls getaddrinfo()).
2. Once it is done, the clients creates an adequate socket.
3. Client uses sendto() to send the first packet.
4. Now the client can continue to sendto() and recvfrom() on the socket.
5. UDP client cannot receive data first because unlike to TCP, there is no handshake going on in order to establish the connection to the server.
6. The configuration of the UDP server is the same with the TCP(getaddrinfo(), create socket, bind).
7.The server listen using recvfrom(), after receiving first information it can either continue listening with recfrom() or using sendto() to send data.\

## Berkley sockets versus Winsock sockets

### Header files

- Headers differ between implementations.

### Socket data type

- on **UNIX** a socket is represented by a standard file descriptor(int).
- on **Windows** a socket can be anything.(SOCKET)
- **solution**: define SOCKET macro on UNIX for the purpose of using the same code in this book.

### Invalid sockets

- on Windows the function socket() returns INVALID_SOCKET if it fails.
- on Unix the fucntions socket() returns a negative number on failure.
- again solution create a ISVALIDSOCKET(s) macro in order to mantain code.

### Closing sockets

- same as before, macro CLOSESOCKET(s)

### Error handling

- same, macro GETSOCKETERRORNO()


## Networking with inetd

- turn console applications into networked ones.
- can configure it in /etc/inetd.conf with program's location, port and protocol(TCP or UDP).
- all sockets inputs are redirected through stdin and stdout.

