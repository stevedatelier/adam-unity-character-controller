# Adam Unity Character Controller

> An early Unity prototype exploring articulated character control through touch and language — one of the earliest foundations behind [Adam](https://adam10.com), our ongoing exploration into interactive characters and physical intelligence.

Read the full project write-up: [adam10.com/introducing-adam-10](https://adam10.com/introducing-adam-10)

---

<!-- BANNER IMAGE -->
<!-- Replace with project banner: ![Banner](assets/images/banner.png) -->

---

## Overview

Adam Unity Character Controller is a research prototype built to explore how an articulated 3D character can be controlled through direct touch and natural language input. It reflects an early stage of Adam's development and contains prototype systems, workflows, and design decisions that helped shape later versions.

This release is shared for **educational purposes**, experimentation, and community exploration.

---

<!-- DEMO GIF OR VIDEO PREVIEW -->
<!-- Replace with demo: ![Demo](assets/images/demo.gif) -->

---

## What's Included

| Component | Description |
|---|---|
| Unity Project Files | Complete project, ready to open |
| Character Controller | Touch-driven articulated character system |
| Touch Interaction Prototype | Early finger-tracking and body-contact input |
| Animation & Articulation Setup | Rigged character with layered animation |
| Sample Scene | Fully lit and rendered reference scene |
| Project Structure & Reference Files | Documented layout for exploration |
| Video Tutorials | Coming soon |

---

## Getting Started

### Requirements

- Unity **2021.3 LTS** or later (URP)
- The **Assets** folder (distributed separately — see below)

### Installation

**1. Clone this repository**

```bash
git clone https://github.com/stevedatelier/adam-unity-character-controller.git
```

**2. Download the Assets folder**

The character models, textures, and scene assets are distributed separately due to file size. Download and place the `Assets/` folder in the root of the cloned repo:

> **[Download Assets from Google Drive](https://drive.google.com/drive/folders/1tja8iXRrmTxw8pPwiovO4kLRCUTuHR6M?usp=sharing)**

Your folder structure should look like this:

```
adam-unity-character-controller/
├── Assets/          ← place downloaded folder here
├── Packages/
├── ProjectSettings/
└── README.md
```

**3. Open in Unity**

Open the project folder in Unity Hub. Allow Unity to import and compile on first launch.

---

<!-- SCENE SCREENSHOT -->
<!-- Replace with Unity scene screenshot: ![Scene](assets/images/scene.png) -->

---

<!-- INSPECTOR / CONTROLLER SCREENSHOT -->
<!-- Replace with inspector screenshot: ![Inspector](assets/images/inspector.png) -->

---

## Project Structure

```
Assets/
├── Characters/          # Character mesh, FBX, and animations
├── ExampleAssets/       # Materials, props, and environment pieces
├── Editor/              # Custom editor tooling
└── ...

ProjectSettings/         # Unity project configuration (URP, physics, input)
Packages/                # Package manifest and lock file
```

---

## Credits

- Character model by **3DZipGuy**
- Additional assets belong to their respective owners and are used solely for demonstration purposes

---

## About Adam

The current release, **Adam 1.0**, is an AI-powered interactive character that can be controlled through touch, voice commands, and chat in real time.

To experience the latest version: [adam10.com](https://adam10.com)

---

## License

See [LICENSE](LICENSE) for details. Character and third-party assets remain the property of their respective owners.
