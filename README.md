# Neural World Model for Embodied AI Robotics

**Project Title**: Action-Conditioned Latent Dynamics for Future Frame Prediction  
**Author**: Sayed Mahdi & Jayesh Nikam 

**Status**: Completed Prototype  
**Date**: February 2025  

---

## Objective

The goal of this project is to build a **neural world model** that learns compact latent representations of robot interaction scenes and predicts **future visual observations conditioned on actions**.

This type of model is a foundational component for:
- Model-based reinforcement learning
- Planning and control
- Embodied AI and robotics simulation

The project uses the **RoboNet dataset** and focuses on **open-loop rollout prediction**.

---

## Approach

The system is trained in two stages:

### 1. Variational Autoencoder (VAE)
- Learns compact latent representations of individual frames
- Convolutional encoder / decoder
- Trained with **MSE + KL divergence**
- Fine-tuned with **LPIPS perceptual loss** to improve sharpness

### 2. Latent Dynamics Model
- GRU-based recurrent model
- Predicts next latent state conditioned on:
  - Current latent state
  - Continuous robot action (4-D)
- Autoregressive rollout for up to **20 future steps**

### 3. Evaluation
- Open-loop prediction (no ground-truth correction)
- Latent predictions decoded back to pixel space
- Metrics computed over **20-step rollouts**

---

## Results Summary

Evaluation performed on **8 validation samples**, each with **20-step open-loop rollouts**, using the sharpened VAE.

### Quantitative Metrics

| Sample | Avg MSE | Avg PSNR (dB) | Avg SSIM | Notes |
|------|--------|---------------|----------|------|
| 0 | 0.0267 | 15.91 | 0.4019 | Moderate quality |
| 1 | 0.0196 | 17.09 | 0.5654 | **Best SSIM – strong structure** |
| 2 | 0.0245 | 16.11 | 0.2965 | Some detail loss |
| 3 | 0.0557 | 12.54 | 0.1787 | Strong drift |
| 4 | 0.0198 | 17.15 | 0.5415 | Stable rollout |
| **Average** | **~0.022** | **~16.5** | **~0.45** | Acceptable early prototype |

### 🎥 Qualitative Observations

- **Short-term predictions (5–10 steps)**:
  - Sharp reconstructions
  - Correct motion direction
  - High structural similarity

- **Long-term predictions (15–20 steps)**:
  - Gradual blur and drift
  - Accumulated latent error
  - Common limitation of deterministic dynamics models

Best examples are saved in:

