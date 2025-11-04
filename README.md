# Encrypted CLI Chat in C (AES + HMAC)

> A command-line chat demo that showcases end-to-end encryption and message integrity with AES-256-CBC and HMAC-SHA256.

<p align="center">
  <a href="#overview"><img src="https://img.shields.io/badge/Language-C-blue" alt="Language"></a>
  <a href="#cryptographic-flow"><img src="https://img.shields.io/badge/Encryption-AES--256--CBC-purple" alt="Encryption"></a>
  <a href="#cryptographic-flow"><img src="https://img.shields.io/badge/Integrity-HMAC--SHA256-brightgreen" alt="Integrity"></a>
</p>

## 📚 Table of Contents

- [Overview](#overview)
- [Cryptographic Flow](#cryptographic-flow)
- [Requirements](#requirements)
- [Build Instructions](#build-instructions)
- [Generating Shared Keys](#generating-shared-keys)
- [Starting the Secure Chat](#starting-the-secure-chat)
- [Sample Terminal Output](#sample-terminal-output)
- [Command Options](#command-options)
- [Learning Notes](#learning-notes)
- [Author](#author)

## Overview

This program simulates an encrypted chat between two users using two terminals. Both users can send and receive messages securely in real-time, just like WhatsApp, but with every cryptographic step displayed in the terminal. The system uses AES-256-CBC for encryption and HMAC-SHA256 for message authentication, ensuring confidentiality and integrity. Each step is shown clearly for learning how secure communication works at a low level.

## Cryptographic Flow

Both terminals act as sender and receiver.

**When sending a message, the program displays:**

- Original message
- Generated IV (Initialization Vector)
- Generated MAC (Message Authentication Code)
- Message + MAC (combined)
- Encrypted output
- Sent data

**When receiving a message, the terminal shows:**

- Received IV
- Ciphertext
- Decrypted message
- MAC verification result

If MAC verification fails, the receiver is alerted to possible message tampering. This demonstrates both confidentiality (AES) and integrity (HMAC).

## Requirements

Install the OpenSSL libraries before building the project:

```bash
sudo apt install libssl-dev
```

## Build Instructions

Compile the chat application with:

```bash
gcc -O2 crypto_chat.c -o crypto_chat -lssl -lcrypto -lpthread
```

## Generating Shared Keys

Create shared keys for both users:

```bash
./crypto_chat -g
```

The program outputs a 128-hex-character key (first 64 = encryption key, next 64 = MAC key). Both users must use the same key.

**Example**

```
Shared keys (hex): a9f2109dbc28d97182001fb71f8bd7fba163442b8ed2800bd21e78dec46a25e3e4e4f0e24b6586157a7ca07d2793182a9c9752c6777c753139b1e4e457685cb8
```

## Starting the Secure Chat

Open two terminals to begin chatting.

**Server / User A**

```bash
./crypto_chat -p 5555 -k a9f2109dbc28d97182001fb71f8bd7fba163442b8ed2800bd21e78dec46a25e3e4e4f0e24b6586157a7ca07d2793182a9c9752c6777c753139b1e4e457685cb8
```

**Client / User B**

```bash
./crypto_chat -c 127.0.0.1 -p 5555 -k a9f2109dbc28d97182001fb71f8bd7fba163442b8ed2800bd21e78dec46a25e3e4e4f0e24b6586157a7ca07d2793182a9c9752c6777c753139b1e4e457685cb8
```

Now both terminals can chat securely. Every sent message goes through encryption and authentication steps, and every received message is decrypted with MAC verification.

## Sample Terminal Output

```
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
```

## Command Options

| Flag | Description |
| ---- | ----------- |
| `-g` | Generate new keys |
| `-p` | Start as server (listening on port) |
| `-c` | Connect as client (IP required) |
| `-k` | Provide shared AES+HMAC keys |

## Learning Notes

This is a hands-on educational project showing cryptography in action inside a real-time chat system. It helps visualize encryption, authentication, and data flow between two endpoints.

## Author

**Bhuvan B N**  
[LinkedIn](https://www.linkedin.com/in/bhuvanbn/)
