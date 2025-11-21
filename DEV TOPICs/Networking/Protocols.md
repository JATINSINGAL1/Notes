they are set of rules for communication .

# TCP

Transmission Control Protocol , it is unidirectional way of communication which is controlled in sense a 3 way handshake ensure the connection between two systems before exchanging the information .

also this protocol itself tackle problems like

- flow control
    
    Flow control is a mechanism that regulates the rate of data transmission between a sender and a receiver in a network. Its primary purpose is to prevent a fast sender from overwhelming a slower receiver by ensuring that the sender does not transmit more data than the receiver can process or store in its buffer at any time. Flow control usually operates at the data link or transport layer and uses different protocols or feedback mechanisms to match the sender’s rate to the receiver’s capacity—such as sending acknowledgments or specific signals to pause transmission until the receiver is ready.
    
- **congestion control**
    
    Congestion control is a network technique to prevent the network from becoming overloaded due to excessive data traffic. Congestion occurs when too much data is transmitted, causing routers or links in the network to be overwhelmed, leading to dropped packets, delays, and poor  
    network performance. Congestion control mechanisms regulate the rate of data sent into the network, typically at the network or transport layer, to ensure fair and efficient use of network resources and to prevent overall congestion.  
    
      
    

  

  
TCP is called **connection-oriented** and **stateful**. By means of stateful here is both server and client remember the connection exits.  

### What is 3-way Handshake ?

consider a client before sending a request a connection is ensured by sending a [SYN] packet , which is responded with [SYN-ACK] packet from server and again [ACK] packet from client which ensure the packet has been received.

  

### File Descriptor

the connection is established (socket) and server will create a file descriptor

for every request received by server a file is created inside the memory this is called file descriptor which stores many information also include the hash of (receiver and senders (port and Ip) .

Like when the request is served by the server the response need to be send the location where it need to be send is obtained from this file descriptor .

**To summarize:**

- **Socket:** The actual "phone line" or communication endpoint for a single conversation.
- **File Descriptor:** A simple integer (`3`, `4`, `5`...) that your program uses as a nickname or handle to tell the OS which socket it wants to use.

  

### Working Summary

Sending of data : the request or data is broke into segments by TCP, and assigned a sequence number which is assembled by the receiver. As IP protocol doesn’t guarantee us even the delivery of packet , so in TCP it get easy to find if any packet is missing, server will respond with ACK for every request telling the status of packets received.

like if any packet is lost recipient send ACK for up to the where all the packets are received like packet 1 , 2 and 3 were to be received but only 1 and 3 are received so the ACK 1 is sent informing till packet 1 all sequence is formed so the sender sent 2 and 3 again but 3 will be rejected as it’s already there with packet 2 making sequence complete .

  

### Ending the Connection: The Four-Way Handshake 👋

When the conversation is over, you can't just hang up. Both sides must agree to close the connection politely. This is done with a **Four-Way Handshake** using `FIN` (Finish) packets.

1. **Client sends FIN:** "I'm done sending data."
2. **Server sends ACK:** "Got it, you're done."
3. **Server sends FIN:** "Okay, I'm done sending data too."
4. **Client sends ACK:** "Got it. Goodbye."

Now the connection is gracefully terminated, and the **sockets** (the open phone lines) on both ends are closed and their resources are freed up.

  

# UDP

user datagram protocol , it is the most simple protocol just here to send and receive data with out any extra overhead like TCP.