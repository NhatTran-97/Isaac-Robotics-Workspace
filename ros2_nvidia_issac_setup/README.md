## System Prerequisites

For a local setup, use a machine with:
- Ubuntu 22.04 or Ubuntu 24.04
- NVIDIA RTX 3050 or better
- More than 6 GB GPU VRAM

If you are using Windows or macOS and do not have a compatible local Linux + NVIDIA setup, you can use a cloud GPU service such as [vast.ai](https://vast.ai/).

## Docker Installation

This section explains how to install Docker so you can run the bootcamp containers.
For Ubuntu, a script is provided for automatic installation.
For Windows, follow the WSL + Docker Desktop steps.
For Vast.ai Ubuntu Desktop VM, Docker is already pre-installed.



### Windows 11/10 (WSL + Docker Desktop)

1. Install WSL (Ubuntu 24.04) in PowerShell (as Administrator):

```powershell
wsl --install -d Ubuntu-24.04
```

## Vast.ai Setup (Ubuntu 22.04 Desktop VM with GPU)

If you do not have a compatible local Linux + NVIDIA GPU, you can rent a cloud GPU using [Vast.ai](https://vast.ai/) and use the pre-configured Ubuntu Desktop VM template (GUI + Docker already installed).

### Step 1: Create a Vast.ai Account

Sign up at Vast.ai and add credits.

### Step 2: Choose Instance

- GPU: RTX 3060 / 3090 / A5000 or better
- VRAM: Minimum 12 GB (16+ GB recommended)
- Disk: >= 50 GB

### Step 3: Select Template

- Select **Ubuntu Desktop (VM)**
- This template includes GUI, NVIDIA drivers, and Docker pre-installed

### Step 4: Access Desktop via VNC (No SSH Required)

- Open Vast.ai dashboard
- Click your running instance
- Click **Open**
- This launches the Ubuntu Desktop in your browser (VNC/Web Desktop)

### Step 5: Verify GPU inside Desktop Terminal

```bash
nvidia-smi
```

**Note:** Docker is already installed in the Ubuntu Desktop VM template. No installation needed.

## Download Workshop Docker Image

Run the following command in the terminal of the environment where you will run the workshop:
- Local Ubuntu PC terminal, OR
- Vast.ai Ubuntu Desktop VM terminal (inside the VNC desktop)

```bash
docker pull therobocademy/ros2_nvidia_workshop:latest
```

Important: If you are using Vast.ai, open the Ubuntu Desktop (VNC), then open Terminal and run the `docker pull` command there. Do not run it on your local PC if you plan to run the container inside the Vast.ai VM.

## Start Docker Compose (GUI Enabled)

The compose file supports Linux and Windows (WSL) GUI forwarding.

### Ubuntu 22.04 / 24.04 (X11)

From your host terminal:

```bash
git clone https://github.com/therobocademy/ros2_nvidia_isaac_bootcamp.git

cd ros2_nvidia_isaac_bootcamp

xhost +local:docker

export DISPLAY=${DISPLAY:-:0}
export DOCKER_NETWORK_MODE=host

docker compose up workshop
docker exec -it isaac-sim bash

```

### Vast.ai Ubuntu Desktop VM (VNC Desktop)

Open Terminal inside the Vast.ai Ubuntu Desktop (in browser VNC), then run:

```bash
git clone https://github.com/therobocademy/ros2_nvidia_isaac_bootcamp.git

cd ros2_nvidia_isaac_bootcamp

xhost +local:docker

export DISPLAY=${DISPLAY:-:0}
export DOCKER_NETWORK_MODE=host

docker compose up workshop
docker exec -it isaac-sim bash
```

### Windows 11/10 (WSL2 + Docker Desktop)

1. Start an X server on Windows (VcXsrv/X410) with access control disabled (or allow your WSL IP).
2. Run these commands in Ubuntu WSL terminal:

```bash
git clone https://github.com/therobocademy/ros2_nvidia_isaac_bootcamp.git

cd ros2_nvidia_isaac_bootcamp

unset DISPLAY
unset DOCKER_NETWORK_MODE

docker compose up workshop
docker exec -it isaac-sim bash
```

Notes for Windows:
- Compose defaults `DISPLAY` to `host.docker.internal:0.0` when `DISPLAY` is unset.
- Start Docker Desktop before `docker compose up`.
- Keep WSL integration enabled for your Ubuntu distro.

Then launch your GUI app inside the container (for example, `rviz2` or Isaac Sim command used in the workshop).
