# Deploy SAM3D on RTX 5090

The RTX 5090 uses CUDA 12.8, which causes compatibility issues with some SAM3D dependencies.
Most libraries work out of the box, but the following components must be manually adjusted.

## Libraries Requiring Special Handling
- PyTorch
- PyTorch3D
- kaolin
- xFormers

---

## SAM3D Repository
👉 https://github.com/facebookresearch/sam-3d-objects


## Setup Python Environment

⚠️ **You need to download the SAM3D code and weights before proceeding.**

### 1. Create conda environment and install some default SAM3D dependencies

```bash
# create sam3d-objects environment
mamba env create -f environments/default.yml
mamba activate sam3d-objects

export PIP_EXTRA_INDEX_URL="https://pypi.ngc.nvidia.com https://download.pytorch.org/whl/cu121"

pip install -e '.[dev]'

# The above is the part that does not need to be modified.
```

### 2. Reinstall PyTorch 2.7.0 (CUDA 12.8)

```bash
# Next, reinstall torch. The version being installed here is torch 2.7.0.
# Please ignore the error that occurs.
pip install torch==2.7.0 torchvision==0.22.0 torchaudio==2.7.0 --index-url https://download.pytorch.org/whl/cu128
```

###  3. Install requriements.p3d.txt (including flash_attn and pytorch3d)

```bash
pip install flash_attn==2.8.3

# ⚠️⚠️⚠️This .whl document of pytorch3d is placed at the end. 
pip install pytorch3d-0.7.9-cp311-cp311-linux_x86_64.whl

# You can also compile pytorch3d directly from the source code. 👉 https://github.com/facebookresearch/pytorch3d
```

### 4. Install Inference Dependencies (including kaolin, gsplat, seaborn and gradio)
```bash
# You can install them separately
pip install gsplat @ git+https://github.com/nerfstudio-project/gsplat.git@2323de5905d5e90e035f792fe65bad0fedd413e7
pip install seaborn==0.13.2
pip install gradio==5.49.0
# Pay attention to choosing the appropriate version. Here is torch 2.7.0 and cuda 12.8
pip install kaolin==0.18.0 -f https://nvidia-kaolin.s3.us-east-2.amazonaws.com/torch-2.7.0_cu128.html
```

### 5. Reinstall xFormers (compatible version)
```bash
# install xFormers
pip install xformers==0.0.30
# You can also choose other appropriate version on https://github.com/facebookresearch/xformers/releases

```
## PyTorch3D Wheel Download
👉 [Download pytorch3d-0.7.9-cp311-cp311-linux_x86_64.whl]([https://github.com/HRyoimiya/Deploy-SAM3D-on-RTX-5090/releases/download/pytorch3d-whl/pytorch3d-0.7.9-cp311-cp311-linux_x86_64.whl](https://github.com/HRyoimiya/Deploy-SAM3D-on-RTX-5090/releases/download/pytorch3d-whl/pytorch3d-0.7.9-cp311-cp311-linux_x86_64.whl))


## Verify Installation
```bash
python demo.py
```
