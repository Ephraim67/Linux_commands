# Webdeves Ethical Hacking Final Project (Team Green)

## Topic: Reverse Shell/Backdoor with Python
### Overview:
- **Objective**: Simulate a reverse shell to understand unauthorized access.
- **Ethical & Legal Disclaimer**: Only run this in **isolated virtual environments**. Understand the implications and avoid unauthorized use.

### Project Structure:
```
reverse_shell_simulator/
│
|-- utils.py
|-- config.py
├── client.py               # Reverse shell client (attacked machine)
├── server.py               # Attacker's listening server
├── README.md
|-- requirements.txt              # Instructions, setup, and ethical considerations
└── demo_files/             # Files for testing reverse shell (e.g. text files)
```

- client.py – Reverse shell (victim side)
- server.py – Command and control (attacker side)
- config.py – Central settings
- utils.py – Reusable helpers
- requirements.txt – Dependencies

## reverse_shell/config.py
```python
# config.py

# Attacker IP and Port
ATTACKER_IP = "127.0.0.1"  # Change to your VM IP if needed
ATTACKER_PORT = 4444

# Buffer and Retry
BUFFER_SIZE = 4096
RETRY_DELAY = 5  # Seconds between reconnect attempts
```

## reverse_shell/utils.py
```python
# utils.py

from colorama import init, Fore, Style
import logging

init(autoreset=True)

def print_banner():
    banner = f"""
{Fore.GREEN}
    ____  ____  ____  __  __  ____  _  _ 
   (  _ \\( ___)( ___)(  \\/  )(  _ \\( \\/ )
    )___/ )__)  )__)  )    (  )___/ \\  / 
   (__)  (____)(____)(_/\\/\\_)(__)   (__)
{Style.RESET_ALL}
"""
    print(banner)

def setup_logger():
    logging.basicConfig(
        filename='shell_activity.log',
        level=logging.INFO,
        format='%(asctime)s - %(message)s',
    )

def log_activity(msg):
    logging.info(msg)
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
