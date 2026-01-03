# Vectis 2.0 - Distribution Files

## Download

**Latest Release:** [v2.0](https://github.com/Casperrules/vectis-deployment/releases/tag/v2.0)

### Available Downloads

- **vectis-v2.0-macos-arm64.tar.gz** (113MB) - TAR.GZ archive for macOS
- **vectis-v2.0-macos-arm64.zip** (113MB) - ZIP archive for macOS

### Quick Start

```bash
# Download and extract (choose one)
tar -xzf vectis-v2.0-macos-arm64.tar.gz
# OR
unzip vectis-v2.0-macos-arm64.zip

# Run the server
cd vectis-v2.0-macos-arm64
./vectis serve

# Test it
curl http://localhost:3000/health
```

### What's Included

- **vectis** (90MB) - Single binary executable
- **model.onnx** (86MB) - AI embedding model
- **vocab.txt** (226KB) - Model vocabulary
- **USER_GUIDE.md** - Complete usage documentation
- **LICENSE** - MIT License
- **VERSION** - Version information

### System Requirements

- macOS (Apple Silicon - M1/M2/M3)
- 4GB RAM minimum (8GB+ recommended)
- 500MB disk space

---

**Note:** Distribution archives are hosted on GitHub Releases due to their size (>100MB).
The source code repository contains only documentation and build instructions.
