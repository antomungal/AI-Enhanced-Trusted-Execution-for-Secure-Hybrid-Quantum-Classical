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
