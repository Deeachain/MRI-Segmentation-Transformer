The framework is provided to perform segmentation on medical images. It establishes all functionality needed to operate on 3D images with a patch-based architecture.

Network architectures including 3d u-net, non-local neural network, attention u-net are proposed.

Models trained on NAKO dataset

## Installation

use pip3 (with a venv)

    pip3 install -e .

if it fails consider

    pip3 install -e . --user
    
## Usage

For training use

    nohup python3 -u train.py > file_out 2> file_err &
    
For prediction use

    nohup python3 -u evaluate.py > file_out 2> file_err &
