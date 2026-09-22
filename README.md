# 🌐 JavaNetworkCommunicationSuite

A Java networking project demonstrating multiple communication models through **TCP**, **UDP**, and **UDP Multicast**, with interactive **JavaFX interfaces**, multi-client messaging, media transfer, file routing, and concurrent network processing.

The project was developed to explore and compare different network communication mechanisms and their behavior in client-server and group communication scenarios.

---

## ✨ Overview

`JavaNetworkCommunicationSuite` contains three complementary networking modules:

- **TCP Multi-Client Communication**
- **UDP Multi-Client Messaging**
- **UDP Multicast File Distribution**

Each module demonstrates a different communication model and implements dedicated client/server applications using Java networking APIs.

```text
JavaNetworkCommunicationSuite
│
├── TCP
│   ├── Multi-client server
│   ├── Public messaging
│   ├── Private messaging
│   ├── File transfer
│   └── JavaFX media client
│
├── UDP
│   ├── Public messaging
│   ├── Private messaging
│   ├── Group messaging
│   ├── Client discovery
│   └── Image / GIF transfer
│
└── UDP Multicast
    ├── Group broadcasting
    ├── Text transmission
    ├── Image transmission
    ├── Video chunking
    └── PDF chunking
```

---

# 🔌 Network Communication Models

| Module | Protocol | Communication Model | Main Purpose |
|---|---|---|---|
| **TCP** | TCP | Connection-oriented client/server | Reliable chat and file transfer |
| **UDP** | UDP | Datagram client/server | Lightweight multi-client messaging |
| **Multicast** | UDP Multicast | Group broadcasting | One-to-many media distribution |

---

# 🟦 TCP Multi-Client Communication

The TCP module implements a multi-client chat and file-transfer system using Java sockets.

The server accepts multiple simultaneous connections and assigns a dedicated thread to every connected client.

## Features

- TCP client/server communication
- Multiple simultaneous clients
- Dedicated server thread per client
- Thread-safe connected-client management
- Unique user handles such as:

```text
username#1
username#2
username#3
```

- Public broadcast messages
- Private messaging
- Multiple private recipients
- Connected-user listing
- Graceful client disconnection
- Reusable user identifiers
- Binary file transfer
- Broadcast file transfer
- Targeted file transfer

The TCP server uses concurrent collections such as `CopyOnWriteArrayList` and `ConcurrentHashMap` to coordinate connected users safely across multiple threads. :contentReference[oaicite:0]{index=0}

---

## 💬 TCP Messaging Flow

```text
Client A
   │
   │ TCP Socket
   ▼
TCP Server
   │
   ├──────────────► Client B
   │
   ├──────────────► Client C
   │
   └──────────────► Client D
```

The server supports both broadcast communication and private messages between selected clients. :contentReference[oaicite:1]{index=1}

---

## 📁 TCP File Transfer

Files are transferred as binary streams using:

- `FileInputStream`
- `FileOutputStream`
- `DataInputStream`
- `DataOutputStream`

Files can be sent either to:

```text
all
```

or to selected users.

```text
Sender
   │
   │ FILE_HEADER
   ▼
Server
   │
   │ Binary stream
   ├────────► Recipient A
   └────────► Recipient B
```

The server routes the incoming file stream toward the requested recipients without requiring the whole file to be stored server-side first. :contentReference[oaicite:2]{index=2}

---

## 🖥️ TCP JavaFX Client

The graphical TCP client provides a chat-oriented interface built with JavaFX.

It includes:

- Connection / disconnection controls
- Text messaging
- File selection
- Incoming message display
- Image preview
- Audio playback
- Video playback
- File download links
- Background network reception thread

The JavaFX client separates network reception from the UI thread and uses `Platform.runLater()` when updating graphical components. :contentReference[oaicite:3]{index=3}

Supported media previews include:

- Images
- Audio
- Video
- Generic downloadable files

:contentReference[oaicite:4]{index=4}

---

# 🟨 UDP Multi-Client Messaging

The UDP module explores connectionless communication using:

