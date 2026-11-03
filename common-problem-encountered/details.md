## Common Problems faced while using scripts on Server

1. Even though the model itself (`/home/models/gpt-oss-120b`) is downloaded to your server locally, this particular model is quantized (compressed) using a newer format called mxfp4.

When the transformers library detects this format, it tries to automatically connect to Hugging Face (huggingface.co) to download the necessary optimization kernels (kernels-community/gpt-oss-triton-kernels).


PROBLEM faced when internet connection was not present


2. When script uses all GPUs in the cluster (e.g. all 4 GPUs memory shared in Optimus) even though 1 GPU is fixed in script.

 Why it failed:
In the script, I had put the import torch line at the very top of the file, before setting os.environ["CUDA_VISIBLE_DEVICES"]. When torch is imported in Python, it immediately initializes the CUDA backend and locks in its view of all the GPUs. Because it was imported before the restriction was applied, it saw all 5 GPUs and decided to just use Physical GPU 0 by default.