# Custom HTTP Forward Proxy Server (Python)

A custom-built HTTP forward proxy server implemented in Python, designed to demonstrate core systems and networking concepts such as TCP socket programming, concurrency, HTTP request parsing, forwarding, logging, and rule-based traffic filtering.
This project was built as a systems / networking project, focusing on correctness, modularity, and extensibility rather than relying on existing proxy libraries.

-- Features
✅ TCP-based HTTP proxy server
✅ Concurrent handling of multiple clients (thread pool)
✅ HTTP request parsing (method, host, headers, body)
✅ Forwarding of client requests to destination servers
✅ Streaming of server responses back to clients
✅ Domain/IP-based blocking via configuration file
✅ Detailed request logging
✅ Configurable server parameters (JSON config)
🔹 Optional / extensible support for:
HTTPS tunneling (CONNECT)
Caching
Authentication

-- Project Structure
proxy-project/
├── proxy_server.py        # Main proxy server (entry point)
├── http_parser.py         # HTTP request parsing logic
├── filter_manager.py     # Domain/IP filtering logic
├── config_loader.py      # Loads JSON-based configuration
├── logger.py              # Logging utilities
├── web_interface.py      # Optional monitoring / interface layer
│
├── config/
│   ├── proxy_config.json # Server configuration
│   └── blocked_domains.txt
│
├── logs/
│   └── proxy.log          # Generated at runtime
│
├── README.md

🚀 How to Run
1. Prerequisites
Python 3.8+
No external dependencies (uses Python standard library)
2. Start the Proxy Server
python3 proxy_server.py
Expected output:
[INFO] Proxy server listening on 127.0.0.1:8888
🧪 Testing the Proxy
Basic HTTP Test
curl -x http://localhost:8888 http://example.org

HEAD Request
curl -x http://localhost:8888 -I http://example.org

Blocking Test
Add example.org to blocked_domains.txt and run:
curl -x http://localhost:8888 http://example.org
Expected response:
HTTP/1.1 403 Forbidden

Concurrent Requests Test
for i in {1..10}; do
  curl -x http://localhost:8888 http://example.org &
done
wait

-- Request Flow (Data Path)
Client
  ↓
TCP Connection
  ↓
HTTP Request Parsing
  ↓
Filtering (Domain/IP)
  ├── Blocked → 403 Forbidden
  └── Allowed
        ↓
   Forward Request to Server
        ↓
   Stream Response Back
        ↓
       Logging