```java
DatagramSocket
DatagramPacket
```

Both the server and client use JavaFX graphical interfaces.

---

## Features

- UDP client/server communication
- Public messaging
- Private messaging
- Group messaging
- Dynamic connected-client list
- Client identification using name + UDP port
- Dedicated private chat windows
- Dedicated group chat windows
- Per-client message history
- Server communication journal
- Background UDP receiver thread
- Image transmission
- Animated GIF transmission
- Base64 encoding and decoding

The client maintains separate private and group conversation windows and stores conversation history for each communication context. :contentReference[oaicite:5]{index=5}

---

## 👥 UDP Group Messaging

Users can select multiple connected clients and create a group conversation.

```text
Client A
      │
      ▼
 UDP Server
   │   │   │
   ▼   ▼   ▼
   B   C   D
```

The server routes a separate UDP datagram to each selected recipient.

This is **group routing through the server**, not IP multicast.

The server identifies clients through their IP address and UDP port and dynamically maintains a client registry. :contentReference[oaicite:6]{index=6}

---

## 🖼️ Image & GIF Transfer

Images and animated GIFs are:

```text
File
 ↓
byte[]
 ↓
Base64 encoding
 ↓
UDP message
 ↓
Server routing
 ↓
Recipient
 ↓
Base64 decoding
 ↓
JavaFX Image
```

The client supports both private and group image/GIF communication. :contentReference[oaicite:7]{index=7}

---

## 📋 UDP Client Discovery

The server maintains the active clients and periodically distributes an updated client list.

```text
CLIENTS:
   ↓
Client A
Client B
Client C
```

The JavaFX client automatically updates its selectable client list when this information is received. :contentReference[oaicite:8]{index=8}

---

# 🟩 UDP Multicast Distribution

The multicast module demonstrates **one-to-many UDP communication**.

It uses:

```java
MulticastSocket
DatagramPacket
InetAddress
```

with the multicast group:

```text
230.0.0.1
```

and port:

```text
12345
```

Multiple clients can join the multicast group and receive the same transmitted content.

---

## 📡 Multicast Architecture

```text
                     Server
                       │
                       │ UDP Multicast
                       ▼
                230.0.0.1:12345
                  /     |     \
                 /      |      \
                ▼       ▼       ▼
            Client A Client B Client C
```

A single multicast transmission can therefore reach multiple receivers subscribed to the same group.

---

## Multicast Features

The multicast server can transmit:

- Text messages
- JPEG images
- PNG images
- GIF images
- MP4 videos
- PDF documents

The client can:

- Listen continuously to the multicast group
- Display received text
- Display received images
- Reconstruct videos
- Play received videos
- Reconstruct PDF files
- Display PDF documents
- Save received files locally

---

## 🎬 Video Chunking

Large video files are divided into smaller UDP datagrams.

```text
MP4 File
   ↓
Read into bytes
   ↓
Split into ~60 KB chunks
   ↓
Add end-of-file flag
   ↓
UDP Multicast
   ↓
Client receives chunks
   ↓
Reassemble byte buffer
   ↓
Save MP4
   ↓
JavaFX MediaPlayer
```

The final chunk contains a flag indicating that the video transmission is complete.

---

## 📄 PDF Chunking

PDF documents use a similar mechanism but include an additional type marker:

```text
PDF
 ↓
Chunks
 ↓
[PDF TYPE][END FLAG][DATA]
 ↓
UDP Multicast
 ↓
Client reconstruction
 ↓
Saved PDF
```

This demonstrates the implementation of a simple custom protocol on top of UDP.

---

# 🏗️ Overall Architecture

```text
                 Java Network Communication Suite
                             │
          ┌──────────────────┼──────────────────┐
          │                  │                  │
          ▼                  ▼                  ▼
         TCP                UDP           UDP Multicast
          │                  │                  │
          ▼                  ▼                  ▼
   Connection-based     Datagram-based      Group-based
    communication       communication       broadcasting
          │                  │                  │
          ▼                  ▼                  ▼
   Reliable streams     Lightweight        One-to-many
    + file transfer      messaging         distribution
```

