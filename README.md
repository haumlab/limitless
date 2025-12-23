# Limitless: Universal AI Game Agent

Limitless is a high-performance, vision-only foundation model agent designed to learn and play any commercial video game. It uses a ResNet50 backbone to map raw gameplay pixels directly to virtual controller actions.

## 🚀 Features

- **Universal Simulator**: A Gymnasium-compatible environment that captures any game window and sends inputs via a Virtual Xbox 360 Controller.
- **Vision-Only Generalist**: No game-specific IDs or embeddings. The model learns pure visual-to-action mapping, allowing for zero-shot potential across different games.
- **Turbo Training**: Optimized training pipeline featuring:
  - **RAM Caching**: Pre-loads datasets into system memory for zero-latency access.
  - **GPU Transforms**: All image augmentations and normalization happen on the CUDA cores.
  - **Mixed Precision**: Support for FP16 and BF16 for maximum throughput.
- **Robust Extractor**: Automatically extracts controller states from gameplay videos, even with semi-transparent overlays (e.g., Cuphead, Ready or Not).

## 🛠️ Installation

### 1. Prerequisites
- **Python 3.10+**
- **ViGEmBus Driver**: Required for virtual controller support. [Download here](https://github.com/ViGEm/ViGEmBus/releases).
- **NVIDIA GPU**: Recommended for high-speed training and inference.

### 2. Setup
```powershell
# Clone the repository
git clone https://github.com/haumlab/limitless
cd ai_player

# Install dependencies
pip install -r requirements.txt
```

## 🎮 Usage

### 1. Extract Data
Extract actions from your own gameplay videos. Place your controller templates in the `templates/` folder.
```powershell
python main.py extract --input "path/to/videos" --output "dataset/my_game"
```

### 2. Train the Model
Train the "Limitless" model on one or multiple datasets.
```powershell
python main.py train --dataset dataset/readyornot --dataset dataset/cuphead
```

### 3. Run the Agent
Run the trained agent on a live game window.
```powershell
python main.py run --window "Cuphead" --model limitless_latest.pth
```

### 4. Test on Video
Visualize the model's predictions on a video file before running it live.
```powershell
python main.py test-video --video "gameplay.mp4" --model limitless_latest.pth
```

## 🧠 Technical Architecture

- **Backbone**: ResNet50 (Pre-trained on ImageNet).
- **Policy Heads**: 
  - **Buttons**: Multi-label classification (Sigmoid + BCEWithLogitsLoss).
  - **Sticks**: Regression (MSELoss) for LX, LY, RX, RY axes.
- **Input**: 224x224 RGB frames.
- **Output**: 14 Buttons, 4 Stick Axes.

## ⚠️ Safety & Isolation
Limitless is designed to be non-intrusive:
- It uses a **Virtual Gamepad** (vgamepad) and does not touch your physical mouse or keyboard.
- It captures screen data via `mss` and does not inject code into game processes.

## 📜 License
MIT
