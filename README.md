#  FinoDSP - Real-Time Multiband Audio Engine & AI Calibrator

> **Status:** Closed-source commercial project. Currently in final preparations for release on the Steam Store.

FinoDSP is a high-performance, real-time multiband audio processing engine written from scratch in C++. At its core, it is a zero-latency harmonic exciter and spatializer with 29 tweakable parameters. To complement the manual DSP engine, it features an optional AI-driven convenience layer that automatically calibrates the processor based on standard AutoEQ headphone profiles.

![FinoDSP](https://github.com/user-attachments/assets/962f565d-9969-42a9-9b2e-1cc633cbd356)

##  The Core: Real-Time DSP Engine (C++)

The heart of FinoDSP is a highly optimized audio thread built to process live system audio without causing buffer underruns or CPU spikes. 
* **Lock-free Audio Routing:** Utilizes `PortAudio` to capture system audio via virtual cables, processing it block-by-block and outputting it directly to the hardware DAC with zero perceivable latency.
* **Granular Multiband Control:** The engine splits the audio into 4 distinct frequency bands (Sub, Bass, Mid, High). Each band features 7 independent parameters: *Gain, Drive, Mix, Character, Saturation, Drive Sensitivity, and Mix Sensitivity*, plus a global *Space* parameter (29 parameters in total).
* **Extreme Efficiency:** The entire DSP pipeline, alongside the graphical interface, consumes an average of only **up to 2.0%** of CPU (Zen 3 Ryzen 5)  on standard consumer hardware.
* **Hardware-Accelerated UI:** Custom graphical interface built with `Dear ImGui` and `GLFW`, featuring a retro-hardware aesthetic, real-time VU meters, and custom control widgets.
* **Commercial DRM:** Fully integrated with the `Steamworks C++ SDK` for license verification.

##  The Convenience Layer: AI Parameter Prediction (Python -> ONNX)

While audiophiles can manually tweak all 29 parameters, finding the perfect synergy for a specific headphone model can be daunting. To solve this, a custom neural network was built to act as an "AI Sound Engineer."

* **Advanced Architecture:** The model utilizes an **AST (Audio Spectrogram Transformer)** backbone to extract high-level acoustic features, combined with an **LSTM** for sequential context, and **FiLM (Feature-wise Linear Modulation)** layers to condition the network based on the target frequency deltas.
* **The Task:** It reads standard AutoEQ profiles (.txt) and instantly predicts the optimal combination of the 29 DSP parameters to flatten the headphone's frequency response while adding musical harmonic excitement.
* **Quantization & Deployment:** Trained in `PyTorch`, the final model was exported to `ONNX` and heavily quantized. This allows the sophisticated Transformer/LSTM architecture to run inference purely on the CPU in a fraction of a second via the ONNX Runtime C++ API, without requiring a dedicated GPU.

## Tech Stack

**Engine / Runtime:**
* C++17 & CMake 3.18
* PortAudio (Audio I/O)
* Dear ImGui + GLFW + OpenGL3 (UI)
* ONNX Runtime (C++ API for Inference) 1.23.2
* Steamworks C++ SDK
* NSIS (Installer Generation)

**Deep Learning / Training:**
* Python 3.13.3 & PyTorch 2.9.0
* AST (Audio Spectrogram Transformer) + LSTM + FiLM
* NumPy / Pandas
* ONNX

---
*Developed by cck000 - 2026*
