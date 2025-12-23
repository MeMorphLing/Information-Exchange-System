# Information Exchange System

The **Information Exchange System** is a centralized client–server networking application designed to model the communication infrastructure between multiple **FAST-NUCES campuses**.  
It uses a **hybrid networking approach** by combining **TCP** and **UDP** protocols to balance reliability, scalability, and efficiency.

This project is implemented in **C** and demonstrates low-level socket programming, I/O multiplexing, and custom application-layer protocol design.

---

## 📌 System Architecture

The system follows a **centralized client–server architecture** with three primary components:

### 1. Central Server
- Acts as the main communication hub (e.g., Islamabad Campus).
- Handles multiple TCP clients and UDP datagrams concurrently.
- Uses **I/O multiplexing** via the `select()` system call.
- Avoids complex multi-threading while maintaining responsiveness.

### 2. Campus Clients
- Each client represents a **Department within a Campus**  
  (e.g., `LAHORE-CS`, `KARACHI-ACADEMICS`).
- Maintains:
  - A **persistent TCP connection** for reliable message delivery.
  - A **UDP socket** for lightweight status updates (heartbeats).

### 3. Admin Console
- A separate administrative tool.
- Allows system administrators to:
  - View active clients.
  - Broadcast system-wide announcements using UDP.

---

## 🚀 Core Features

### 🔁 Concurrency
- The server manages multiple clients simultaneously using:
  - `fd_set`
  - `select()`
- Enables non-blocking handling of:
  - Authentication
  - Messaging
  - Heartbeat tracking

### 🔐 Authentication
- Clients must authenticate immediately after connecting.
- Credentials are validated against a **hard-coded internal passTable**.
- Unauthorized clients are rejected.

### 📬 Message Routing
- Implements an **application-layer routing table**.
- Messages are routed based on:

- The server forwards messages to the correct active TCP socket.

###  Heartbeat Mechanism
- Clients send periodic UDP heartbeats every **7 seconds**.
- Helps the server:
- Track online status.
- Detect stale or disconnected clients.
- Prevents unnecessary TCP traffic.

---

## 📡 Protocol Design

The system runs a **custom application-layer protocol** over standard TCP/IP sockets.

---

### 1️⃣ TCP Protocol (Port `9000`)
Used for **critical, reliable communication**.

#### 🔐 Authentication Handshake
**Client → Server**
