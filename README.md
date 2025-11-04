# Encrypted CLI Chat in C (AES + HMAC)

This program simulates an encrypted chat between two users using two terminals. Both users can send and receive messages securely in real-time, just like WhatsApp, but with every cryptographic step displayed in the terminal. The system uses AES-256-CBC for encryption and HMAC-SHA256 for message authentication, ensuring confidentiality and integrity.

Both terminals act as sender and receiver. Every time a user sends a message, the program will display:
- Original message
- Generated IV (Initialization Vector)
- Generated MAC (Message Authentication Code)
- Message + MAC (combined)
- Encrypted output
- Sent data
When the other user receives it, the terminal will show:
- Received IV
- Ciphertext
- Decrypted message
- MAC verification result

This demonstrates end-to-end encryption and message integrity in a simple, command-line-based chat.

To build the project, install OpenSSL libraries:
sudo apt install libssl-dev

Then compile:
gcc -O2 crypto_chat.c -o crypto_chat -lssl -lcrypto -lpthread

To generate shared keys, run:
./crypto_chat -g
It will output a 128-hex-character key (first 64 = encryption key, next 64 = MAC key). Both users must use the same key.

Example:
Shared keys (hex): a9f2109dbc28d97182001fb71f8bd7fba163442b8ed2800bd21e78dec46a25e3e4e4f0e24b6586157a7ca07d2793182a9c9752c6777c753139b1e4e457685cb8

To start chatting, open two terminals.
In the first terminal (server/user A):
./crypto_chat -p 5555 -k a9f2109dbc28d97182001fb71f8bd7fba163442b8ed2800bd21e78dec46a25e3e4e4f0e24b6586157a7ca07d2793182a9c9752c6777c753139b1e4e457685cb8
In the second terminal (client/user B):
./crypto_chat -c 127.0.0.1 -p 5555 -k a9f2109dbc28d97182001fb71f8bd7fba163442b8ed2800bd21e78dec46a25e3e4e4f0e24b6586157a7ca07d2793182a9c9752c6777c753139b1e4e457685cb8

Now both can chat securely. Every sent message will go through encryption and authentication steps, and every received message will go through decryption and MAC verification.

Example terminal output:
Enter message: hello
Generated IV: 71c39a...
Computed MAC: ae23f9...
Appended message + MAC: 68656c6c6f|ae23f9...
Encrypted output (AES-256-CBC): 8a7e0c4b...
[SENT] Encrypted message sent.

[RECEIVED]
Received IV: 71c39a...
Ciphertext: 8a7e0c4b...
Decrypted: hello
MAC verified ✅

Each step is shown clearly for learning how secure communication works at a low level.

If MAC verification fails, the receiver will be alerted of possible message tampering. This demonstrates both confidentiality (AES) and integrity (HMAC).

Command options:
-g  Generate new keys
-p  Start as server (listening on port)
-c  Connect as client (IP required)
-k  Provide shared AES+HMAC keys

This is a hands-on educational project showing cryptography in action inside a real-time chat system. It helps visualize encryption, authentication, and data flow between two endpoints.

Created by Bhuvan B N
LinkedIn: https://www.linkedin.com/in/bhuvanbn/
