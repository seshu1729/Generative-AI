If you are use INTEL GPU:

pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/xpu

If you are using Nvidia GPU:

pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu126

Install the required libraries. Uninstall the libraries if you face any issues based on the error logs.


====================
Some more Library packages needed:

pip install huggingface_hub[hf_xet] transformers diffusers accelerate
