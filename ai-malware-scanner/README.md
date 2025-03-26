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
```

## Usage

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

## Model Training

The ML model is based on a neural network trained on a large dataset of benign and malicious files. The model uses static features extracted from PE files to make predictions.

To train your own model, see the training scripts in the [malware-detection](https://github.com/fabiocaiulo8/malware-detection) repository.

## Credits

This project incorporates code and methodologies from:

- [nebula](https://github.com/dtrizna/nebula): For static analysis components
- [malware-detection](https://github.com/fabiocaiulo8/malware-detection): For machine learning model architecture

## License

MIT
