# ChronosGuard: Real-Time, Master-Managed Backup System

## Project Goal
To create a robust, scalable, and real-time backup solution using Go, featuring a master-agent architecture for centralized management and efficient data protection.

## Project Features

### Master-Agent Architecture
- **Agents**: Lightweight Go applications installed on target servers to monitor and backup files.
- **Master Server**: Centralized Go application for managing agents, backup configurations, and remote storage.

### Real-Time File Monitoring
- Agents monitor specified files/folders for changes using file system notifications (e.g., fsnotify in Go).
- Backups are triggered immediately upon file modification, not just daily.

### Flexible Backup Configuration
- Master server UI/API to define backup rules:
  - File/folder paths to backup.
  - Backup frequency (real-time and scheduled backups).
  - Remote storage configuration (server address, credentials).
  - Backup retention policies.

### Data Compression and Encryption
- Agents compress backup data (e.g., using gzip or zip libraries).
- Agents encrypt backup data before transmission (e.g., using AES).

### Remote Storage Integration
- Support for various remote storage options:
  - SSH/SFTP servers
  - Cloud storage (AWS S3, Google Cloud Storage, Azure Blob Storage)
  - NFS (for network-based storage)
  - Rsync (for efficient file transfers)
- Allow the master to configure these connections.

### Backup History and Retrieval
- Master server maintains a backup history database.
- UI/API to browse and search backup versions.
- Download functionality to retrieve specific backup files/versions.

### Agent Management
- Master server UI/API to:
  - Register/unregister agents.
  - View agent status and logs.
  - Deploy configuration updates to agents.

### Logging and Monitoring
- Comprehensive logging for both master and agents.
- Basic monitoring capabilities (e.g., agent uptime, backup success/failure).

### Replace Excel Tracking
- Database to store all backup information. This database is to be controlled via the master server.

### Communication Between Agent and Master
To enable secure and efficient communication, a combination of methods will be used:
1. **gRPC for Real-Time Communication**
   - The master exposes gRPC services.
   - Agents use gRPC clients for efficient binary communication.
   - Supports streaming for logs and real-time updates.
2. **Message Queue for Event-Driven Logging & Status Updates**
   - Agents push backup logs, file change events, and heartbeats to a message queue.
   - The master reads from the queue and processes the data.
   - Options: Kafka, RabbitMQ, or NATS.
3. **Secure File Transfers**
   - SSH/SFTP, NFS, or Rsync will be used for efficient and secure file transfers.
   - Ensures reliability and security when moving backup data.
4. **Persistent WebSockets for Live Updates (Optional)**
   - Agents maintain a persistent WebSocket connection to the master.
   - Enables the master to push updates to agents in real-time.

## Project Plan

### Phase 1: Architecture and Core Functionality (4-6 weeks)
- **Define API Contracts**: Design the communication protocol between master and agents (gRPC, message queue, etc.).
- **Agent Development**:
  - Implement file monitoring using fsnotify.
  - Develop backup logic (compression, encryption).
  - Create agent registration and communication functionality.
- **Master Server Development (Core)**:
  - Set up database schema for backup configurations and history.
  - Implement basic API endpoints for agent management.
  - Create the ability to store and edit backup configurations.
- **Basic Remote Storage (SSH/SFTP, NFS, Rsync)**:
  - Implement the ability for agents to send the backups to a remote server.

### Phase 2: UI/UX and Advanced Features (6-8 weeks)
- **Master Server UI Development**:
  - Create a web-based UI for managing agents and backup configurations.
  - Implement backup history browsing and download functionality.
- **Advanced Remote Storage Integrations**:
  - Add support for cloud storage providers (AWS S3, etc.).
- **Enhanced Monitoring and Logging**:
  - Implement detailed logging and monitoring features.
  - Alerting system for backup failures.
- **Security Enhancements**:
  - Implement user authentication and authorization for the master server.
  - Ensure secure communication between master and agents.

### Phase 3: Testing, Deployment, and Optimization (4-6 weeks)
- **Comprehensive Testing**:
  - Unit testing, integration testing, and end-to-end testing.
  - Performance testing and load testing.
- **Deployment Strategy**:
  - Create deployment scripts for master and agents.
  - Containerization (Docker) for easier deployment.
- **Optimization**:
  - Performance tuning of backup and data transfer processes.
  - Optimize database queries.
- **Documentation**:
  - Create user documentation and technical documentation.

## Technology Stack
- **Programming Language**: Go
- **Database**: PostgreSQL, MySQL, or SQLite (depending on scalability needs)
- **Web Framework (Master Server UI)**: Gin, Echo, or standard net/http with templating.
- **Remote Storage**: SSH/SFTP libraries, NFS, Rsync, AWS SDK, Google Cloud SDK, Azure SDK.
- **Compression**: gzip, zip
- **Encryption**: crypto/aes
- **File System Monitoring**: fsnotify
- **API**: gRPC for agent communication, RESTful API for UI interactions.
- **Containerization**: Docker
