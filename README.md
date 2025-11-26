**single automated Bash script** that installs **Docker (docker.io)** or **Docker CE** on **Kali Linux**, including:

✔ docker.io install (default Kali repo)
✔ docker-ce install (official Docker repo)
✔ Adds current user to docker group
✔ Enables Docker service
✔ Detects architecture
✔ Works on fresh Kali

✅ **Kali Linux Docker Install Script (All-in-One)**

Save as: `install_docker_kali.sh`

```bash
#!/bin/bash

set -e

echo "========================================="
echo "  Docker Installer for Kali Linux"
echo "  Supports: docker.io and docker-ce"
echo "========================================="

echo ""
echo "[1] Installing required dependencies..."
sudo apt update -y
sudo apt install -y ca-certificates curl gnupg lsb-release

echo ""
echo "Choose Docker installation type:"
echo "1) docker.io  (Kali/Debian repo)"
echo "2) docker-ce (Official Docker repository)"
read -p "Enter option [1/2]: " choice

if [ "$choice" == "1" ]; then
    echo ""
    echo "=============================="
    echo " Installing docker.io package "
    echo "=============================="
    sudo apt install -y docker.io

elif [ "$choice" == "2" ]; then
    echo ""
    echo "=============================="
    echo " Installing docker-ce package "
    echo "=============================="

    ARCH=$(dpkg --print-architecture)
    echo "Using architecture: $ARCH"
    echo ""

    sudo install -m 0755 -d /etc/apt/keyrings

    curl -fsSL https://download.docker.com/linux/debian/gpg | \
        sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

    echo "deb [arch=${ARCH} signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/debian bookworm stable" | \
        sudo tee /etc/apt/sources.list.d/docker.list

    sudo apt update -y
    sudo apt install -y docker-ce docker-ce-cli containerd.io
else
    echo "Invalid choice! Exiting."
    exit 1
fi

echo ""
echo "[+] Enabling Docker service..."
sudo systemctl enable docker --now

echo ""
echo "[+] Adding user '$USER' to docker group (no sudo required)..."
sudo usermod -aG docker $USER

echo ""
echo "===================================================="
echo "Docker installation finished!"
echo "→ Log out and back in to use Docker without sudo."
echo "→ Test with: docker run hello-world"
echo "===================================================="
```

▶️ How to Run

```bash
chmod +x install_docker_kali.sh
sudo ./install_docker_kali.sh
```