---

# 🛠️ Technology Stack

| Technology | Usage |
|---|---|
| **Java** | Main programming language |
| **JavaFX** | Graphical user interfaces |
| **TCP Sockets** | Reliable client/server communication |
| **ServerSocket** | TCP server connection management |
| **DatagramSocket** | UDP communication |
| **DatagramPacket** | UDP packet transmission |
| **MulticastSocket** | UDP multicast communication |
| **Java Threads** | Concurrent network processing |
| **ConcurrentHashMap** | Thread-safe shared server state |
| **CopyOnWriteArrayList** | Concurrent connected-client management |
| **Java IO Streams** | Binary file transfer |
| **JavaFX MediaPlayer** | Audio/video playback |
| **Base64** | UDP image and GIF transport |

---

# 🧵 Concurrency

Network operations are executed outside the JavaFX UI thread.

The project uses:

- Dedicated TCP client-handler threads
- Background receive threads
- JavaFX `Platform.runLater()`
- Thread-safe collections

This prevents blocking network operations from freezing the graphical interface.

---

# 📂 Main Modules

```text
src/
│
├── tcp_mini_project/
│   ├── Serveur.java
│   ├── Client.java
│   └── ChatClientFX.java
│
├── udp_mini_projet/
│   ├── ServeurUDP_Multi.java
│   └── ClientUDP_Multi.java
│
└── MuiltiCastMiniProjet/
    ├── Server.java
    └── Client.java
```

---

# ▶️ Running the Project

The applications can be launched independently from an IDE configured with JavaFX.

### TCP

Start the TCP server first:

```text
Serveur.java
```

Then start one or more clients:

```text
ChatClientFX.java
```

Default TCP port:

```text
1234
```

---

### UDP

Start:

```text
ServeurUDP_Multi.java
```

Then start multiple:

```text
ClientUDP_Multi.java
```

Default UDP server:

```text
127.0.0.1:9876
```

---

### Multicast

Start:

```text
Server.java
```

Then run one or more multicast clients:

```text
Client.java
```

Multicast group:

```text
230.0.0.1:12345
```

---

# 📸 Application Preview

Add screenshots here showing:

- TCP multi-client chat
- TCP file transfer
- Image/video preview
- UDP client list
- UDP private conversation
- UDP group conversation
- UDP server journal
- Multicast media transmission

---

# 🎥 Video Demo

A demonstration video can be added here to show:

1. Multiple TCP clients connecting
2. Public and private messaging
3. TCP file transfer
4. UDP private and group communication
5. UDP image/GIF transfer
6. Multicast transmission to multiple receivers
7. Video/PDF reconstruction

---

# ⚠️ Current Limitations

This project is designed primarily as an academic networking implementation.

Current limitations include:

- No TLS encryption
- No user authentication
- No persistent message database
- UDP does not guarantee delivery or packet ordering
- Standard UDP image/GIF transfer does not implement fragmentation
- Multicast video/PDF chunking does not implement retransmission
- No checksum or integrity-verification mechanism
- Network addresses and ports are currently configured directly in the source code

These limitations also provide opportunities for future improvements.

---

# 🚀 Possible Improvements

Potential extensions include:

- Configurable IP addresses and ports
- TLS-secured TCP communication
- User authentication
- Persistent message history
- UDP packet sequence numbers
- Packet acknowledgements and retransmission
- File integrity validation
- Improved custom packet protocol
- Larger-file fragmentation support
- Network performance monitoring

---

# 🎓 Concepts Demonstrated

This project demonstrates practical understanding of:

- TCP/IP networking
- UDP communication
- IP multicast
- Client-server architecture
- Socket programming
- Concurrent programming
- Thread-safe data structures
- Binary stream processing
- Datagram-based communication
- File transfer protocols
- Custom network protocols
- JavaFX desktop development

---

## 👨‍💻 Author

**Bassem Wali**

Software Engineering Student  
Java • Full-Stack Development • Networking • Applied AI
