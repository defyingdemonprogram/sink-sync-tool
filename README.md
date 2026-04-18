# Sink: LAN Peer-to-Peer File Syncing Tool

Sink is a lightweight, zero-configuration peer-to-peer file synchronization tool designed for local area networks (LANs). It automatically discovers other devices on your network running Sink and allows you to synchronize folders with minimal setup.

## Features

- **Automatic Discovery**: Uses Zeroconf to find other devices on the LAN.
- **Peer-to-Peer**: Directly syncs files between devices without a central server.
- **Secure Trust Mechanism**: Interactive "trust" system to approve connections from new devices.
- **Conflict Handling**: Automatically detects file conflicts and stores versions in a `.sink_conflicts/` directory.
- **Ignore Patterns**: Supports `.sinkignore` files (similar to `.gitignore`) to skip unwanted files.
- **Real-time Monitoring**: Automatically watches for file changes and syncs them to trusted peers.

## Installation

1. Ensure you have Python 3.12 or newer installed.
2. Clone the repository:
   ```bash
   git clone git@github.com:defyingdemonprogram/sink-sync-tool.git
   cd sink
   ```
3. Install dependencies in your virtual environment:
   ```bash
   pip install -r requirements.txt
   ```

## Usage

### Starting the Sync Server

Run the `main.py` script, optionally specifying the directory you want to synchronize (defaults to `./sync`):

```bash
python main.py /path/to/your/sync/folder
```

### Interactive Shell Commands

Once started, Sink provides an interactive prompt for managing connections:

- `devices`: List all discovered, trusted, and pending devices.
- `trust <device_id>`: Request to sync with a discovered device or approve an incoming trust request.

### Ignoring Files

Create a `.sinkignore` file in the project root to exclude specific files or patterns from syncing. Example:

```text
# Ignore python cache
__pycache__/
*.pyc

# Ignore temp files
.DS_Store
*.tmp
```

## How It Works

1. **Discovery**: Each device broadcasts its presence using Zeroconf (`_internallan._tcp.local.`).
2. **Authentication**: When you "trust" a device, a request is sent to that device's HTTP server. Once both sides approve, they exchange metadata and begin syncing.
3. **Synchronization**:
   - On startup, a full folder snapshot is compared with peers.
   - While running, Sink polls for filesystem changes every second.
   - Files are transferred via POST requests with metadata headers.
4. **Resolution**: If a file is modified locally and remotely before sync, Sink moves both versions to a `.sink_conflicts` folder with timestamps and device names.

## Development

Sink is built using:
- `requests`: For peer-to-peer HTTP communication.
- `zeroconf`: For service discovery.
- `cryptography`: For generating self-signed certificates (future SSL support).

## License

MIT

### References
- [ssl - Python Docs](https://docs.python.org/3/library/ssl.html)
- [socket - Python Docs](https://docs.python.org/3/library/socket.html)
- [fnmatch - Python Docs](https://docs.python.org/3/library/fnmatch.html)
- [requests - Docs](https://requests.readthedocs.io/en/latest/)
- [zeroconf - Pypi](https://pypi.org/project/zeroconf/)
