# Keylogger

A simple Python script that records keystrokes and periodically sends the collected logs to a specified email address (SMTP). Useful for educational purposes related to automation, security testing, and demonstrating how keyloggers work in controlled environments.

IMPORTANT: Use only on your own devices or with the explicit, written consent of the owner. This tool is for educational purposes.

## Features
- **Keystroke logging**: captures characters and special keys (e.g., space, enter, function keys).
- **Log buffering**: the current log is accumulated in memory.
- **Periodic email report**: automatically sends collected logs every `N` seconds to the specified address (Gmail SMTP).
- **Configurable interval**: adjustable time between reports.
- **Runs in the background**: the keyboard listener runs until the process ends.

## Requirements
- Python 3.x
- A system with keyboard and internet access (Linux recommended; `pynput` also works on other systems, but additional permissions/environment configuration may be required)
- `pynput` library
- Email account with SMTP access (e.g., Gmail; for Gmail, an app password is recommended)

## Installation

Clone the repository and move into the project directory:

```bash
git clone <REPOSITORY_URL>
cd keylogger
```

Install dependencies (only `pynput` is required):

```bash
pip install pynput
```

Optionally, use a virtual environment (`python -m venv .venv && source .venv/bin/activate`).

### Using a virtual environment (optional)
A virtual environment isolates this project's Python packages from your system-wide Python. This prevents version conflicts and keeps dependencies tidy.

Create and activate a venv (Linux/macOS):

```bash
python -m venv .venv
source .venv/bin/activate
```

Create and activate on Windows (PowerShell):

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

On Windows (Command Prompt):

```bat
python -m venv .venv
.venv\Scripts\activate.bat
```

Upgrade `pip` inside the venv (recommended):

```bash
python -m pip install --upgrade pip
```

When you are done, deactivate the venv:

```bash
deactivate
```

## Configuration

The startup script is `zlogger.py`. Set the parameters there:

```startLine:endLine:/home/kali-ninja/Documents/projects/keylogger/zlogger.py
#!/usr/bin/env python3
import keylogger

my_keylogger = keylogger.Keylogger(120, "johndoe@email.com", "johndoe")
my_keylogger.start()
```

- **Interval (seconds)**: the first argument, e.g., `120` sends a report every 2 minutes.
- **Email**: the address from which and to which logs will be sent (in the example, sender and recipient are the same).
- **Password**: the email account password. For Gmail, use an “app password” (with 2FA enabled). Traditional “less secure apps” are currently blocked by Google.

The default SMTP in `keylogger.py` is the Gmail server: `smtp.gmail.com:587` with `STARTTLS`.

## Usage

Run the startup script:

```bash
python3 zlogger.py
```

The script will start listening for keystrokes and every `N` seconds will send an email with the collected log to the configured address.

### Example

```bash
python3 zlogger.py
```

After 120 seconds (by default) you will receive an email with content similar to:

```
Keylogger started
H e l l o  W o r l d  Key.enter  t e s t
```

Special keys (e.g., space, enter) are represented in the log as key names surrounded by spaces.

### Programmatic usage (import the class)

If you want to use the keylogger from your own script:

```python
from keylogger import Keylogger

kl = Keylogger(time_interval=60, email="twoj_email@example.com", password="HASLO_APP")
kl.start()
```

## Arguments and behavior
- **`time_interval` (int)**: reporting interval in seconds; after each report, the log buffer is cleared.
- **`email` (str)**: sender/recipient address; the built-in mail function uses the same address as both sender and recipient.
- **`password` (str)**: SMTP password (for Gmail: app password).

## Security and permissions
- Some desktop environments (e.g., Wayland) may restrict keyboard capture; in such cases, use X11 or appropriate permissions/run in a TTY console.
- Ensure internet access (required for sending emails) and correct SMTP configuration.
- Store passwords securely. Consider using environment variables or a secrets manager instead of hardcoding passwords in a file.

## Troubleshooting
- **No emails arriving**: check the Spam folder; ensure credentials are correct and you are using an app password (Gmail).
- **`pynput` error**: ensure `pip install pynput` completed successfully; some systems may require system libraries (e.g., X11 dependencies).
- **Insufficient permissions to listen**: in restricted environments (e.g., Wayland) run under X11 or from a system terminal.
- **No internet**: the script cannot send reports without an active network connection.
- **Reports too infrequent/frequent**: adjust `time_interval` according to your needs and SMTP rate limits.

## Notes
- By default, sender and recipient are the same email address.
- The log is sent in the email body as plain text, and the local buffer is cleared after sending.
- The script runs until manually interrupted (e.g., `Ctrl+C`) or the process exits.

## License
Educational project. If you plan public distribution, add an appropriate license file (e.g., MIT).

## About the course
This script was created as part of the [Learn Python & Ethical Hacking from Scratch](https://www.udemy.com/course/learn-python-and-ethical-hacking-from-scratch/) course (Udemy) and is intended for learning Python and basic automation in the context of cybersecurity.
