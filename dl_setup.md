# Deep Learning Setup

Following [this tutorial](https://towardsdatascience.com/setting-up-your-pc-workstation-for-deep-learning-tensorflow-and-pytorch-windows-9099b96035cb) on how to enable CUDA for the PC. 

There is a Tensorflow setup also in this tutorial, but focusing on Pytorch for now.

Note that is recommends (like everyone else) to use Conda (Miniconda)

## Setup

1. Activate the target conda environment
2. Run the following: `conda install pytorch torchvision cudatoolkit -c pytorch`
    * Note: you may not actually need to install `cudatoolkit`. The following enables all GPU setup: `!conda install -c fastai -c pytorch fastai`

**Reminder** to set `always_yes` to true (or use the `--yes` flag) if installing packages using conda directly in Jupyter notebook otherwise it won't work

## Validation

Ensure library is loaded (need to update to show fastai too)
```
# First, import PyTorch
import torch

# Check PyTorch version
torch.__version__
```

Check if CUDA is available
```
# Instant validation
# True - Success!!!
# False - Something is wrong!!!
torch.cuda.is_available()
```

GPU Information
```
# Get information about GPU
device_cnt = torch.cuda.device_count()
cur_device = torch.cuda.current_device()
device_name = torch.cuda.get_device_name(cur_device)

print(f"Number of CUDA-enabled GPUs: {device_cnt}\nCurrent Device ID: {cur_device}\nCurrent Device Name: {device_name}")
```