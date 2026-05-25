<br/>
<p align="center">
  <h3 align="center">LangOS Distributed File System</h3>
  <p align="center">
    A robust, scalable distributed file system written in C.
    <br/>
    <br/>
  </p>
</p>

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Language: C](https://img.shields.io/badge/Language-C-orange.svg)

## 📖 Overview

LangOS Distributed File System is a robust and scalable custom distributed file system (DFS) designed to abstract the complexity of network storage. The architecture consists of multiple active components communicating over TCP/IP sockets to provide a unified filesystem interface. 

The project separates concerns by using a centralized **Name Server (NM)** to handle metadata and routing, multiple **Storage Servers (SS)** to handle raw data persistence, and **Clients** capable of performing concurrent operations.

It is designed for speed, fault tolerance, and effective concurrent access.

## ✨ Features

- **Trie-based File Discovery:** Uses a high-performance Trie structure in the Name Server to manage metadata and rapidly resolve file paths.
- **LRU Caching:** The Name Server implements an LRU (Least Recently Used) cache to store recent file lookups, minimizing search latency.
- **Granular Access Control (ACL):** Robust file permissions system including file ownership and Read/Write access list management for registered users.
- **Concurrent Operations:** Multi-threaded servers capable of handling simultaneous connections using POSIX threads natively.
- **Separation of Concerns:** Clear architectural distinction separating control flow (Name Server) from data flow (Storage Servers).
- **Network Transparency:** Clients perform operations seamlessly across a distributed network of Storage Servers, managed invisibly by the Name Server.

## 🏗️ Architecture

The system utilizes a client-server architecture with three primary entities:

1. **Name Server (NM):** 
   - Acts as the central metadata manager and control plane.
   - Maintains the filesystem tree, tracks active Storage Servers and connected Clients.
   - Handles file creation, deletion, authorization, and location resolution.
   
2. **Storage Server (SS):**
   - The data plane of the application.
   - Handles physical data storage, file reads, and file writes.
   - Communicates with both the Name Server (for heartbeat/metadata) and Clients (for data transfer).

3. **Client:**
   - Provides an interface to interact with the DFS.
   - Contacts the NM for file lookup and metadata operations.
   - Connects directly to the assigned SS for High-Bandwidth data transfers.

## 🛠️ Tech Stack

- **Language:** C (C99/C11 standard)
- **Concurrency:** `pthread` (POSIX Threads)
- **Networking:** TCP/IP Sockets (`sys/socket.h`, `netinet/in.h`)
- **Build System:** `Make`

## ⚙️ Installation & Setup

### Prerequisites
- GCC Compiler
- Make
- POSIX-compliant OS (Linux/macOS)

### Building the Project

The project includes a `Makefile` simplifying the build process.

```bash
git clone https://github.com/adityasingh718/Doc-system.git
cd Doc-system
make
```

This will compile three separate executable binaries:
- `name_server`
- `storage_server`
- `client`

## 🚀 Usage

To run the full cluster, launch the components in the following order:

1. **Start the Name Server:**
   ```bash
   ./name_server
   ```

2. **Start a Storage Server:**
   (Ensure it connects to the Name Server's IP/Port)
   ```bash
   ./storage_server
   ```

3. **Start the Client:**
   ```bash
   ./client
   ```

*(Configuration options like ports and IP arguments depend on your specific environment and run options. Check the internal headers for default ports if testing on localhost).*

## 🧠 Technical Highlights

- **Custom LRU Cache:** We implemented a thread-safe custom LRU cache structure from scratch using a doubly linked list and a hash mechanism within the C ecosystem.
- **Memory Management:** Great care was taken to avoid memory leaks native to long-running C network architecture, heavily managing dynamic allocations associated with `FileMetadata` and `CacheEntry` structures.
- **Distributed Synchronization:** Managing synchronization between the Name Server and various Storage Servers proved to be an exciting challenge, utilizing mutexes (`pthread_mutex_t`) to prevent race conditions during metadata updates.

## 👥 Authors

- **Aditya Singh** - [@adityasingh718](https://github.com/adityasingh718)
- **Rishabh Goyal** - [@rishabh918](https://github.com/rishabh918)

---
*Feel free to star ⭐ the repository if you found it useful or interesting!*