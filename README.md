# AI Malware Scanner

An advanced malware detection system that combines static analysis, machine learning, and sandboxing to identify malicious files. This project integrates components from [nebula](https://github.com/dtrizna/nebula) and [malware-detection](https://github.com/fabiocaiulo8/malware-detection).

## Features

- **Comprehensive Static Analysis**: Extract features from PE files using LIEF
- **Deep Learning Model**: Advanced neural network model for malware classification
- **Basic Dynamic Analysis**: Lightweight process monitoring for suspicious behavior
- **Multithreaded Scanning**: Fast parallel scanning of multiple files
- **System-wide Scanning**: Ability to scan all drives and system directories
- **Detailed Reporting**: Comprehensive reports in JSON format

## Installation

```bash
# Clone the repository
git clone https://github.com/username/ai-malware-scanner.git
cd ai-malware-scanner

# Install dependencies
pip install -r requirements.txt
```

## Requirements

- Python 3.8+
- LIEF for PE file analysis
- PyTorch for the ML model
- Other dependencies in requirements.txt:
  - numpy
  - scikit-learn
  - tqdm
  - lief
  - python-magic
  - psutil

## Configuration

Create a `.env` file in the project root:

```
# Optional: Cuckoo sandbox configuration
CUCKOO_URL=http://your-cuckoo-server:8090

# VirusTotal API key for threat intelligence
VIRUSTOTAL_API_KEY=your_api_key_here
```

### Getting a VirusTotal API Key

1. Create a free account at [VirusTotal](https://www.virustotal.com/gui/join-us)
2. Once logged in, go to your profile and find your API key
3. Copy the API key and add it to your `.env` file
4. The scanner will now query VirusTotal for additional threat information when scanning files

## Usage

### Starting the API Server

Before using the web interface, you need to start the API server:

```bash
# Start the API server
python -m api.server
```

The server will start at http://127.0.0.1:8001. Keep this terminal open while using the web interface.

### Using the Web Interface

After starting the API server, open the frontend/index.html file in your browser to access the web interface.

### Command Line Interface

```bash
# Scan a single file
python -m scanner.full_scan -f /path/to/file.exe

# Scan a directory recursively
python -m scanner.full_scan -d /path/to/directory -r

# Perform a deep scan (all file types)
python -m scanner.full_scan -d /path/to/directory -r --deep

# Use dynamic analysis for suspicious files
python -m scanner.full_scan -f /path/to/file.exe --dynamic

# Save results to a file
python -m scanner.full_scan -d /path/to/directory -o results.json

# Perform a full system scan
python -m scanner.full_scan --full-system
```

### Python API

```python
from scanner.full_scan import MalwareScanner

# Initialize the scanner
scanner = MalwareScanner(
    use_sandbox=False,
    max_workers=10,
    deep_scan=False
)

# Scan a file
result = scanner.scan_file('/path/to/file.exe')
print(f"Malicious: {result.get('is_malicious', False)}")

# Scan a directory
results = scanner.scan_directory('/path/to/directory', recursive=True)
print(f"Found {results['stats']['malicious_files']} malicious files")

# Save results
scanner.save_results('scan_results.json')
```

## Troubleshooting

### Missing Log Files
If log files aren't being created:
1. Run the debug script to verify logging works:
   ```
   python debug_logging.py
   ```
2. Ensure you have write permissions to the project directory
3. Try running the scanner with administrator privileges
4. The logs should be created in one of these locations:
   - `{project_root}/logs/`
   - `{user_home}/.ai-malware-scanner/logs/`

### Scanning Errors for JDK and Large Installers
Large installer files like JDK can cause issues with the scanner:
1. Skip these files using the `--max-size` parameter:
   ```
   python -m scanner.full_scan -d C:\Downloads --max-size 50
   ```
2. Exclude directories containing installers:
   ```
   python -m scanner.full_scan -d C:\ --exclude C:\Program Files\Java
   ```

### Performance Issues
If scanning is too slow or consuming too many resources:
1. Reduce the number of parallel scans:
   ```
   python -m scanner.full_scan -d C:\ --threads 4
   ```
2. Disable deep scanning:
   ```
   python -m scanner.full_scan -d C:\ --deep false
   ```

### "Failed to Fetch" When Scanning Files
If you receive a "Failed to fetch" error when uploading files:

1. Make sure the backend API server is running:
   ```
   python -m api.server
   ```
2. Verify the API_URL in frontend/index.html matches your server address (default: http://127.0.0.1:8000)
3. Check your browser console for detailed error messages
4. If using a virtual environment, ensure all dependencies are installed there
5. If you're behind a corporate firewall or proxy, it might be blocking the connections

## Model Training

The ML model is based on a neural network trained on a large dataset of benign and malicious files. The model uses static features extracted from PE files to make predictions.

To train your own model, see the training scripts in the [malware-detection](https://github.com/fabiocaiulo8/malware-detection) repository.

## Credits

This project incorporates code and methodologies from:

- [nebula](https://github.com/dtrizna/nebula): For static analysis components
- [malware-detection](https://github.com/fabiocaiulo8/malware-detection): For machine learning model architecture

## Youtube Link: https://www.youtube.com/watch?v=OfcpCgvVO4E
## License

Apache License 2.0
