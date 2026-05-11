# OSI (Open-System-Intercommunication) Layers

Imagine it as a building where all applications sit at the top of the building. Each building has 7 layers, and people (network packets) have to travel from top of one building to the top of another building step by step.

## Physical Layer

The wires, the physical medium through which the signals have to travel.

Ethernet Physical signaling, Optical Transceivers, etc.

The Roads

## Data Link Layer

How these wires, or the physical layer is connected to nodes.

Ethernet, Wifi, PPP (Point to Point protocol) etc.
  
The Delivery Trucks

## Network Layers

how do these network packets travel between one node to another node though a series of router. This step is where all routing takes place.
  
The GPS System

## Transport Layer

How data is transmitted between one node to another, like any protocols that maybe followed while transmitting and recieving the packets.

TCP, UDP

The Logistics Manager - Decides how to send the data - verification of each / small segmented fast seending for quick delivery.

## Session Layer

How is a session established, how long is it established for and things like these.

RPC, Streaming etc.
  
The Secretary Office - The receptioninst who establishes the communication session with the other building's secretary office, so that further communication can happen effectively. Remember all communication happen via envelopes travelling these layers.

## Presentation Layer

How is the data formatted for the application to read. Formatting, Encryption, Decryption, Compression, etc.

JPEG, SSL / TLS etc.
  
The Translator Office - All the messages are converted into a single language, encrypted for sending or decrypted after receiving, or compressed or decompressed for large letters etc.  

## Application Layer

The layer which all applications interact with.

HTTP/ HTTPS - s stands for TLS/SSL, FTP etc.

The Penthouse where all the users (Applications) recide and can use this floor for sending or receiving letters.
