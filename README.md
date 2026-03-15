🇪🇸 [Leer en Español](README.es.md)
# 🔍 Web Data Exposure Scanner (WDES)

<p align="center">
  <img src="https://img.shields.io/badge/version-1.1.0-blue.svg" alt="Version">
  <img src="https://img.shields.io/badge/python-3.8+-green.svg" alt="Python">
  <img src="https://img.shields.io/badge/license-MIT-orange.svg" alt="License">
  <img src="https://img.shields.io/badge/platform-linux%20%7C%20windows%20%7C%20macos-lightgrey.svg" alt="Platform">
  <img src="https://img.shields.io/badge/tor-compatible-purple.svg" alt="Tor Compatible">
</p>

**WDES** is an OSINT (Open Source Intelligence) tool designed to detect sensitive data exposed on websites. It identifies emails, identity documents, phone numbers, and potentially sensitive files that may be publicly accessible.

## ⚠️ Legal Disclaimer & Ethical Use

This tool is designed **ONLY** for:

- ✅ Authorized security audits
- ✅ Assessment of your own infrastructure
- ✅ Research with explicit permission from the site owner
- ✅ Authorized Bug Bounty programs
- ✅ Educational purposes in controlled environments

**Unauthorized use may violate local and international laws.** The author is not responsible for any misuse of this tool.

## 🚀 Features

- 📧 **Email detection** exposed on web pages
- 🪪 **Identity document detection** with patterns for multiple countries:
  - 🇺🇾 Uruguay (Cédula de Identidad)
  - 🇦🇷 Argentina (DNI)
  - 🇧🇷 Brazil (CPF)
  - 🇨🇱 Chile (RUT)
  - 🇲🇽 Mexico (CURP)
  - 🇨🇴 Colombia (Cédula de Ciudadanía)
  - 🇵🇪 Peru (DNI)
  - 🇪🇸 Spain (DNI/NIE)
  - 🌐 Customizable generic pattern
- 📞 **Phone number detection**
- 📁 **Sensitive file identification** (.pdf, .doc, .xls, .sql, .bak, etc.)
- 🧅 **Tor network support** (optional anonymous connection)
- 🔄 **Recursive crawling** with depth control
- 🚀 **Multi-threading** for fast scans
- 📊 **JSON and TXT reports**
- 🎨 **Colorful** and user-friendly interface
- 💻 **Interactive and CLI modes**

## 📋 Requirements

- Python 3.8 or higher
- Dependencies:
  ```
  requests
  beautifulsoup4
  colorama
  tqdm
  PySocks (optional, only for Tor)
  ```
- **To use Tor:** Tor service running on port 9050

## 🔧 Installation

### Option 1: Quick Install

```bash
# Clone the repository
git clone https://github.com/eduardoit/web-data-exposure-scanner.git
cd web-data-exposure-scanner

# Install dependencies
pip install -r requirements.txt

# Run
python scanner.py
```

### Option 2: Manual Install

```bash
# Install dependencies manually
pip install requests beautifulsoup4 colorama tqdm

# Optional: Tor support
pip install PySocks

# Download the script
wget https://raw.githubusercontent.com/eduardoit/web-data-exposure-scanner/main/scanner.py

# Grant execution permissions (Linux/Mac)
chmod +x scanner.py

# Run
python scanner.py
```

### Configure Tor (Optional)

```bash
# Ubuntu/Debian
sudo apt install tor
sudo systemctl start tor

# macOS (with Homebrew)
brew install tor
brew services start tor

# Verify Tor is running
curl --socks5-hostname localhost:9050 https://check.torproject.org/api/ip
```

## 📖 Usage

### Interactive Mode (Recommended for beginners)

```bash
python scanner.py
```

The interactive mode will guide you step by step:

1. **Select connection mode** (Direct or Tor)
2. Enter the target site URL
3. Select document patterns to search for
4. Configure advanced options (optional)
5. The scan will start automatically

### Command Line Mode

```bash
# Basic scan
python scanner.py -u example.com

# Anonymous scan through Tor
python scanner.py -u example.com --tor

# With multiple document patterns
python scanner.py -u example.com -p uruguay,argentina,brasil

# Custom configuration
python scanner.py -u example.com -d 5 -m 200 -t 10

# Save report
python scanner.py -u example.com -o report.json

# List all available patterns
python scanner.py --list-patterns
```

### Command Line Options

| Option | Description | Default |
|--------|-------------|---------|
| `-u, --url` | Target site URL | - |
| `-p, --patterns` | Document patterns (comma-separated) | uruguay |
| `-d, --depth` | Maximum crawling depth | 3 |
| `-m, --pages` | Maximum pages to scan | 100 |
| `-t, --threads` | Concurrent threads | 5 |
| `-o, --output` | Output file (JSON) | - |
| `--tor` | Use Tor network for anonymity | False |
| `--no-ssl` | Disable SSL verification | False |
| `-q, --quiet` | Quiet mode | False |
| `--list-patterns` | List available patterns | - |

