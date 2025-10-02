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
## requirements.txt
```bash
colorama
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
- IP address
- Port
- Timeouts

## **Core Python Modules (Standard Library)**

| Module         | Purpose |
|----------------|--------|
| `socket`       | Create TCP connections for reverse shell (client/server). |
| `subprocess`   | Run shell commands on the client (victim) side. |
| `os`           | For file path operations, navigating directories, etc. |
| `time`         | For implementing reconnection delays. |
| `sys`          | For handling system-level interactions (like exiting cleanly). |
| `threading`    | To handle receiving/sending simultaneously (optional). |
| `base64`       | For encoding/decoding file data or messages. |
| `logging`      | To implement logging of activities. |
| `platform`     | For getting OS information (could be useful to identify the host). |

## **Optional Third-Party Modules**

If you want to add color, encryption, or better UX:

| Module         | Purpose |
|----------------|--------|
| [`colorama`](https://pypi.org/project/colorama/)   | Print colored banners/messages in terminal (cross-platform). |
| [`cryptography`](https://cryptography.io/en/latest/) | Encrypt communication (e.g., AES or Fernet encryption of payloads). |
| [`pyfiglet`](https://pypi.org/project/pyfiglet/)    | Fancy ASCII banners for `utils.py` (optional aesthetic). |
| [`rich`](https://pypi.org/project/rich/)            | For more advanced CLI output and logging (optional). |

To install the third-party modules:
```bash
pip install colorama cryptography pyfiglet rich
```

## Example `requirements.txt`
```txt
colorama
cryptography
pyfiglet
rich
```

## Bonus Module Ideas (for extensions)

| Module         | Use Case |
|----------------|----------|
| `hashlib`      | File integrity checking (MD5/SHA256 checks for uploads/downloads). |
| `json`         | Structured messaging between client and server (instead of raw strings). |
| `select`       | For non-blocking sockets and better scalability. |
| `ssl`          | Wrap socket with SSL/TLS (for encrypted shell). |

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
