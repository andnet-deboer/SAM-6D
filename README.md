# SAM-6D Setup

This guide explains how to replicate the **SAM-6D** environment using a custom Docker image and a configured codebase.

---

## Prerequisites

Ensure the following are installed on the server:

- `git`
- `pip`
- `docker`
- NVIDIA drivers + NVIDIA Container Toolkit (for `--gpus all` support)

---

## 1. Setup Code & Environment

### Clone the Repository

Clone the fork containing the fixed configuration files and scripts:

```bash
git clone https://github.com/andnet-deboer/SAM-6D.git
cd SAM-6D
```

---

### Download Docker Environment

The custom Docker image (including BlenderProc and system dependencies) is hosted on Google Drive.

#### Install `gdown`

```bash
pip install gdown
```

#### Download the Docker Image

```bash
gdown https://drive.google.com/uc?id=YOUR_FILE_ID -O sam6d_env.tar
```

#### Load the Image into Docker

```bash
docker load -i sam6d_env.tar
```

---

## 2. Download Model Weights

Model weights are excluded from Git and the Docker image to save space. Download them directly into the expected directories.

---

### Download SAM (Base Version)

```bash
mkdir -p Instance_Segmentation_Model/checkpoints/segment-anything
wget -P Instance_Segmentation_Model/checkpoints/segment-anything/ \
  https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth
```

---

### Download DINOv2

```bash
mkdir -p Instance_Segmentation_Model/checkpoints/dinov2
wget -P Instance_Segmentation_Model/checkpoints/dinov2/ \
  https://dl.fbaipublicfiles.com/dinov2/dinov2_vitl14/dinov2_vitl14_pretrain.pth
```

---

## 3. Run the Docker Container

Launch the container with GPU support and mount the repository so code and weights are accessible inside the container.

```bash
docker run --gpus all -it \
  -v $(pwd):/workspace/SAM-6D \
  --name sam6d_server \
  sam6d-custom:v1 \
  /bin/bash
```

---

## 4. Run the Demo

Once inside the container shell:

```bash
cd /workspace/SAM-6D
```

### Export Required Paths

```bash
export CAD_PATH=$(pwd)/Data/Example/obj_000005.ply
export RGB_PATH=$(pwd)/Data/Example/rgb.png
export DEPTH_PATH=$(pwd)/Data/Example/depth.png
export CAMERA_PATH=$(pwd)/Data/Example/camera.json
export OUTPUT_DIR=$(pwd)/Data/Example/outputs
```

### Run the Demo Script

```bash
chmod +x demo.sh
./demo.sh
```

---

## Verification

If successful, the pipeline will:

1. Render template views using Blender (logs will show **"Rendering..."**)
2. Run instance segmentation using **SAM** and **DINOv2**
3. Perform pose estimation
4. Save results to:

```text
Data/Example/outputs/
```

---
