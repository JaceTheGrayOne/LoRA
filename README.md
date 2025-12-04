How to:
1. Set up Stable Diffusion using Stability Matrix
2. Create and Use a character LoRA for Stable Diffusion WebUI Forge (SDXL + Juggernaut XL)
3. Use a trained character LoRA to produce predictable controllable results

### **Workflow Summary**
1. Prepare 15–30 high-quality images.
2. Caption images with wd14-tagger.
3. Train LoRA using Kohya_ss on SDXL Base 1.0.
4. Install Python.
5. Install Stability Matrix and run it.
6. Install WebUI Forge inside Stability Matrix.
7. Download Juggernaut XL through Stability Matrix.
8. Import your LoRA through the Models tab.
9. Launch Forge from Stability Matrix.
10. Configure ADetailer, ControlNet, and IP-Adapters.
11. Generate images using your LoRA.
12. Adjust weights and prompt settings until results match your desired fidelity.

### **Image Set Prep**
This will require 10+ high-quality images of the character.

Image Requirements
	 - 1024×1024 (or larger) images when possible
	 - High-quality PNG or JPG images 
	 - Avoid blurry or distorted images  
	 - Avoid images with filters, AI edits, or heavy compression

Image Variety
	 - 3+ close-ups of the face  
	 - 3+ half-body shots  
	 - 3+ full-body images  
	 - Images that vary in lighting and angle will increase training quality

Image Backgrounds
	 - For ~50% of the images try to use simple, clean backgrounds  
	 - For ~30% of the images try to use varied backgrounds  
	 - Avoid images containing unique iconic landmarks  
	 - Do not use identical backgrounds repeatedly  
	 - Avoid colored lighting reflecting on the face (neon, RGB LEDs)

### **Captioning**
This is optional but I do recommend doing it.
Captions define what is identity vs. what is clothing or environment.

Captioning Tools
	 - wd14-tagger (recommended)  
	 - OmegaTagger  
	 - BLIP captions (must be manually edited)

Captioning Procedure
1. Run each image through wd14-tagger.
2. Remove all expression tags such as “smiling,” “laughing,” “surprised,” etc.
3. Remove all specific objects or clothing if they are not core identity traits.
4. Add missing useful descriptors manually (hair length, hair color, age category, etc.).
5. Add a unique trigger word to every caption:  
    Example: `ohwx` or `sks_person`.
6. Ensure captions remain 6–20 words long.
##### Caption Examples
Bad: `woman smiling`
Good: `ohwx 1girl blonde_hair denim_jacket outdoor sunny looking_at_viewer`

### **Training the LoRA using Kohya_ss**
Training Base Model: SDXL Base 1.0

Recommended Settings
	 - Resolution: `1024, 1024`  
	 - Batch Size: `1` for 8GB VRAM, `2–4` for 24GB+ VRAM  
	 - Rank (Dimension): `32` (use 64 or 128 for more detail)  
	 - Alpha: `Rank ÷ 2` (16, 32, or 64)  
	 - UNet Learning Rate: `0.0001–0.0002`  
	 - Text Encoder Learning Rate: `0.00005`  
	 - Optimizer: `Adafactor` or `AdamW`  
	 - Steps:  1500–2500 total effective steps  
	(example: 20 images × 10 repeats × 10 epochs)
	 - Dropout Settings
	 - Dropout prevents overfitting.
	 - UNet Dropout: `0.15`  
	 - TE Dropout: `0.10`
These values improve generalization, pose flexibility, and prevent “pose lock.”

Training Output
Kohya_ss produces a file ending in: `your_character_name.safetensors`
This file is the LoRA.

### **Installing WebUI Forge**
Installing Python
Stability Matrix requires a modern Python installation.
1. Install Python 3.10.x from: [https://www.python.org/downloads/release/python-3100/](https://www.python.org/downloads/release/python-3100/)
2. During installation, enable: “Add Python to PATH”
3. Complete installation and restart your PC.

Installing and Launching Stability Matrix
1. Download Stability Matrix from: [https://github.com/LykosAI/StabilityMatrix](https://github.com/LykosAI/StabilityMatrix)
2. Extract the ZIP to any folder.
3. Run: `StabilityMatrix.exe`
4. When prompted, select your GPU.
5. Once launched, proceed to the Interfaces tab.

Installing WebUI Forge for Stability Matrix
1. In Stability Matrix, open the Interfaces tab.
2. Locate WebUI Forge in the list of supported interfaces.
3. Select Install.
4. Once installed, click Launch to open Forge inside your browser.

### **Installing Juggernaut XL**
1. Open Stability Matrix.
2. Go to the Models tab → Stable Diffusion Models.
3. Select Download Model.
4. Search for Juggernaut XL, Juggernaut XL v9, or Juggernaut XL X.
5. Click Download.
6. Launch WebUI Forge through Stability Matrix.
7. In Forge, select Juggernaut XL from the Checkpoint dropdown.

### **Installing the LoRA**
1. Open Stability Matrix.
2. Go to the Models tab → LoRA Models.
3. Select Add Local Model.
4. Choose your LoRA file (e.g., `your_character_name.safetensors`).
5. Launch Forge and confirm the LoRA appears in the Lora drop-down menu or the prompt autocomplete suggestions.

### **Configuring Stability Matrix for Forge**
1. Open Stability Matrix.
2. Navigate to Settings → Interfaces → WebUI Forge.
3. Confirm the following:  
     - GPU acceleration is enabled  
     - VRAM optimization settings match your GPU  
     - xFormers or memory-efficiency modes are ON if using 8GB VRAM
4. Save configuration and restart the Forge interface.

### **Using the LoRA**
1. Launch WebUI Forge from Stability Matrix.
2. In the Checkpoint dropdown, select: juggernaut XL / Juggernaut XL v9 / Juggernaut XL X
3. Load your LoRA
	Use the LoRA syntax: ```<lora:your_lora_name:0.8>```
	Start with a weight between 0.6–0.9.

##### Example Prompt
Positive prompt: `<lora:your_lora_name:0.8> photo of ohwx 1girl, red evening gown, sitting in a cafe, soft lighting`
Negative prompt: `cartoon, illustration, 3d, blurry, low quality`

### **Using ADetailer**
1. Enable ADetailer in the Forge UI.
2. Set:  
     - Detection Model: `face_yolov8n.pt`  
     - Post-processing: enabled
	 - Controlling Pose, Identity, and Consistency (Optional)
		If using ControlNet or IP-Adapters:
		 - ControlNet Weight: `0.5`  
		 - LoRA Weight: `0.8`  
		 - IP-Adapter Strength: `0.3–0.6`

### **Troubleshooting**
##### Adjusting for Clothing or Scene Drift
If the character’s identity shifts due to outfit or lighting:
Increase the identity prompt weighting: `(ohwx woman:1.1)`
##### Plastic Skin
**Cause:** overtraining or high LoRA weight  
**Fix:** reduce weight to `0.6–0.7`
##### Clothing Won’t Change
**Cause:** clothing not labeled in captions  
**Fix:** ensure clothing is explicitly tagged
##### Lighting Issues
**Cause:** insufficient lighting variety in dataset  
**Fix:** add more diverse lighting to dataset
##### Pose Limitations
**Cause:** overfitting  
**Fix:** increase dropout or reduce training epochs  
Use ControlNet OpenPose XL for pose control
