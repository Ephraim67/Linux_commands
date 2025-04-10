# Webdeves Ethical Hacking Final Project (Team Green)

## Topic: Reverse Shell/Backdoor with Python
### Overview:
- **Objective**: Simulate a reverse shell to understand unauthorized access.
- **Ethical & Legal Disclaimer**: Only run this in **isolated virtual environments**. Understand the implications and avoid unauthorized use.

### Project Structure:
```
reverse_shell_simulator/
│
├── client.py               # Reverse shell client (attacked machine)
├── server.py               # Attacker's listening server
├── README.md               # Instructions, setup, and ethical considerations
└── demo_files/             # Files for testing reverse shell (e.g. text files)
```

# Module Breakdown
**reverse_shell/client.py**
- Auto-reconnect loop
- Command execution
- File download/upload (optional enhancement)

**reverse_shell/server.py**
- Accepts one client at a time
- Sends shell commands
- Receives command output

**reverse_shell/utils.py**
- Print colored banners
- Logging
- Encode/decode helpers

**reverse_shell/config.py**
-IP address
Port
Timeouts

### Key Files:
1. **client.py**: Reverse shell client script.
2. **server.py**: Attacker's listening server.
3. **README.md**: Instructions, setup, and ethical considerations.

### Steps:
1. Set up the environment (install dependencies, create test files).
2. **Run the Attacker's Server (`server.py`)**.
3. **Run the Victim's Client (`client.py`)** to simulate the reverse shell connection.
4. Interact with the reverse shell by sending commands from the server.
5. **Cleanup**: Remove scripts and restore systems.

### Learning Outcomes:
- Understand reverse shell attack mechanics.
- Learn remote command execution techniques.
- Implement basic detection and defense mechanisms (firewalls, etc.).
