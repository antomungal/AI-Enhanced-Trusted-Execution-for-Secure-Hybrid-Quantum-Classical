
## 2. Hardware Requirements

Edge device

Raspberry Pi 5 (8 GB RAM)

Discrete TPM 2.0 module (SPI/I²C), exposed as /dev/tpmrm0

Gigabit Ethernet connection to the backend (recommended for stable RTT)

Backend server

Linux x86_64 host (e.g. Ubuntu 22.04) or equivalent

8–12 CPU cores, ≥ 16 GB RAM

Python 3.11 or 3.12

Network connectivity to the Raspberry Pi in a low-latency LAN

The experiments were executed with a TPM-equipped Raspberry Pi 5 as trusted edge and a modest x86_64 server as quantum backend.

## 3. Software Requirements
### 3.1. System packages (edge / Raspberry Pi)

On Raspberry Pi OS (Bookworm, 64-bit):

sudo apt update
sudo apt install -y \
  python3 python3-pip python3-venv \
  tpm2-tools tpm2-tss \
  build-essential pkg-config \
  libssl-dev


tpm2-tools ≥ 5.7 (Debian/Ubuntu ship 5.7.x as of 2024–2025). 
GitHub
+2
UbuntuUpdates
+2

## 3.2. System packages (backend / Linux server)

On Ubuntu 22.04 or similar:

sudo apt update
sudo apt install -y \
  python3 python3-pip python3-venv \
  build-essential cmake ninja-build \
  libssl-dev libcurl4-openssl-dev \
  tpm2-tools tpm2-tss

### 3.3. Open Quantum Safe stack

On the backend and, if desired, on the edge:

[liboqs] version 0.15.0 or later 
Open Quantum Safe
+2
Open Quantum Safe
+2

oqs-provider version 0.10.0 for OpenSSL 3.x 
Open Quantum Safe

Follow the official OQS instructions to build and install OpenSSL 3 with the OQS provider:

liboqs home and documentation: 
Open Quantum Safe
+1

OQS project homepage (release notes for liboqs 0.15.0 and oqs-provider 0.10.0): 
Open Quantum Safe


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


The repository includes example oqs-openssl-*.cnf files showing how to load the OQS provider and select ML-KEM and ML-DSA in TLS 1.3 cipher suites.


## 4. Python Environment

We provide a requirements.txt file (see below) that pins library versions known to work with the experiments.

Create a virtual environment (recommended):

python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
