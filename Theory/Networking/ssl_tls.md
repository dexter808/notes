# SSL/TLS - Secure Socket Layer / Transport Layer Security

## Steps

### 1. Basic Connection

Client -> Server : SYN

Server -> Client : SYN ACK

Client -> Server : ACK
Client -> Server : Client Hello (The TLS versions I support.)
Client -> Server : Client Random (A random generated string of bytes)

### 2. The Handshake 

Server -> Client : Server Hello (Agreeing upon a tls version and other specs)
Server -> Client : Server's Certificate (Hash{server address, s public key, expiry timestamp}.CA's_Signed_Certificate.SCT_receipt[From the log servers of google or cloudflare])
Server -> Client : Server Random
Server -> Client : Server Hello Completed


#### The Dettective Phase

Client : Calculates the Hash of the payload. Uses CA's public key to decrypt the signature, which gives us a hash. Now we can compare both of these to match which authenticates the certificate. 

Extra: SCT receipt is added as a new form of security where a CA's private key might be compromised and the MIM might try to change the payload and create a fake certificate. The SCT acts as a backup mechanism to detect compromised CAs. There is still a goldilock time in between to perform MIM attack at targets.

Client -> Server : Generates a random pre-master-key and encrypts using public key and sends to the server for Master Key generation.

<-> : Now both the parties have Client Random, Server Random and Pre-Master-Secret. MIM does not have Pre Master Secret. Both uses these 3 input to create a master-key, which would be later used for symmetric encryption and decryption as they are faster.

### 3. The Signal

(This step may or may not be present depending on the TLS version)
Client -> Server : Now we will talk using our master key that we both generated.

Client -> Server : FINISHED (Using master key encryption -> All the previous messages) This is to prevent a downgrade attack where a MIM might try to alter Client Hello to send a weaker encryption.

Server verifies and both agrees on communication now.

### 4. Application communication takes over

Both Parties now use the symmetric enc / dec using master key for communication.


## Extra Clarification
The Symmetric Session Keys (the ones actually used to encrypt the data) are then derived from that Master Secret. Usually, they generate four keys: two for encryption (inbound/outbound) and two for integrity (MAC keys).

### TLS Termination

This means connection is converted from a HTTPS to HTTP for easier and faster processing.

If happening at LB, it means the LB is performing the TLS handshake and other steps with the browser and then converting the content to HTTP and then passing it to the application servers.