## 🧅 Using with Tor

### Advantages
- Your real IP is not logged by the target site
- Useful for OSINT when you don't want to leave a trace
- Automatic IP rotation
- Bypasses IP-based rate-limiting

### Considerations
- **Reduced speed**: Scans will be slower
- **Blocks**: Some sites block Tor traffic
- **CAPTCHAs**: Cloudflare and others may display CAPTCHAs
- **Reduced threads**: Automatically limited to 3 to avoid overloading the Tor network

### Tor Connection Flow Example

```
┌─ Connection Mode ─────────────────────────────────────────────────────┐
│                                                                       │
│  1. 🌐 Direct connection (faster)                                    │
│  2. 🧅 Connect through Tor (anonymous)                               │
│                                                                       │
│ Select connection mode [1/2]: 2                                      │
│                                                                       │
│  ⏳ Verifying Tor connection...                                      │
│  ✓ Connected to Tor. Exit IP: 185.220.101.xxx                        │
│                                                                       │
│  ⚠️  Note: Scanning will be slower through Tor                       │
│  ⚠️  Some sites may block Tor traffic                                │
└───────────────────────────────────────────────────────────────────────┘
```

## 📊 Output Example

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                         SCAN SUMMARY                                         ║
╚══════════════════════════════════════════════════════════════════════════════╝

  Target: https://example.com
  Date: 2024-01-15T10:30:00
  URLs scanned: 87

  FINDINGS:
  ────────────────────────────────────────
  📧 Unique emails found: 23
      • contact@example.com
      • admin@example.com
      • ...
  
  🪪 Cédula de Identidad (Uruguay): 5
      • 1.234.567-8
      • 2.345.678-9
      • ...
  
  📁 Interesting files: 12
      • https://example.com/docs/report.pdf
      • https://example.com/data/users.xlsx
      • ...
```

## 📁 JSON Report Structure

```json
{
  "target": "https://example.com",
  "scan_date": "2024-01-15T10:30:00",
  "summary": {
    "total_urls_scanned": 87,
    "unique_emails_found": 23,
    "unique_documents_found": 5,
    "unique_phones_found": 8,
    "interesting_files_found": 12
  },
  "findings": {
    "emails": ["contact@example.com", "..."],
    "documents": {
      "Cédula de Identidad (Uruguay)": ["1.234.567-8", "..."]
    },
    "phones": ["+598 99 123 456", "..."],
    "interesting_files": ["https://example.com/doc.pdf", "..."]
  }
}
```

## 🔐 Legitimate Use Cases

### 1. Audit your own organization
```bash
python scanner.py -u mycompany.com -p uruguay -d 5 -m 500 -o audit_mycompany.json
```

### 2. Pre-launch verification
```bash
python scanner.py -u staging.myapp.com -p generic -o pre_launch_check.json
```

### 3. Periodic exposure monitoring
```bash
# Add to cron for weekly scans
0 0 * * 0 python /path/to/scanner.py -u mycompany.com -o /logs/scan_$(date +\%Y\%m\%d).json -q
```

## 🛠️ Customization

### Adding new document patterns

Edit the `ID_PATTERNS` variable in the script:

```python
ID_PATTERNS = {
    "my_country": {
        "name": "My Country Document",
        "pattern": r'\b\d{8}-[A-Z]\b',  # Your regex here
        "example": "12345678-A",
        "description": "8 digits + hyphen + letter"
    },
    # ... other patterns
}
```

### Customizing file extensions

Edit the `INTERESTING_EXTENSIONS` variable:

```python
INTERESTING_EXTENSIONS = [
    '.pdf', '.doc', '.docx', '.xls', '.xlsx', 
    '.csv', '.txt', '.json', '.xml', '.sql', 
    '.bak', '.log', '.conf', '.env',
    # Add more extensions as needed
]
```

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a branch for your feature (`git checkout -b feature/new-feature`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/new-feature`)
5. Open a Pull Request

### Ideas for contributions

- [ ] Add more document patterns for other countries
- [ ] Implement credit card number detection (with caution)
- [ ] Add CSV/Excel export
- [ ] Create a web interface
- [ ] Add breach database API integration
- [ ] Implement smart rate limiting
- [ ] Support for authentication (cookies, tokens)

## 📜 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- To the security community for sharing knowledge
- To everyone who contributes to making the internet safer

## 📧 Contact

If you find bugs or have suggestions, please open an [Issue](https://github.com/eduardoit/web-data-exposure-scanner/issues).

---

<p align="center">
  <strong>Made by <a href="https://github.com/eduardoit">eduardoit</a></strong>
</p>

<p align="center">
  <em>Remember: With great power comes great responsibility. Use this tool ethically.</em>
</p>
