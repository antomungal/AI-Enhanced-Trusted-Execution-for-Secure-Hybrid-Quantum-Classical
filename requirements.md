# System Requirements

## 2. Hardware Requirements

### Edge Device (Trusted Platform)
- Raspberry Pi 5 (8 GB RAM)
- Discrete TPM 2.0 module (SPI/I²C), exposed as `/dev/tpmrm0`
- Gigabit Ethernet connection to the backend (recommended for stable RTT)

### Backend Server (Quantum Processing)
- Linux x86_64 host (e.g., Ubuntu 22.04 or equivalent)
- 8–12 CPU cores, ≥ 16 GB RAM
- Python 3.11 or 3.12
- Low-latency LAN connectivity to the Raspberry Pi

> The experiments were executed with a TPM-equipped Raspberry Pi 5 as trusted edge and a modest x86_64 server as quantum backend.

---

## 3. Software Requirements

### 3.1 System Packages (Edge / Raspberry Pi)

Raspberry Pi OS (Bookworm, 64-bit):

```sh
sudo apt update
sudo apt install -y \
  python3 python3-pip python3-venv \
  tpm2-tools tpm2-tss \
  build-essential pkg-config \
  libssl-dev

Note: Recommended: tpm2-tools ≥ 5.7 (Debian/Ubuntu ship 5.7.x as of 2024–2025)

### 3.2 System Packages (Backend / Linux Server)

Ubuntu 22.04 or similar:

sudo apt update
sudo apt install -y \
  python3 python3-pip python3-venv \
  build-essential cmake ninja-build \
  libssl-dev libcurl4-openssl-dev \
  tpm2-tools tpm2-tss

### 3.3 Open Quantum Safe (OQS) Stack

Required on backend and optional on edge:

liboqs ≥ 0.15.0

oqs-provider v0.10.0 (for OpenSSL 3.x)

Build instructions (summary):

# Clone and build liboqs
git clone https://github.com/open-quantum-safe/liboqs.git
cd liboqs
git checkout v0.15.0
cmake -S . -B build -DOQS_BUILD_ONLY_LIB=ON -DCMAKE_INSTALL_PREFIX=/opt/oqs
cmake --build build --target install

# Clone and build OpenSSL with OQS provider
cd ..
git clone https://github.com/open-quantum-safe/oqs-provider.git
cd oqs-provider
git checkout v0.10.0
cmake -S . -B build -DCMAKE_PREFIX_PATH=/opt/oqs -DCMAKE_INSTALL_PREFIX=/opt/oqs-openssl
cmake --build build --target install


The repository includes example oqs-openssl-*.cnf files to enable the OQS provider and configure ML-KEM / ML-DSA for TLS 1.3.

## 4. Python Environment

Use the provided requirements.txt to ensure compatible package versions.

Recommended workflow:

python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
