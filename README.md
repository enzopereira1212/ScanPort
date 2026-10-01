# Amaterasu

<h3 align="center">A fast, multithreaded TCP port scanner written in Python.</h3>

<p align="center">
<img width="559" height="179" alt="Amaterasu Port Scanner" src="https://github.com/user-attachments/assets/77b167f6-5bf6-4587-aec5-f6ee146c5adc" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Author-Enzo%20Pereira-blue?style=flat-square">
  <img src="https://img.shields.io/badge/Open%20Source-Yes-darkgreen?style=flat-square">
  <img src="https://img.shields.io/badge/Maintained-Yes-purple?style=flat-square">
  <img src="https://img.shields.io/badge/Written%20In-Python-darkcyan?style=flat-square">
</p>

---

## Credits

This project was created as a learning project based on a port scanner tutorial by **HackStation**.

The original tutorial was used as a reference while learning and implementing concepts such as Python sockets, TCP port scanning, multithreading, queues and network latency.

The Amaterasu repository may contain modifications, improvements and experiments made during my own development.

**Original creator:** HackStation
**Original tutorial:** [HackStation on YouTube](https://www.youtube.com/@CanalHackStation)

---

<h3><p align="center">Disclaimer</p></h3>

<i>Any actions and or activities related to <b>Amaterasu Port Scanner</b> are solely your responsibility. This tool is intended for educational purposes and authorized security testing only. Only scan systems that you own or have explicit permission to test.</i>

<i>The author and contributors are not responsible for any misuse of this software or for any consequences resulting from unauthorized scanning activities.</i>

---

## Overview

Amaterasu performs TCP connect scans against a target host using a configurable thread pool. Before scanning, it pings the target to measure round-trip time and automatically adjusts the per-port timeout, eliminating the need to guess a safe value. Results are printed to the terminal in real time and can optionally be saved to a file.

The default scan covers the top 1,000 most common ports. A flag is available to extend coverage to the top 10,000 ports.

---

## Requirements

* Python 3.10 or higher
* [pythonping](https://pypi.org/project/pythonping/)

Install the dependency:

```bash
pip install pythonping
```

> **Note:** `pythonping` requires raw socket privileges to send ICMP packets. On Linux and macOS, run the script with `sudo`. On Windows, run your terminal as Administrator.

---

## Installation

Clone or download the repository and enter the directory:

```bash
git clone https://github.com/SEU-USUARIO/amaterasu.git
cd amaterasu
```

Install dependencies:

```bash
pip install pythonping
```

---

## Usage

```bash
sudo python portscan.py -t <target> [options]
```

### Arguments

| Argument    | Short | Required | Default           | Description                                |
| ----------- | ----- | -------- | ----------------- | ------------------------------------------ |
| `--target`  | `-t`  | Yes      | —                 | Target IP address or hostname              |
| `--timeout` | —     | No       | Auto (ping-based) | Per-port timeout in milliseconds           |
| `--threads` | —     | No       | 30                | Number of concurrent worker threads        |
| `--output`  | `-o`  | No       | None              | File path to save open ports               |
| `--top-10k` | —     | No       | False             | Scan top 10,000 ports instead of top 1,000 |

---

## Examples

### Basic scan against an IP address

```bash
sudo python portscan.py -t 192.168.1.1
```

Pings the target, auto-sets the timeout, then scans the top 1,000 ports using 30 threads.

---

### Scan a domain

```bash
sudo python portscan.py -t scanme.nmap.org
```

---

### Set a manual timeout and thread count

```bash
sudo python portscan.py -t 10.0.0.5 --timeout 300 --threads 50
```

Useful when the target does not respond to ICMP and auto-detection fails, or when you want finer control over scan speed vs. accuracy.

---

### Scan top 10,000 ports

```bash
sudo python portscan.py -t 192.168.1.1 --top-10k
```

Increases port coverage significantly. Expect longer execution time.

---

### Save results to a file

```bash
sudo python portscan.py -t 192.168.1.1 -o results.txt
```

Each open port is written to `results.txt` as it is discovered, one port per line.

---

### Full example combining all options

```bash
sudo python portscan.py -t 10.10.10.5 --timeout 200 --threads 100 --top-10k -o open_ports.txt
```

---

## How It Works

1. **Ping phase** — The script sends two ICMP echo requests to the target and computes the average RTT. The per-port timeout is set to `RTT + 80 ms`. If a manual `--timeout` value is provided, the ping phase is skipped.

2. **Scan phase** — A queue is populated with the selected port list (top 1k or top 10k). Worker threads pull ports from the queue and attempt a TCP `connect()` to each one. A port is marked open if `connect_ex()` returns `0`.

3. **Output phase** — Open ports are printed to the terminal in real time with color highlighting. After all threads finish, the tool prints a summary with total open ports, total scanned ports, and elapsed time.

---

## Learning Purpose

Amaterasu was developed as a practical learning project to study:

* Python
* TCP/IP networking
* Socket programming
* TCP port scanning
* Multithreading
* Thread pools
* Queues
* Network latency
* Command-line arguments
* File handling

The project is also an opportunity to experiment with performance improvements, timeout handling and different scanning configurations.

---

## Output Example

```text
[ PINGING ON DOMAIN... ]

Domain  = 192.168.1.1
Timeout = 142.5 ms
Threads = 30

[ RESULTS ]

[+] 22    -> Open
[+] 80    -> Open
[+] 443   -> Open

[#] 3 Open ports
[i] 1000 Scanned Ports
[i] Execution Time: 4.821 seconds
```

---

## Acknowledgements

Special thanks to **HackStation** for the tutorial that served as the starting point for this project and for providing the reference used during the learning process.

Please check out the original creator:

📺 **HackStation on YouTube:**
https://www.youtube.com/@CanalHackStation
