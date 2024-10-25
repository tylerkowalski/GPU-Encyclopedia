# TensorRT
[Click here to go back to main](README.md)

**What is TensorRT?**
  - SDK for accelerating deep learning inference
  - Has parsers to convert trained models into optimized runtime engines
  - Complements training frameworks (e.g PyTorch), instead focusing on taking an already-trained neural network and running it as quickly as possible on Nvidia hardware

## Capabilities of TensorRT

## Misc. 
- Multi-Instance GPU (MIG)
  - allows for user-directed partitioning of a single GPU into multiple smaller ones
  - If there is low GPU utilization, TensorRT can use MIG to get higher throughput with near no latency impact
  - On Nvidia GPUS with Ampere architecture or later
- Triton Inference Server
  - Library to run an optimized server for NN inference
- DALI
  - Library that provides high-performance operations for image, audio, and video processing
  - TensorRT inference can be added as a custom operator within a DALI pipeline
- Torch-TRT
  - Pytorch -> TensorRT compiler
  - Compiler takes subgraphs of the original PyTorch graph and accelerates them with TensorRT, while the rest is done natively by PyTorch
  - Result is still a PyTorch module
- TensorFlow-Quantization Toolkit
  - Provides utilities to train/run TF2 Keras models as reduced precision
- PyTorch Quantization Toolkit
  - Provides facilities for training PyTorch models in reduced precision
- Exists a subset of TensorRT that is certified for NVIDIA DRIVE SOCs
  

  
## GTC Talk Notes
[Click here to go to notes on GTC talks on TensorRT](TensorRT_GTC.md)