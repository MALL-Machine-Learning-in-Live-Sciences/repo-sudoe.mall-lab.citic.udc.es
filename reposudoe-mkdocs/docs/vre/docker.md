---
comments: true
---

# Docker Installation and Usage Guide

Docker is a platform used to develop, ship, and run applications inside containers. This tutorial will guide you through the steps to install Docker and start using it.

## Prerequisites

- A compatible operating system:
  - **macOS:** Docker Desktop requires macOS 10.15 or newer.
  - **Windows:** Docker Desktop supports Windows 10 64-bit: Pro, Enterprise, or Education (Build 16299 or later).
  - **Linux:** A modern Linux distribution, like Ubuntu, Fedora, or CentOS.

## Installation

### macOS

1. **Download Docker for Mac:**
   - Visit [Docker Desktop for Mac](https://www.docker.com/products/docker-desktop) and download the installer.

2. **Install Docker Desktop:**
   - Open the downloaded `.dmg` file and drag the Docker icon to your Applications folder.

3. **Launch Docker Desktop:**
   - Open Docker from your Applications folder. You'll need to authorize Docker with your system's password.

### Windows

1. **Download Docker for Windows:**
   - Visit [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop) and download the installer.

2. **Install Docker Desktop:**
   - Run the installer and follow the setup instructions. Ensure "Enable WSL 2 Features" is checked if prompted.

3. **Start Docker Desktop:**
   - Open Docker from your Start menu. Windows may require restarting to finalize the installation.

### Linux

1. **Update your package index:**

   ```sh
   sudo apt-get update
   ```

2. **Install prerequisites:**

   ```sh
   sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
   ```

3. **Add Docker’s official GPG key and verify:**

   ```sh
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

4. **Add Docker's stable repository:**

   ```sh
   echo "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

5. **Install Docker:**

   ```sh
   sudo apt-get update
   sudo apt-get install docker-ce docker-ce-cli containerd.io
   ```

6. **Start Docker:**

   ```sh
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

## Usage

### Verify Installation

Run the following command to check if Docker is installed correctly:

```sh
docker --version
```

You should see something like `Docker version 20.10.x, build xxxxxxx`.

### Running Your First Container

1. **Pull a Docker Image:**

   Pull the latest Ubuntu image from Docker Hub:

   ```sh
   docker pull ubuntu
   ```

2. **Run a Container:**

   Run a container from the Ubuntu image:

   ```sh
   docker run -it ubuntu bash
   ```

   This command runs the Ubuntu container in interactive mode (`-it`) and opens a bash shell inside it.

### Basic Docker Commands

- **List Running Containers:**

  ```sh
  docker ps
  ```

- **List All Containers:**

  ```sh
  docker ps -a
  ```

- **Stop a Running Container:**

  ```sh
  docker stop <container_id>
  ```

- **Remove a Container:**

  ```sh
  docker rm <container_id>
  ```

- **Remove an Image:**

  ```sh
  docker rmi <image_id>
  ```
